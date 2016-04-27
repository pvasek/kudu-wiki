This document describes various aspects of the environment that Azure Web Apps run under.

## Environment

Azure Web Apps sets some environment variables with information about your Web App.

### Website Environment Variables

- `WEBSITE_SITE_NAME` - The name of the site.
- `WEBSITE_SKU` - The sku of the site (Possible values: `Free`, `Shared`, `Basic`, `Standard`).
- `WEBSITE_COMPUTE_MODE` - Specifies whether website is on a **dedicated** or **shared** VM/s (Possible values: `Shared`, `Dedicated`).
- `WEBSITE_SITE_MODE` - The mode for the site (can be `Limited` for a free site, `Basic` for a shared site or empty for a standard site).
- `WEBSITE_HOSTNAME` - The Azure Website's primary host name for the site (For example: `site.azurewebsites.net`). Note that custom hostnames are not accounted for here.
- `WEBSITE_INSTANCE_ID` - The id representing the VM that the site is running on (If site runs on multiple instances, each instance will have a different id).
- `WEBSITE_NODE_DEFAULT_VERSION` - The default node version this website is using.
- `WEBSOCKET_CONCURRENT_REQUEST_LIMIT` - The limit for websocket's concurrent requests.

### WebJobs Environment Variables

[[Web-jobs#environment-settings]]

## File system

There are three main types of files that an Azure Web App can deal with

### Persisted files

This is what you can view as your web site's files. They follow a structure described [[here|File structure on azure]]. They are rooted in `d:\home`, which can also be found using the `%HOME%` environment variable.

These files are persistent, meaning that you can rely on them staying there until you do something to change them. Also, they are shared between all instances of your site (when you scale it up to multiple instances). Internally, the way this works is that they are stored in Azure Storage instead of living on the local file system.

Free and Shared sites get 1GB of space, Basic sites get 10GB, and Standard sites get 50GB. See more details on the [Web App Pricing](http://www.windowsazure.com/en-us/pricing/details/web-sites/) page.


### Temporary files

A number of common Windows locations are using temporary storage on the local machine. For instance

- `%APPDATA%` points to something like `D:\local\AppData`.
- `%TMP%` goes to `D:\local\Temp`.

Unlike Persisted files, these files are not shared among site instances. Also, you cannot rely on them staying there. For instance, if you stop a site and restart it, you'll find that all of these folders get reset to their original state.

For Free and Shared sites, there is a 500MB limit for all these locations together (i.e. not per-folder). For Standard and Basic sites, the limit is very high (over 100GB).


### Machine level read-only files

The Web App is able to access many standard Windows locations like %ProgramFiles% and %windir%. These files can never be modified by the Web App.


## Restrictions

Azure Web Apps run in a sandboxed environment that allows multiple sites to safely co-exist on the same machine.

In most cases, sites are able to do most things that 'normal' web sites typically need to do. But in some scenario, you may run into something that doesn't work due to sandbox limitations. See [[Azure Web-App Sandbox]] for more details.
