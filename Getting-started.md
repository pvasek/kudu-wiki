### Prerequisites to run Kudu
We have recently created a few custom scripts to automate the setup of Kudu development dependencies
see the readme for [KuduDevSetup.cmd](https://github.com/projectkudu/kudu/tree/master/Setup) for details. The new approach has been minimally tested so the older manual instruction below are still worth reading to understand what the new script does. The script requires that [WebPI 4.5](http://www.microsoft.com/web/downloads/platform.aspx) be installed. Once you have WebPI install simply execute the script from the directory.

### Manual Setup Instructions
* Visual Studio 2012. Note that you need to run it as admin. Install/Update to the latest version of NuGet Package Manager (Tools -> Extensions and Updates).
* IIS >= 7, configured to run ASP.NET 4.0. This can be installed using [WebPI](http://www.microsoft.com/web/downloads/platform.aspx).
* Install Git (http://git-scm.com/downloads) to the default location (we expect it in ```C:\Program Files (x86)\Git\bin```).
* Install [Node](http://nodejs.org/) 0.8.x. Node is used as part of the build process for sites.

Note: If you manually install IIS, here is what needs to be enabled:

![Windows Features](http://i.imgur.com/ZdFpz.png)

### Additional prerequisites to run the functional tests

* Install the XUnit.net runner for VS 2012, which you install from Tools / Extensions & Updates
* Open your %WINDIR%\System32\inetsrv\config\applicationHost.config and look for `<applicationPoolDefaults>`. Under `<processModel>`, make sure you don't have `setProfileEnvironment="false"`. If you do, set it to true.
* Install the [URL rewrite module for IIS](http://www.iis.net/download/URLRewrite).
* Install [IISNode](http://go.microsoft.com/?linkid=9784331)
* Pretend to have the 32 bit version of Node to fool the version detection logic:
 * Copy your `\Program Files\nodejs` folder to \Program Files (x86)\nodejs.
 * Under `\Program Files (x86)\nodejs`, you need to have a folder named `0.8.2`, and containing [node.exe version 0.8.2](http://nodejs.org/dist/v0.8.2/node.exe). And then same deal for Node 0.10.5. Yes, this is kind of painful, and we need to make it easier!
* The Web and WebApplications folders need to be present under `\Program Files (x86)\MSBuild\Microsoft\VisualStudio\v10.0`. If you don't have them, you may need to copy them from a machine that has VS 2010 installed. This is needed because some of the test apps target these. **TODO**: we need to make this easier!
* Install [Mercurial](http://mercurial.selenic.com/release/windows/mercurial-2.4.2-x86.msi) to the default location (we expect it in ```C:\Program Files (x86)\Mercurial```)


### Deploying your first application
1. First clone the repository.

        git clone git://github.com/projectkudu/kudu.git

3. Open Kudu.sln as **Administrator** and build it.

4. Set **Kudu.Web** as the startup project and run!

After running Kudu.Web, you should be able to use the dashboard to manage your application. Below is a walk through on how to create and deploy your first application using git.

### Step 1
Create an application.

![Step 1](http://i.imgur.com/pScf7.png)

### Step 2
Give it a name.

![Step 2](http://i.imgur.com/xebWn.png)

### Step 3
Copy the git url and push.

![Step 3](http://i.imgur.com/vsWqb.png)

### Step 4
You're live!

![Step 4](http://i.imgur.com/zN5lf.png)