All tests use xunit, this includes the unit tests and functional tests. 

## Unit tests
To run the unit tests just use build.cmd or use [Test Driven .NET](http://www.testdriven.net/) to run them. They are in `Kudu.Core.Test`.

## Functional tests
The functional tests are located in the `Kudu.FunctionalTests` project. Running them requires the following software to be installed.

* Visual Studio 2012
* Both 32-bit and 64-bit versions of [Node](http://nodejs.org/download/). Only the 64 bit one is actally used at runtime, but there are some quirky tests that expect the 32-bit version to also be present. The co-exist fine side by side, so this is harmless.
* x64 [IISNode](https://github.com/tjanczuk/iisnode)

You can also use the [Web Platform Installer](http://go.microsoft.com/fwlink/?LinkId=255386) to install iisnode. Once you have this software, you should be able to run all functional tests. 

After each test is run, there'll be a folder created in `bin\{Configuration}\TestResults\{appname}` where the app name is the name of the test. If the test fails, it will *NOT* be deleted from IIS, you should clean this up manually by deleting {site}, kudu_service_{site} and the app pool {site}.

When making small changes and you want to verify basic behavior, run the following:

* `GitRepositoryManagementTests.PushSimpleRepoShouldDeploy` - Tests a basic website appliction
* `GitRepositoryManagementTests.WapBuildsReleaseMode` - Tests a basic Web application project
* `GitRepositoryManagementTests.PushAppChangesShouldTriggerBuild` - Tests a basic mvc site
* `GitRepositoryManagementTests.WarningsAsErrors` - Tests an error condition

### Repository tests
There's a set of functional tests that are driven by `GitDeploymentTests.csv`. This file contains a list of repositories and metadata information that we can use to verify each push. You can run them by executing `GitRepositoryManagementTests.PushAndDeployApps`.

**NOTE: The functional tests can take up to 45 minutes to run.**