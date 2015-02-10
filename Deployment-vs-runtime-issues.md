We get a lot of questions from users that look more or less like "*I deploy my site and it doesn't run*". When that happens, the first thing we need to do is determine whether it is a deployment issue or a runtime issue. This article explains the difference, and how to help isolate which one it is.

## Deployment issues

A deployment issue is an issue that causes the wrong set of files to get deployed to your site folder (typically `site\wwwroot`). i.e. as a result of deployment, you expect a certain set of files to end up in that folder, but due to some problem during deployment, you end up with a different set of files.

As a result of a deployment issue, you will often end up with a non-running site, which makes it look like a 'runtime' issue. But the core issue is with the deployment. **When that happens, you should not ask yourself "*why isn't my site running correctly?*". Instead, the right question to ask is "*why aren't the right files ending up in my wwwroot folder?*".**

Note that there can be all kind of deployment issues, and they can potentially happen with any deployment technology (e.g. Kudu, WebDeploy, FTP).

When using Kudu, there are all kind of possibilities:
- something in your repo may not be quite right
- maybe you have a custom deployment script that doesn't quite work
- maybe you're running into a Kudu bug

We can help investigate those after you have made the determination that you are in fact dealing with a deployment issue.

## Runtime issues

A runtime issue happens when the files in your wwwroot folder are exactly what they should be, but for some reason the site doesn't run correctly.

When that happens, it is no longer relevant what technique you used to deploy your site. All that matter is that you got the files in the right place. In this case, a description of your issue should focus on what your code is doing at runtime, and how it is failing, rather than on how you deployed it.

One important point here is that we are talking about the runtime **on Azure Websites**. So just because it works on your machine does not imply that you don't have a runtime issue On Azure Websites.

There are many things that could cause a runtime issue. e.g.
- there could be an external dependency like a database that's not set up correctly
- your code could be making invalid assumptions about paths
- something in the Websites runtime environment might block certain things that work on your local box
- there could be OS differences if you are developing on a Mac, since Azure Websites runs on Windows.

## Determining which type of issue you have

The simplest way is to use the [[Kudu Console]] to look at the files in your wwwroot folder. It even lets you easily download everything in that folder as a zip file, so you can analyze it locally.

It can take a little bit of effort, but it is a key step to take to help getting your issue properly investigated.