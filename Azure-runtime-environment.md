This document describes various aspects of the environment that Azure Web Sites run under.

## File system

There are three main types of files that an Azure Web Site can deal with

### Persisted files

This is what you can view as your web site's files. They follow a structure described [[here|File structure on azure]].

These files are persistent, meaning that you can rely on them staying there until you do something to change them. Also, they are shared between all instances of your site (when you scale it up to multiple instances). Internally, the way this works is that they are stored in Azure Storage instead of living on the local file system.

Free and Shared sites get 1GB of space, while Standard sites get 10GB. See more details on the [Web Sites Pricing](http://www.windowsazure.com/en-us/pricing/details/web-sites/) page.


### Temporary files

A number of common Windows locations are using temporary storage on the local machine. For instance

- `%APPDATA%` points to something like `C:\DWASFiles\Sites\mysite\AppData`.
- `%TMP%` goes to `C:\DWASFiles\Sites\mysite\Temp`.

Unlike Persisted files, these files are not shared among site instances. Also, you cannot rely on them staying there. For instance, if you stop a site and restart it, you'll find that all of these folders get reset to their original state.

For Free and Shared site, there is a 200MB limit for all these locations together (i.e. not per-folder). For Standard site, the limit is very high.


### Machine level read-only files

The web site is able to access many standard Windows locations like %ProgramFiles% and %windir%. These files can never be modified by the web site.


## Restrictions

Windows Azure runs web sites in a sandboxed environment that allows multiple sites to safely co-exist on the same machine.

In most cases, sites are able to do most things that 'normal' web sites typically need to do. But in some scenario, you may run into something that doesn't work due to sandbox limitations.

One such example is that sites cannot use the GDI API.

If you find something that doesn't work and you are not sure why, the best place to ask is the [forum](http://social.msdn.microsoft.com/Forums/en-US/home?forum=windowsazurewebsitespreview).