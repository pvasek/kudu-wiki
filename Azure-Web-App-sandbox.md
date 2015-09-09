Your Azure Web Apps (as well as Mobile App/Services) run in a sandbox.  This article describes the purpose of the sandbox and the restrictions that it imposes on your applications.  We have not done a great job of communicating these limitations, hence the article.  Over time we will update with corrections as well as when we update the sandbox itself.  

### Purpose of the sandbox

We run applications in a sandbox primarily to ensure they do not do damage to other applications or the underlying platform itself.  This is not an issue for customers running an application on their own machine, but is a very important living issue for a service like Azure Web Apps which has 100000's of applications running on 100000's of VMs.  With Azure Web Apps the platform VMs are configured/maintained by Microsoft; we provide this application environment for customers and the sandbox protects this environment from the application itself.  

A secondary but also important motivation for the sandbox is to meter use of the platform resources (storage, networking, compute) as well as prevent abuse.

### Definition of the sandbox

For the context of Azure Web Apps, the sandbox is a set of kernel-mode enforced restrictions on a set of processes.  Practically this set is defined as the root W3WP.EXE process which is launched to run your application, plus all the child processes launched by that root process.  So the sandbox is basically a container of a set of processes, which have a set of restrictions applied.  

**Put another way, each Azure Web App (formerly known as site) has its own sandbox, consisting of all the processing used in the app.**  

When running applications in Azure Web Apps, multiple processes may be involved in actually executing the application.  Here are some examples of languages supported in Azure Web Apps and the processes contained in the respective sandboxes:

| Language | Processes in sandbox |
| -------- | -------------------- |
| C#       | W3WP.EXE, CSC.EXE, CVTRES.EXE |
| node.js  | W3WP.EXE, NODE.EXE |
| PHP      | W3WP.EXE, PHP-CGI.EXE |
| Java     | W3WP.EXE, JAVA.EXE |

The vast majority of the limits in this article apply to the entire sandbox; that is to all the processes in the tree.  

### App Service Plans and Sandbox Limits

In the context of the sandbox, the limits which apply may depend on the type of plan in which your application runs.  For example, some limits only apply to applications running in Free/Shared modes; these limits are lifted when running your application in Basic/Standard/Premium modes.  If the plan affects the limits, it will be called out.  

### General Sandbox Restrictions

The following section describes things that are totally restricted from within the sandbox.  

#### Writing to registry

Applications cannot write to any location in the registry.  This limitation is implemented a global restriction rather than a set of ACLs.  There is no hive/key which is writable including HKCU.  

(Reading the registry, however, is permitted.  ACLs are used to restrict what keys are accessible).  

#### Access to Event Log

Applications cannot read from or write to the event log service.  However, as a workaround, applications are provided a user-mode "virtualized"” event log in which to write events.  These events end up in a file named `d:\home\logfiles\eventlog.xml`, with a limit of 1000 (rotated) entries in the file; this virtualization was done to enable better debugging of applications as well as to support applications which require (as part of their basic function) to register event sources.  As mentioned, the virtualization is done in user-mode within the application process itself using API hooking (in our case detours).  

#### Access to out-of-process COM servers

Windows Servers have a bunch of COM servers configured and available for consumption by   default; however the sandbox prevents access to all out-of-proc COM servers.  For example, an sandboxed application cannot call into WMI, or into MSIServer.  

#### Use of Console

Applications do not have access to their console.  Some console APIs are virtualized (faked) in user mode but you should consider console access a non-starter within sandbox.  

#### Win32k.sys (User32/GDI32) Restrictions

For the sake of radical attack surface area reduction, the sandbox prevents almost all of the Win32k.sys APIs from being called, which practically means that most of User32/GDI32 system calls are blocked.  For most applications this is not an issue since most Azure Web Apps do not require access to Windows UI functionality (they are web applications after all). 

However one common pattern that is affected is PDF file generation.  There are several cases of note:

##### Microsoft SQL Reporting Framework manually generating PDFs

This usage pattern does not work for Free/Shared plans due to the sandbox.  However, a small set of APIs are opened for use in Basic+ plans; these APIs are designed to enable SQL Reporting framework creation of PDFs.  So these libraries should be usable for PDF generation in Basic+.  

##### PDF generation from HTML

There are multiple libraries used to convert HTML to PDF.  Many Windows/.NET specific versions leverage IE APIs and therefore leverage User32/GDI32 extensively.  These APIs are largely blocked in the sandbox (regardless of plan) and therefore these frameworks do not work in the sandbox.  

There are some frameworks that do not leverage User32/GDI32 extensively (wkhtmltopdf, for example) and we are working on enabling these in Basic+ the same way we enabled SQL Reporting.

#### Process Enumeration/Job-Assignment

