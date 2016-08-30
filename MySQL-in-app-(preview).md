The feature enables running [MySql](http://www.mysql.com/) within the site itself.  One does not need to provision database explicitly and the data is always backed up along with the site contents.   The key benefit is ease of use/setup and network performance (MySql running locally within the site).   The limitation is it does not support multiple instances, auto scale nor slots.

## Scenarios

Any sites using MySql as database namely [WordPress](https://wordpress.org/), [Joomla](https://www.joomla.org/), etc.

## Enable/disable feature 

To enable the feature, see [this](https://blogs.msdn.microsoft.com/appserviceteam/2016/08/18/announcing-mysql-in-app-preview-for-web-apps/).  To verify if MySql is actually accessible by the site, create the following file `d:\home\site\wwwroot\phpmysql.php` with below content and browse to `http://<sitename>.azurewebsites.net/phpmysql.php`.

```php
<?php
$servername = "";
$username = "";
$password = "";
$dbname = "";

// Parsing connnection string
foreach ($_SERVER as $key => $value) {
    if (strpos($key, "MYSQLCONNSTR_") !== 0) {
        continue;
    }
    
    $servername = preg_replace("/^.*Data Source=(.+?);.*$/", "\\1", $value);
    $dbname = preg_replace("/^.*Database=(.+?);.*$/", "\\1", $value);
    $username = preg_replace("/^.*User Id=(.+?);.*$/", "\\1", $value);
    $password = preg_replace("/^.*Password=(.+?)$/", "\\1", $value);
}

// Create connection
$conn = new mysqli($servername, $username, $password, $dbname);
// Check connection
if ($conn->connect_error) {
    die("Connection failed: " . $conn->connect_error);
} else {
    echo "connection successful<br/>";
}
$conn->close();
?>
```

## Internals, Diagnostics and Loggings

#### Where can I find MySql Credentials (connection string)?

The connection string flows to your application as an env variable `MYSQLCONNSTR_localdb`.  Beware that we are *not* using the default MySql port (3306).  In fact, the port number may vary for each application life cycle depending on its availability at startup time.  The port info is also available as an env variable `WEBSITE_MYSQL_PORT` to your site.
  
#### How to use phpMyAdmin with MySql in-app?

[phpMyAdmin](https://www.phpmyadmin.net/) is enabled by default with the feature.  You can access it thru `https://<sitename>.scm.azurewebsites.net/phpMyAdmin/`.  The phpMyAdmin is aware of MySql credentials and will connect automatically.  The *caveat* is since MySql is only started with the main site if one were to access phpMyAdmin (which is part of SCM site) *without* the main site (and MySql) running, it may confusingly prompt for password.   We are working toward improving the phpMyAdmin experience to better inform users in this scenario.

#### What about MySql command line tools?

Among other things, `MySql.exe` or `MySqlAdmin.exe` tools are available at `d:\program files (x86)\MySql\5.7.9.0\bin`.  Like phpMyAdmin above, make sure the main site is running before using the tool.  The credential is available at `D:\home\data\mysql\MYSQLCONNSTR_localdb.txt`.  Due to limited spaces, we may not have all the complete MySql tool set.  Let us know (via [forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=windowsazurewebsitespreview)) if any other tools would be useful.

#### How do we define MySql credentials/security?

MySql in-app is running on the same sandbox as the site itself and it can only be accessed locally (127.0.0.1).  The port is dynamically defined (not always default 3306) to support multiple sites running on the same VM (multi-tenant).  We will best-effort affinitize to the same port for the same site.   The most important part is MySql in-app database files are part of the site content (`D:\home\data\mysql`) and ones with site admin credential (for instance, able to access SCM site) will have access to the files; as a result, full access to database.   Based on above security, we intentionally keep MySql credential simple with a pre-set of username, password and database name.  If you think of a scenario where different user credentials must be used for the same app, do let us know (via [forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=windowsazurewebsitespreview)).

#### Where is MySql Data directory?

MySql data is stored at `d:\home\data\mysql` directory.

#### Where is MySql Server log?

MySql server log is stored at `d:\home\logfiles\mysql` directory.   This will be where you start investigating any MySql issues.  Beware that the logfile names are dated and, if the number exceeds the limit (50 log files), old ones will be removed to free up spaces.  

To turn on [general log](http://dev.mysql.com/doc/refman/5.7/en/query-log.html), simply add appSettings `WEBSITE_MYSQL_GENERAL_LOG = 1`.  The log file is also stored at `d:\home\logfiles\mysql` directory (look for log file with `general` suffix).

To turn on [slow query log](http://dev.mysql.com/doc/refman/5.7/en/slow-query-log.html), simply add appSettings `WEBSITE_MYSQL_SLOW_QUERY_LOG = 1`.  The log file is also stored at `d:\home\logfiles\mysql` directory (look for log file with `slowquery` suffix).

#### How do I customize MySql server (MySqld.exe arguments)?

At this moment, we don't expose such knobs.  However, do give us feedbacks (via [forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=windowsazurewebsitespreview)) and we will look into it. 

#### How do I report issues?

[Forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=windowsazurewebsitespreview) is a good starting point.  Do title it with `MySql in-app` keyword for faster references.
