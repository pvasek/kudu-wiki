Overview
--------
By this time, we assume you have created the Azure WebSites and using its companion SCM (aka. Kudu) endpoint to do git publishing or various tasks.  This SCM endpoint can be viewed as Site Extension of the actual user main site.  It is intended for site owner and is protected by [basic authentication](https://github.com/projectkudu/kudu/wiki/Deployment-credentials).  You may not realize that you can also do much more things with Kudu Extension such as getting diagnostic dump, log stream or the DebugConsole for remote shell interaction with the actual file contents. 

Out of the box, Windows Azure provides one Site Extension (which is Kudu).   All builtin Site Extensions are installed at `d:\Program Files (x86)\SiteExtensions`.  For instance, the below is Kudu Site Extensions file layout.

    d:\
        Program Files (x86)
            SiteExtensions
                Kudu
                    extension.xml
                    1.24.12345.67
                        applicationHost.xdt
                        kudu bits...
                    1.24.34567.89-preview
                        applicationHost.xdt
                        kudu bits...
   
The `extension.xml` indicates what version to be used by default.  Good news is each site can overwrite the version by specifying `<extension>_EXTENSION_VERSION` in AppSettings.   For above example, we have 2 versions of Kudu in different [semver](http://semver.org/) folders.   The default version setting is `latest` which means using any latest version excluding preview/beta (in this case, 1.24.12345.67).   However, if you want to try beta or preview version, you may set KUDU_EXTENSION_VERSION to `beta` and it will look for latest including such.   Other available version values include `disabled` if you want to disable this extension entirely as well as `specific semver` if you only want to use specific version.
  
ApplicationHost Config Transformation 
-------------------------------------
You may wonder how Kudu or any other extensions gets setup in SCM site.   The key is the applicationHost.xdt (notice one exists for each versioning folders).   The [Xml Document Transform](http://msdn.microsoft.com/en-us/library/dd465326.aspx) file is used to transform the actual applicationHost.config for the site.  For instance, a simple site extension XDT would just add the IIS application under SCM site.


    <?xml version="1.0"?>
    <configuration xmlns:xdt="http://schemas.microsoft.com/XML-Document-Transform">
      <system.applicationHost>
        <sites>
          <site name="%XDT_SCMSITENAME%" xdt:Locator="Match(name)">
            <application path="/Dev" xdt:Locator="Match(path)" xdt:Transform="Remove" />
            <application path="/Dev" applicationPool="%XDT_APPPOOLNAME%" xdt:Transform="Insert">
              <virtualDirectory path="/" physicalPath="%XDT_EXTENSIONPATH%" />
            </application>
          </site>
        </sites>
      </system.applicationHost>
    </configuration>

There are a set of env variables being pass to XDT to assist in locating the appropriate elements and set the proper property.

* `XDT_SITENAME` is the current site name
* `XDT_SCMSITENAME` is the current scm site name
* `XDT_APPPOOLNAME` is the application pool name
* `XDT_EXTENSIONPATH` is the version specific extension path
* `HOME` is the site root path

By the way, the XDT is not limited to only <site/> sections.   It could transform other part of applicationHost.config such as introuducing a new mime type for httpCompression or adjusting httpModules.    

Private Extensions
------------------
The site owners can overwrite the existing site extensions with their own implementations or introduce a totally new set of site extensions altogether.   For demonstration, we created this [simple extension](https://github.com/projectkudu/SimpleWebSiteExtension) sample.   The step involves ...

* Upload extension bits to root `SiteExtensions` folder.   Below examples illustrate overriding Kudu extension as well as introducing new Foo extension.

    /site
    /LogFiles
    /SiteExtensions
        /Kudu
            applicationHost.xdt
            artifacts
        /Foo
            applicationHost.xdt
            artifacts

* Set AppSetting `WEBSITE_PRIVATE_EXTENSIONS` = `1`

Note: For any issue with XDT, look at `/LogFiles/Transform` log files.