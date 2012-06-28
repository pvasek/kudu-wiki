### Kudu's single tenant architecture

Kudu has a somewhat unusual architecture, in the sense that it is a single-tenant rather than multi-tenant service. What this means is that each Azure site which uses git deployment has its own instance of the Kudu service, completely distinct from the Kudu service sites used for other Azure sites.

Furthermore, the Kudu site itself runs as an Azure web site, abeit a somewhat special site. The best way to look at the Kudu service is that it is a 'buddy site' to the real Azure site. The main difference is that the Kudu site is being given access to the files of the real site, allowing it to publish to it.


### Security model

The Kudu site runs in the same sandbox as the real site. This has some important implications:

- The Kudu site cannot do anything that the site itself wouldn't be able to do itself. This is very important, because as part of deploying an app, arbitrary user code can run (e.g. msbuild custom code, etc...). But being in the same sandbox as the site, the only thing it can harm is the site itself.
- It's under the same quotas as the site. That is, the CPU/RAM/Disk space used by the Kudu service is counted toward the site's quota. This makes sense, as it prevents a bad build process from using more resources than it should. In normal situations, the build process should not hurt the site quotas in any measurable way, but if it starts doing crazy things, it will get blocked in the exact same way as if the site itself tried to do those things.
