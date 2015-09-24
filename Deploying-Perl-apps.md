Azure Web Apps doesn't have direct support for Perl, but it's possible to get it working with some manual steps:

- Create a Web App.
- Go to [[Kudu Console]] to easily drop files into your site.
- Go to http://strawberryperl.com/releases.html and download the "32bit ZIP edition".
- Create a `perl` folder under `D:\home\site`.
- Drag and drop the perl zip file into that folder, making sure to drop it in the [special area](https://github.com/projectkudu/kudu/wiki/Kudu-console#upload-and-expand-zip-file) that expands zip files. It's a big file, so it could take several minutes.
- Go into the `D:\home\site\perl\perl\lib` folder, and create a subfolder named `FCGI`.
- On your machine, download http://search.cpan.org/CPAN/authors/id/C/CO/COSMICNET/FCGI-IIS-0.05.tar.gz
- Using a tool like 7zip, extract the file `IIS.pm` out of it (it's under `FCGI-IIS-0.05\lib\FCGI`).
- Copy that file into the FCGI folder you create above in the Kudu Console.
- Go to the [Azure Portal](https://manage.windowsazure.com/) and go to you site's Configure page.
- Under Handler Mappings, add one for: Extension: `*.pl`, Script processor path: `D:\home\site\perl\perl\bin\perl.exe`, Additional arguments: `-MFCGI::IIS=do`.

![Handler](https://cloud.githubusercontent.com/assets/6472374/10073698/7694ff64-62d3-11e5-9ec3-0cc7bab187f3.PNG)

- Using Kudu console, copy a Perl file into `D:\home\site\wwwroot`. e.g. `hello.pl`.
- Request `http://{yoursite}.azurewebsites.net/hello.pl` to run it.

Here is an example hello.pl file you can start with:

```perl
print "Content-type: text/plain\n\n";
print "Hello from Perl on Windows!\n";
```

If you're using __CGI.pm__ to write to disk (or anything that uses __File::Temp__ for that matter), be aware that Perl does not get the right temporary location in its `$ENV{TMPDIR}`. You'll need to specify it yourself at the top of your script.

Example:
```perl
BEGIN { $ENV{TMPDIR} = "d:\\local\\temp"; }

use CGI;
my $q = new CGI;
print $q->header(-type => "text/plain");

print "Temporary folder set to $ENV{TMPDIR}\n";
```


Output:

`Temporary folder set to d:\local\temp`

See this issue for more details on `File::Temp`: https://github.com/projectkudu/kudu/issues/1700