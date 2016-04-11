## [UNDER REVIEW]
## Manually triggering a deployment

Here's how to trigger a (fetch) deployment from an external __git__ repository.

e.g. You have a git repository at https://example.com/git/RepositoryName.git and you'd like to notify Kudu to trigger a deployment for every new commit.

##### Raw POST
```
POST /deploy HTTP/1.1

Host: $SiteLevelUsername:SiteLevelPassword@<SiteName>.scm.azurewebsites.net
Content-Type: application/json
Accept: application/json
X-SITE-DEPLOYMENT-ID: <SiteName>
Transfer-encoding: chunked

{
    "format":"basic",
    "url":"https://username:password@example.com/git/RepositoryName.git"
}
````
For Mercurial (hg) add `"is_hg": true` to the JSON payload.

#### Azure Specific (using Azure PowerShell):

E.g. Using the "External Repository" deployment method in the Azure Portal.

In addition to the POST above you can accomplish the same thing by invoking the following cmdlet:
````
# Action sync
Invoke-AzureRmResourceAction -ResourceGroupName <ResourceGroupName> `
                             -ResourceType Microsoft.Web/sites `
                             -ResourceName <WebAppName> `
                             -Action sync `
                             -ApiVersion 2015-08-01 `
                             -Force -Verbose
````

GitHub specific guide here: https://github.com/projectkudu/kudu/wiki/Deploying-from-github
