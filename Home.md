Kudu is the engine behind [git deployments in Azure Web Sites](https://www.windowsazure.com/en-us/develop/nodejs/common-tasks/publishing-with-git/). It can also run outside of Azure.

## Getting help

* Post to the [Azure Git Deployment forum on MSDN](http://social.msdn.microsoft.com/Forums/en-US/azuregit)
* Go to the [Kudu room on JabbR](http://jabbr.net/#/rooms/kudu)
* Ping [@davidebbo](https://twitter.com/davidebbo) or [@davidfowl](https://twitter.com/davidfowl) on Twitter


## Using git in Azure Web Sites

If you are using git to push to an Azure Web Site, this is the section you'll care most about.

### Version of Kudu currently live in Azure

* The changeset that's currently live in Azure is [94d69abe1cd0bc822635f0b4dc12e459fae4ef04](https://github.com/projectkudu/kudu/commit/94d69abe1cd0bc822635f0b4dc12e459fae4ef04)

### How-to guides

* [Publishing a website with Git](https://www.windowsazure.com/en-us/develop/nodejs/common-tasks/publishing-with-git/) (official documentation ftom windowsazure.com)
* [[Customizing Deployments]]: choosing which folder or project to deploy to the web site
* [[Deployment branch]]: choosing the branch that gets deployed when you push
* [[Managing settings and secrets]]: how to modify the values when the app is deployed
* [Managing database connections in Azure Web Sites](http://blog.davidebbo.com/2012/09/managing-database-connections-in-azure.html)
* [[Deployment hooks]]: how to take over the deployment logic for a site
* [[Accessing files via FTP]]

### Understanding Kudu

* [[Kudu architecture]]
* [[REST API]]
* [[Enabling continuous deployment from Github, Codeplex and Bitbucket|Continuous deployment]]
* [[File structure on Azure]]
* [[Understanding deployment credentials|Deployment credentials]]
* [[How Kudu deploys sites|Deployment]]


### Troubleshooting

* [[Known issues]]
* [[Investigating Issues]]
* [[Diagnostic Log Stream]]
* [[Investigating Continuous Deployment]]

## Building and running Kudu from the github sources

Read this is you are interested in contributing to Kudu

* [[Getting Started]]
* Blog post: [Developing Kudu locally and on Azure](http://blog.davidebbo.com/2012/06/developing-kudu-locally-and-on-azure.html)
* [[Project Structure]]
* [[Running Tests]]
* [[Deploying to a server]]
* [[Git workflow]] for contributors
* [Coding Guidelines](http://aspnetwebstack.codeplex.com/wikipage?title=CodingConventions) We use the coding guidelines used by AspNetWebStack

## Features in development

* [[Deploying from GitHub]]
* [[Custom Deployment Script]]

## Miscellaneous

* [[Blog posts and screencasts]]
* [[Cool user tweets]]