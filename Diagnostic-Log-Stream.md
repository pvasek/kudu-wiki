## Overview

One of the keys to diagnose the issues is to look at the trace files.  Kudu service as well as its application write traces to /Logfiles folder (see [[File structure on Azure]]).  Logstream endpoint (/logstream) enables developers to view live traces as they are being written allowing the real-time diagnostics.

## Using /logstream

By simply connecting to <kudu-service-url>/logstream, you will be able to get the live streaming of traces.  The /logstream watches all the files changes under /LogFiles and its sub folders.   To scope the live traces to certain providers/folders, you may additional specify the path.

* /logstream/git/trace => kudu related traces intended for diagnostic kudu issues.
* /logstream/git/deployment => kudu deployment traces intended for application developers/operators.
* /logstream/application => application traces.
* /logstream/http => iis logs.

etc.

## trace_level knobs

Most of the trace providers allow users to adjust the verbosity of the traces.  Kudu trace provider uses Diagnostic's TraceLevel; Off=0 (default), Error=1, Warning=2, Info=3, Verbose=4.  Its trace level can be adjusted via settings.

* set kudu trace_level to Verbose.

  `$ curl <kudu-service-url>/settings -X POST -H "Content-Type: Application/json" -d "{'trace_level':'4'}"` 

* get kudu trace_level to Verbose.

  `$ curl <kudu-service-url>/settings/trace_level` 
 
## Lifetime /logstream

By simply connecting to <kudu-service-url>/logstream, you will be able to get the live streaming of traces.  The /logstream watches all the files changes under /LogFiles and its sub folders.   To scope the live traces to certain providers/folders, you may additional specify the path.


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

## Building and running Kudu from the github sources

Read this is you are interested in contributing to Kudu

* [[Getting Started]]
* Blog post: [Developing Kudu locally and on Azure](http://blog.davidebbo.com/2012/06/developing-kudu-locally-and-on-azure.html)
* [[Project Structure]]
* [[Running Tests]]
* [[Git workflow]] for contributors
* [Coding Guidelines](http://aspnetwebstack.codeplex.com/wikipage?title=CodingConventions) We use the coding guidelines used by AspNetWebStack

## Features in development

* [[Deploying from GitHub]]
* [[Custom Deployment Script]]

## Miscellaneous

* [[Blog posts and screencasts]]
* [[Cool user tweets]]