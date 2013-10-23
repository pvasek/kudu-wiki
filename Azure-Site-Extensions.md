Overview
--------
By this time, we assume you have created the Azure WebSites and using its companion SCM (aka. Kudu) endpoint to do git publishing or various tasks.  This SCM endpoint can be viewed as Site Extension of the actual user main site.  It is intended for site owner and is protected by [basic authentication](https://github.com/projectkudu/kudu/wiki/Deployment-credentials).  You may not realize that you can also do much more things with Site Extension endpoint such as getting diagnostic dump, log stream or the DebugConsole for remote shell interaction with the actual file contents. 

Out of the box, Windows Azure provides one Site Extension (which is Kudu).   All builtin Site Extensions are installed at `d:\Program Files (x86)\SiteExtensions`.  For instance, the below is Kudu Site Extensions layout.

    d:\
        Program Files (x86)\
            SiteExtensions\
                Kudu\
                    extension.xml
                    1.24.12345.67\
                        applicationHost.xdt
                        kudu bits...
                    1.24.34567.89-preview\
                        applicationHost.xdt
                        kudu bits...
   


Customize Builtin Extensions
----------------------------

Private Extensions
------------------