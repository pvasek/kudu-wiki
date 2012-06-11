When you have a Windows Azure Web Site, it is possible to access its files via FTP. This includes both the files that are deployed, and the repository and deployment related files.

To connect via FTP, you need the following information:

- **The FTP host name**, which will be something like waws-prod-blu-001.ftp.azurewebsites.windows.net. You'll find it under the Dashboard tab for your site in the Azure portal.
- **Your username**: you'll also find it on the dashboard, under Deployment User. It's the same as your git username, but prefixed with the site name. e.g. 'MyCoolSite\davidebbo'
- **Your password**: use the same password as when you 'git push'

You can use whatever FTP client you like, but you can't go wrong if you go with [FileZilla](http://filezilla-project.org/).

Once you've FTP'd, check out [[File structure on Azure]] to navigate through the files.