Here are some of the steps that are needed to deploy Kudu to a Windows 2012 server:

- Enable Web Server role, and within that:
  - Application Development / ASP.NET 4.5
- Copy "C:\Program Files (x86)\MSBuild" from dev machine onto same location on server
- Install:
  - Git
  - Node
- Copy KuduWeb artifacts to C:\inetpub\wwwroot
  - Create an empty App_Data in there
- Copy the KuduService artifacts into C:\inetpub\Kudu.Services.Web
- Change AppPool to run as LocalSystem so it can manage IIS sites
- Give 'Users' full access to C:\inetpub\apps, which is where sites get created
  - Note that you can change that location using the `sitesPath` AppSetting in Kudu.Web's web.config.