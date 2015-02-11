One area that is a bit confusing is that Azure Website deployment using Visual Studio Online (VSO) works quite different from deployment using Kudu.

When you set up deployment from source control in the Azure portal, you get a number of options:
- Visual Studio Online
- Local Git repository
- GitHub
- Dropbox
- Bitbucket
- CodePlex
- External repository

Under the cover, all of these options are using the Kudu deployment engine, except for VSO which is using the separate VSO build infrastructure.

This article explains some of the differences you may encounter, and how to report and investigate them.

## How does Kudu deployment work

When using one of the options that go through Kudu, your sources live within the Azure Websites infrastructure, and get built and deployed from within Azure Websites.

If you connect to your site via [[Kudu Console]], you will see your sources under `site\repository`, as well, as various deployment related artifacts.

## How does Visual Studio Online deployment work

When setting up continuous deployment to an Azure Website from Visual Studio Online, things work in the following way:
- You repository lives in the VSO infrastructure.
- When you push a change via git, the VSO build infrastructure builds your sources, and produces some deployment artifacts.
- VSO then uses WebDeploy (the same technology used by Visual Studio) to deploy the artifacts to your Azure Websites.

Here, if you connect to your site using Kudu console, you will see the deployed files under your `site\wwwroot` folder, but won't see a repository folder, or any of the other things you get with Kudu. The Kudu engine is not involved at all in this scenario.

## Why does this matter?

It matters because the build process and build environment are not identical in both cases. e.g. they may not have exactly the same version of the SDKs, as they update on different schedules.

Also, everything you read in this Kudu wiki applies to Kudu based deployments and not to VSO deployments. e.g. the various Kudu app settings like `SCM_BUILD_ARGS` (from [this page](https://github.com/projectkudu/kudu/wiki/Configurable-settings)) will have no effect at all when using VSO.

Instead, with VSO, you have to use the mechanisms provided by VSO (like the Build Definition file) to tweak your deployment process.

Also, it matters in term of reporting issues to the correct set of people:
- For Kudu, you can use the [Websites forum](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=windowsazurewebsitespreview), or open issue in this Kudu GitHub project.
- For VSO, you should instead use the [Visual Studio Online Forum](https://social.msdn.microsoft.com/Forums/vstudio/en-US/home?forum=TFService), where you will find the right set of experts.