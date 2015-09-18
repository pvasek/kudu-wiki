Azure Web Apps doesn't have direct support for Perl, but it's possible to get it working with some manual steps:

- Create a Web App.
- Go to [[Kudu Console]] to easily drop files into your site.
- Go to http://strawberryperl.com/releases.html and download the "32bit ZIP edition".
- Create a `perl` folder under `D:\home\site`.
- Drag and drop the perl zip file into that folder, making sure to drop it in the special area that expands zip files. It's a big file, so it could take several minutes.
- Go into the `D:\home\site\perl\perl\lib` folder, and create a subfolder named `FCGI`.
- On your machine, download http://search.cpan.org/CPAN/authors/id/C/CO/COSMICNET/FCGI-IIS-0.05.tar.gz
- Using a tool like 7zip, extract the file `IIS.pm` out of it (it's under `FCGI-IIS-0.05\lib\FCGI`).
- Copy that file into the FCGI folder you create above in the Kudu Console.
- Go to the Azure Portal (https://manage.windowsazure.com/) and go to you site's Configure page.
- Under Handler Mappings, add one for: Extension: `*.pl`, Script processor path: `D:\home\site\perl\perl\bin\perl.exe`, Additional arguments: `-MFCGI::IIS=do`.
- Using Kudu console, copy a Perl file into `D:\home\site\wwwroot`. e.g. `hello.pl`.
- Request http://{yoursite}.azurewebsites.net/hello.pl to run it.

Here is an example hello.pl file you can start with:

```
#!/usr/bin/perl
print "Content-type: text/html\n\n";
print "Hello from perl!\n";
```
