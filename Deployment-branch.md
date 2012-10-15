By default, only pushes to the 'master' branch get deployed. But you can change this by calling a REST API on the Kudu service. One simple way to do this is to use curl.

Here is an example:

    curl --user yourname -H "Content-Type: application/json; charset=UTF-8" --data "{key: 'branch', value: 'somebranch' }" https://yoursite.scm.azurewebsites.net/settings

The Azure portal also has UI to change this (in the Configure tab).