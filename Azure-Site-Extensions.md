Overview
--------
By this time, we assume you have created the Azure WebSites and using its companion SCM (aka. Kudu) endpoint to do git publishing or various tasks.  This SCM endpoint can be viewed as Site Extension of the actual user main site.  It is intended for site owner and is protected by [basic authentication](https://github.com/projectkudu/kudu/wiki/Deployment-credentials).  You may not realize that you can also do many more things with the Kudu Extension such as getting diagnostic dump, log stream or the DebugConsole for remote shell interaction with the actual file contents. 

Out of the box, Windows Azure provides one Site Extension (which is Kudu).   All built-in Site Extensions are installed at `d:\Program Files (x86)\SiteExtensions` path. 

    d:\
        Program Files (x86)
            SiteExtensions
                Kudu
                    extension.xml
                    1.24.12345
                        applicationHost.xdt
                        kudu bits...
                    1.24.34567-preview
                        applicationHost.xdt
                        kudu bits...
   
The `extension.xml` indicates what version is used by default.  Good news is each site can overwrite the version by specifying `<extension>_EXTENSION_VERSION` in AppSettings.   For above example, we have 2 versions of Kudu in different [semver](http://semver.org/) folders.   The default version setting is `latest` which means using any latest version excluding preview/beta (in this case, 1.24.12345).   However, if you want to try beta or preview version, you may set KUDU_EXTENSION_VERSION to `beta` and it will look for latest including such.   Other available version values include `disabled` if you want to disable this extension entirely as well as `specific semver` if you only want to use specific version.
  
ApplicationHost Config Transformation 
-------------------------------------
You may wonder how Kudu or any other extensions gets setup in the SCM site.   The key is the applicationHost.xdt (notice one exists for each versioned folders).   The [Xml Document Transform](http://msdn.microsoft.com/en-us/library/dd465326.aspx) file is used to transform the actual applicationHost.config for the site.  For instance, a simple site extension applicationHost.xdt could just add a /somepath IIS application under the SCM site.


    <?xml version="1.0"?>
    <configuration xmlns:xdt="http://schemas.microsoft.com/XML-Document-Transform">
      <system.applicationHost>
        <sites>
          <site name="%XDT_SCMSITENAME%" xdt:Locator="Match(name)">
            <application path="/somepath" xdt:Locator="Match(path)" xdt:Transform="Remove" />
            <application path="/somepath" applicationPool="%XDT_APPPOOLNAME%" xdt:Transform="Insert">
              <virtualDirectory path="/" physicalPath="%XDT_EXTENSIONPATH%" />
            </application>
          </site>
        </sites>
      </system.applicationHost>
    </configuration>

There are a set of env variables passed to XDT to assist in locating the appropriate elements and set the proper property.

* `XDT_SITENAME` is the current site name
* `XDT_SCMSITENAME` is the current scm site name
* `XDT_APPPOOLNAME` is the application pool name
* `XDT_EXTENSIONPATH` is the version specific extension physical path
* `HOME` is the site root path

By the way, the XDT is not limited to only `<site/>` sections.   It could transform other part of applicationHost.config such as introducing a new mime type for `<httpCompression/>` or adjusting `<httpModules/>`.    

Private Extensions
------------------
The site owners can overwrite the existing site extensions with their own implementations or introduce a totally new set of site extensions altogether.   For demonstration, we created this [simple extension](https://github.com/projectkudu/SimpleWebSiteExtension) sample.   The step involves ...

* Upload extension bits to root `SiteExtensions` folder.   Below examples illustrate overriding Kudu extension as well as introducing new Foo extension.

        /site
        /LogFiles
        /SiteExtensions
            /Kudu
                applicationHost.xdt
                kudu bits...
            /Foo
                applicationHost.xdt
                foo bits...

* Set AppSetting `WEBSITE_PRIVATE_EXTENSIONS` = `1`

Notes:

- For any issue with XDT, look at `/LogFiles/Transform` log files.
- Extensions only get applied after the site is restarted. Initially, setting `WEBSITE_PRIVATE_EXTENSIONS` = `1` will restart the site, but if you later add/remove extensions, you will need to manually restart the site from the portal. However, if you modify existing extensions without changing the applicationHost.xdt, no manual restart is needed.