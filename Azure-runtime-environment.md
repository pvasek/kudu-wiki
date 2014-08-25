This document describes various aspects of the environment that Azure Web Sites run under.

## File system

There are three main types of files that an Azure Web Site can deal with

### Persisted files

This is what you can view as your web site's files. They follow a structure described [[here|File structure on azure]]. They are rooted in `d:\home`, which can also be found using the `%HOME%` environment variable.

These files are persistent, meaning that you can rely on them staying there until you do something to change them. Also, they are shared between all instances of your site (when you scale it up to multiple instances). Internally, the way this works is that they are stored in Azure Storage instead of living on the local file system.

Free and Shared sites get 1GB of space, while Standard sites get 50GB. See more details on the [Web Sites Pricing](http://www.windowsazure.com/en-us/pricing/details/web-sites/) page.


### Temporary files

A number of common Windows locations are using temporary storage on the local machine. For instance

- `%APPDATA%` points to something like `C:\DWASFiles\Sites\mysite\AppData`.
- `%TMP%` goes to `C:\DWASFiles\Sites\mysite\Temp`.

Unlike Persisted files, these files are not shared among site instances. Also, you cannot rely on them staying there. For instance, if you stop a site and restart it, you'll find that all of these folders get reset to their original state.

For Free and Shared sites, there is a 200MB limit for all these locations together (i.e. not per-folder). For Standard and Basic sites, the limit is very high.


### Machine level read-only files

The web site is able to access many standard Windows locations like %ProgramFiles% and %windir%. These files can never be modified by the web site.


## Restrictions

Azure websites run in a sandboxed environment that allows multiple sites to safely co-exist on the same machine.

In most cases, sites are able to do most things that 'normal' web sites typically need to do. But in some scenario, you may run into something that doesn't work due to sandbox limitations. Some restrictions are described below.

### Sites cannot use the GDI API

For the most part, sites cannot call into GDI, which means that some libraries do not work.

Per [this thread](http://social.msdn.microsoft.com/Forums/windowsazure/en-US/b4a6eb43-0013-435f-9d11-00ee26a8d017/report-viewer-error-on-export-pdf-or-excel-from-azure-web-sites) and [this thread](http://social.msdn.microsoft.com/Forums/windowsazure/en-US/d14bc4fa-256e-4f8f-9682-432ab556f74d/report-viewer-control-fix-for-export-to-pdf-available?forum=windowsazurewebsitespreview), special support was added to make ReportViewer work on sites that run in Basic or Standard mode. However, other libraries that generate PDF files (or use GDI in other ways) might still not work.

### Sites cannot listen on sockets

Sites receive requests via IIS (or indirectly via iisnode), but they cannot start listening on sockets. So for instance if you try to listen on a custom port on localhost, it will not work.

### Final notes

Note that the product is still evolving, so it is possible that some scenarios that don't work today will work in the future.

If you find something that doesn't work and you are not sure why, the best place to ask is the [Azure Websites forum](http://social.msdn.microsoft.com/Forums/en-US/home?forum=windowsazurewebsitespreview).
