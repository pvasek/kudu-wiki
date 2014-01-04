Note: this section applies to both Pre-Installed and Private [[site extensions|Azure Site Extensions]].

See [Xml Document Transform](http://msdn.microsoft.com/en-us/library/dd465326.aspx) for detailed documentation on the XDT syntax.

### Adding new applications to the 'scm' site

You may wonder how Kudu or other extensions gets set up in the SCM site. The key is the applicationHost.xdt (notice one exists for each versioned folders). The [Xml Document Transform](http://msdn.microsoft.com/en-us/library/dd465326.aspx) file is used to transform the actual applicationHost.config for the site. 

This transform adds a /somepath IIS application under the SCM site.

    <?xml version="1.0"?>
    <configuration xmlns:xdt="http://schemas.microsoft.com/XML-Document-Transform">
      <system.applicationHost>
        <sites>
          <site name="%XDT_SCMSITENAME%" xdt:Locator="Match(name)">
            <application path="/somepath" xdt:Locator="Match(path)" xdt:Transform="Remove" />
            <application path="/somepath" xdt:Transform="Insert">
              <virtualDirectory path="/" physicalPath="%XDT_EXTENSIONPATH%" />
            </application>
          </site>
        </sites>
      </system.applicationHost>
    </configuration>
    
### Adding a mime type to the httpCompression section

    <?xml version="1.0"?>
    <configuration xmlns:xdt="http://schemas.microsoft.com/XML-Document-Transform">
      <system.webServer>
        <httpCompression>
          <dynamicTypes>
            <add mimeType="application/foo" enabled="true" xdt:Transform="Insert" />
          </dynamicTypes>
        </httpCompression>
      </system.webServer>
    </configuration>


### Turning off `noCompressionForProxies` attribute

	<?xml version="1.0"?>
	<configuration xmlns:xdt="http://schemas.microsoft.com/XML-Document-Transform">
	  <system.webServer>
	    <httpCompression xdt:Transform="SetAttributes(noCompressionForProxies)" noCompressionForProxies="false" >
	    </httpCompression>
	  </system.webServer>
	</configuration>


### Remove all your recycling options from your .NET 4(+) application pool, and make it available always

    <?xml version="1.0"?>
    <configuration xmlns:xdt="http://schemas.microsoft.com/XML-Document-Transform">
      <system.applicationHost>
        <applicationPools>
          <add name="%XDT_SITENAME%" xdt:Locator="Match(name)" xdt:Transform="Remove" />
          <add name="%XDT_SITENAME%" xdt:Transform="Insert" autoStart="true" managedRuntimeVersion="v4.0" startMode="AlwaysRunning">
            <processModel idleTimeout="00:00:00" />
            <recycling>
              <periodicRestart time="00:00:00" />
            </recycling>
          </add>
        </applicationPools>
      </system.applicationHost>
    </configuration>
        

### Recycle your application pool at a given time, say off-business hours.

    <?xml version="1.0"?>
    <configuration xmlns:xdt="http://schemas.microsoft.com/XML-Document-Transform">
      <system.applicationHost>
        <applicationPools>
          <add name="%XDT_SITENAME%" xdt:Locator="Match(name)">
            <recycling xdt:Transform="Insert" >
              <periodicRestart>
                <schedule>
                  <clear />
                  <add value="00:00:00" />
                </schedule>
              </periodicRestart>
            </recycling>
          </add>
        </applicationPools>
      </system.applicationHost>
    </configuration>


### Adding an attribute to a specific version of PHP

This transform finds the `<application>` tag that has the v5.4 full path, and adds a new `queueLength` attribute to it.

    <?xml version="1.0"?>
    <configuration xmlns:xdt="http://schemas.microsoft.com/XML-Document-Transform">
      <system.webServer>
        <fastCgi>
          <application xdt:Locator="Match(fullPath)" xdt:Transform="SetAttributes(queueLength)"
                fullPath="D:\Program Files (x86)\PHP\v5.4\php-cgi.exe" queueLength="5000"/>
        </fastCgi>
      </system.webServer>
    </configuration>
