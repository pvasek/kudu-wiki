Note: this section applies to both Pre-Installed and Private [[site extensions|Azure Site Extensions]].

The simplest way to apply one of these transforms for your site is:

- Create an `applicationHost.xdt` file under your 'site' folder, and copy the content below into it

See [Xml Document Transform](http://msdn.microsoft.com/en-us/library/dd465326.aspx) for detailed documentation on the XDT syntax.

### Adding environment variables

The following will inject an environment variable named `FOO`, with value `BAR`:
```xml
<?xml version="1.0"?> 
<configuration xmlns:xdt="http://schemas.microsoft.com/XML-Document-Transform"> 
  <system.webServer> 
    <runtime xdt:Transform="Insert" >
         <environmentVariables>
            <add name="FOO" value="BAR" />    
         </environmentVariables>
    </runtime> 
  </system.webServer> 
</configuration> 
```


### Adding new applications to the 'scm' site

You may wonder how Kudu or other extensions gets set up in the SCM site. The key is the applicationHost.xdt (notice one exists for each versioned folders). The [Xml Document Transform](http://msdn.microsoft.com/en-us/library/dd465326.aspx) file is used to transform the actual applicationHost.config for the site. 

This transform adds a /somepath IIS application under the SCM site.

```xml
<?xml version="1.0"?>
<configuration xmlns:xdt="http://schemas.microsoft.com/XML-Document-Transform">
  <system.applicationHost>
    <sites>
      <site name="%XDT_SCMSITENAME%" xdt:Locator="Match(name)">
        <application path="/somepath" xdt:Transform="Insert">
          <virtualDirectory path="/" physicalPath="%XDT_EXTENSIONPATH%" />
        </application>
      </site>
    </sites>
  </system.applicationHost>
</configuration>
```

### Adding new applications to the 'main' site

It is a variation of the above but simply adds a /somepath IIS application under the main site (`%XDT_SITENAME%`) instead.

```xml
<?xml version="1.0"?>
<configuration xmlns:xdt="http://schemas.microsoft.com/XML-Document-Transform">
  <system.applicationHost>
    <sites>
      <site name="%XDT_SITENAME%" xdt:Locator="Match(name)">
        <application path="/somepath" xdt:Transform="Insert">
          <virtualDirectory path="/" physicalPath="%XDT_EXTENSIONPATH%" />
        </application>
      </site>
    </sites>
  </system.applicationHost>
</configuration>
```

### Changing the number of segments allowed in the URL

```xml
<?xml version="1.0"?>
<configuration xmlns:xdt="http://schemas.microsoft.com/XML-Document-Transform">
  <system.applicationHost>
    <sites>
      <site name="%XDT_SITENAME%" xdt:Locator="Match(name)">
        <limits xdt:Transform="Remove" />
        <limits xdt:Transform="Insert" maxUrlSegments="64" />
      </site>
    </sites>
  </system.applicationHost>
</configuration>
```

### Adding a mime type to the httpCompression section

```xml
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
```

### Turning off `noCompressionForProxies` attribute

```xml
<?xml version="1.0"?>
<configuration xmlns:xdt="http://schemas.microsoft.com/XML-Document-Transform">
  <system.webServer>
    <httpCompression xdt:Transform="SetAttributes(noCompressionForProxies)" noCompressionForProxies="false" >
    </httpCompression>
  </system.webServer>
</configuration>
```

### Remove all your recycling options from your .NET 4(+) application pool, and make it available always

```xml
<?xml version="1.0"?>
<configuration xmlns:xdt="http://schemas.microsoft.com/XML-Document-Transform">
  <system.applicationHost>
    <applicationPools>
      <add name="%XDT_SITENAME%" xdt:Transform="Insert" autoStart="true" managedRuntimeVersion="v4.0" startMode="AlwaysRunning">
        <processModel idleTimeout="00:00:00" />
        <recycling>
          <periodicRestart time="00:00:00" />
        </recycling>
      </add>
    </applicationPools>
  </system.applicationHost>
</configuration>
```        

### Recycle your application pool at a given time, say off-business hours.

```xml
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
```

### Increase the queueLength for your application pool

Note: the default is 1000

```xml
<?xml version="1.0"?>
<configuration xmlns:xdt="http://schemas.microsoft.com/XML-Document-Transform">
  <system.applicationHost>
    <applicationPools>
      <add name="%XDT_SITENAME%" xdt:Locator="Match(name)" xdt:Transform="SetAttributes(queueLength)" queueLength="5000">
      </add>
    </applicationPools>
  </system.applicationHost>
</configuration>
```

### Adding or changing an attribute for a specific version of PHP

This transform finds the `<application>` tag that has the v5.4 full path, and adds a new `queueLength` attribute to it.

```xml
<?xml version="1.0"?>
<configuration xmlns:xdt="http://schemas.microsoft.com/XML-Document-Transform">
  <system.webServer>
    <fastCgi>
      <application xdt:Locator="Match(fullPath)" xdt:Transform="SetAttributes(queueLength)"
            fullPath="D:\Program Files (x86)\PHP\v5.4\php-cgi.exe" queueLength="5000"/>
    </fastCgi>
  </system.webServer>
</configuration>
```

This transform changes the value of `maxInstances` for PHP 5.4.

```xml
<?xml version="1.0"?>
<configuration xmlns:xdt="http://schemas.microsoft.com/XML-Document-Transform">
  <system.webServer>
    <fastCgi>
      <application xdt:Locator="Match(fullPath)" xdt:Transform="SetAttributes(maxInstances)"
            fullPath="D:\Program Files (x86)\PHP\v5.4\php-cgi.exe" maxInstances="8"/>
    </fastCgi>
  </system.webServer>
</configuration>
```

### Adding an ASP Classic attribute

e.g. this enables parent paths

```xml
<?xml version="1.0"?>
<configuration xmlns:xdt="http://schemas.microsoft.com/XML-Document-Transform">
  <system.webServer>
    <asp xdt:Transform="SetAttributes(enableParentPaths)" enableParentPaths="true" />
  </system.webServer>
</configuration>
```
### Registering an IIS `Native` HttpModule

e.g. this enables native module (native module must be registered at globalModules)

```xml
<?xml version="1.0"?>
<configuration xmlns:xdt="http://schemas.microsoft.com/XML-Document-Transform">
  <system.webServer>
    <globalModules>
      <add name="Your Module Name" image="<path to dll such as d:\home\SiteExtensions\MyNative\module.dll>" preCondition="bitness32" xdt:Locator="Match(name)" xdt:Transform="InsertIfMissing" />
    </globalModules>
  </system.webServer>
</configuration>
```

### Registering an IIS HttpModule

e.g. this enables SomeModule on the main site

```xml
<?xml version="1.0"?>
<configuration xmlns:xdt="http://schemas.microsoft.com/XML-Document-Transform">
  <location path="%XDT_SITENAME%" xdt:Locator="Match(path)">
    <system.webServer>
      <modules>
        <add name="SomeModule" type="SomeModule.SomeModuleType" xdt:Transform="InsertIfMissing"/>
      </modules>
    </system.webServer>
  </location>
</configuration>
```

### Allowing arbitrary ISAPI extensions to be loaded

```xml
<?xml version="1.0"?>
<configuration xmlns:xdt="http://schemas.microsoft.com/XML-Document-Transform">
  <system.webServer>
    <security>
      <isapiCgiRestriction xdt:Transform="SetAttributes(notListedIsapisAllowed)" notListedIsapisAllowed="true"/>
    </security>
  </system.webServer>
</configuration>
```

### Adding IP restrictions

```xml
<?xml version="1.0"?>
<configuration xmlns:xdt="http://schemas.microsoft.com/XML-Document-Transform">
  <system.webServer>
    <security>
      <ipSecurity xdt:Transform="RemoveAttributes(allowUnlisted)">
        <add ipAddress="204.79.197.200" allowed="true" xdt:Transform="Insert"/>
      </ipSecurity>
    </security>
  </system.webServer>
</configuration>
```

### Only log successful http requests to the IIS log (aka Web Server log)

```xml
<?xml version="1.0"?>
<configuration xmlns:xdt="http://schemas.microsoft.com/XML-Document-Transform">
  <system.webServer>
    <httpLogging xdt:Transform="SetAttributes(selectiveLogging)" selectiveLogging="LogSuccessful" />
  </system.webServer>
</configuration>
```

### Enable Web Sockets

The following does the equivalent of enabling Web Sockets in the portal

```xml
<?xml version="1.0"?>
<configuration xmlns:xdt="http://schemas.microsoft.com/XML-Document-Transform">
  <system.webServer>
    <globalModules>
      <add name="WebSocketModule" xdt:Locator="Match(WebSocketModule)" xdt:Transform="Remove" />
      <add name="WebSocketModule" image="%windir%\System32\inetsrv\iiswsock.dll" xdt:Transform="Insert" />
    </globalModules>
    <webSocket xdt:Transform="SetAttributes(pingInterval)" pingInterval="00:02:30" />
  </system.webServer>
  <location path="" xdt:Locator="Match(path)" xdt:Transform="InsertIfMissing">
    <system.webServer xdt:Transform="InsertIfMissing">
      <modules xdt:Transform="InsertIfMissing">
        <add name="WebSocketModule" xdt:Locator="Match(WebSocketModule)" xdt:Transform="Remove" />
        <add name="WebSocketModule" lockItem="true" xdt:Transform="Insert" />
      </modules>
    </system.webServer>
  </location>
</configuration>
```