Processes within a sandbox are able to access each other via their handles and retrieve the handles using `OpenProcess()` the access is restricted to within the sandbox; a process cannot open/see other processes outside the sandbox.  

For example, if you use the System.Diagnostics.Process class to enumerate processes you will only see the processes within the sandbox.  

The sandbox itself runs in a Job controlled by the Azure Web App administrative service (the moral equivalent of WAS service in Windows).  A process within the sandbox can create a its own nested Job for child processes to occupy.  

#### Service Control Manager (SCM) Access

Applications cannot access SCM to manage/enumerate/start/stop services.  Since some legacy code does require access to SCM, these APIs are faked in user mode to keep the applications working; but these stubs are essentially not functional.  

### File System Restrictions/Considerations

Applications are highly restricted in terms of their access of the file system.  

#### Home directory access (d:\home)

Every Azure Web App has a home directory stored/backed by Azure Storage.  This network share is where applications store their content.  This directory is available for the sandbox with read/write access.  

As a convenience for our customers, the sandbox implements a dynamic symbolic link in kernel mode which maps `d:\home` to the customer home directory.  This is done to remove the need of the customer to keep referencing their own network share path when accessing the site.  No matter where the site runs, or how many sites run on a VM, each can access their home directory using `d:\home`.  

#### Local directory access (d:\local)

Every Azure Web App has a local directory which is temporary and is deleted when the run is no longer running on the VM.  This directory is a place to store temporary data for the application.  The sandbox implements a dynamic symbolic link which maps `d:\local` to point to this directory.  The application naturally has read/write access to this directory.   

#### System drive access

Applications can read the system drive as controlled by ACLs on the drive.  Applications cannot write to any location on the system drive, as implemented in the sandbox (i.e. even if ACLs allowed writing to a location on system drive, the sandbox would restrict it).  

#### Symbolic link creation

While sandboxed applications can follow/open existing symbolic links, they cannot create symbolic links (or any other reparse point) anywhere.  

### Networking Restrictions/Considerations

Applications are restricted in terms of their outbound/inbound network access.  

#### Network endpoint listening

Applications cannot listen on an arbitrary port which is exposed to the internet; the only external way to access the application is through port 80/443.  However, an application can create a listen socket which is accessible only from within the sandbox.  So two processes within a sandbox can communicate with each other using TCP/IP sockets; but not process outside the sandbox would be able to connect to the listening process within the sandbox.  See next topic.  

#### Local Address Requests

With the exception of the above case involving local listening, an application cannot connect to a local IP address; in this context local means either the loopback address (127.0.0.1) or the IP address of the VM itself.  In general when the sandbox rejects a connection for whatever reason, the resulting .NET exception looks like:

```
Exception Details: System.Net.Sockets.SocketException: An attempt was made to access a socket in a way forbidden by its access permissions 127.0.0.1:80
```

#### Private Address Requests

Applications cannot connect to private IP address as defined by IANA (https://en.wikipedia.org/wiki/Private_network).  This definition is augmented to further prevent connections to 100.* IP addresses.  

#### Use of Raw Sockets

Raw sockets (created with socket type SOCK_RAW) are not available to applications.  A side effect of this is you cannot do SYN attacks from an Azure Web App.  

#### Restricted Outgoing Ports

Regarding of address, applications cannot connect to anywhere using ports 445, 137, 138, and 139.  

#### Named Pipes

Within the sandbox, applications can create named pipes which accessible only within the sandbox (much like how listen sockets are allowed within sandbox). 

### Numerical Sandbox Limits

Given the above background information, the following are numerical limits which apply to applications in the sandbox.  Note that "unlimited" still means memory/resource limits may apply; however the sandbox does not explicitly restrict.  

#### Per-Sandbox (per-app/per-site) Numerical Limits

These limits apply to each sandbox and therefore represent the sum usage of all the processes within the given sandbox.  

| Limit name | Description | Free/Shared Limit | Basic+ Limit |
| ---------- | ----------- | ----------------- | ------------ |
| Threads | Number of threads| 512 | Unlimited |
| Processes | Number of processes | 32 | Unlimited |
| Connections | Number of bound sockets outstanding | 250 | Unlimited |
| Named Pipes | Number of named pipes | 128 | 128 |
| Listen Sockets | Number of listen sockets | 256 | 256 |

#### Cross VM Numerical Limits

These limits apply only for customers of Basic or higher plans; in other words customers running on their own dedicated VMs.  These limits are there to protect the entire VM even though one particular site may be with its limits described above.  The limits are different depending on the size of VM configured.  

| Limit name | Description | Small (A1) | Medium (A2) | Large (A3)
| ---------- | ----------- | ---------- | ----------- | ----------
| Connections | Number of connections across entire VM | 1920 | 3968 | 8064
