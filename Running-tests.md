All tests use xunit, this includes the unit tests and functional tests. 

## Unit tests
To run the unit tests just use build.cmd or use [Test Driven .NET](http://www.testdriven.net/) to run them. They are in `Kudu.Core.Test`.

## Functional tests
Running all functional tests require lots of software to be installed and working on IIS.
* IISNode
* ASP.NET MVC 3
* ASP.NET WebPages 1.0

You can download all of this software using the [Web Platform Installer](http://go.microsoft.com/fwlink/?LinkId=255386). Once you have this software, you should be able to run all functional tests. 

When making small changes and you want to verify basic behavior, run the following:
* `GitRepositoryManagementTests.PushSimpleRepoShouldDeploy` - Tests a basic website appliction
* `GitRepositoryManagementTests.WapBuildsReleaseMode` - Tests a basic Web application project
* `GitRepositoryManagementTests.PushAppChangesShouldTriggerBuild` - Tests a basic mvc site
* `GitRepositoryManagementTests.WarningsAsErrors` - Tests an error condition

### Repository tests
There's a set of functional tests that are driven by `GitDeploymentTests.csv`. This file contains a list of repositories and metadata information that we can use to verify each push. You can run them by executing `GitRepositoryManagementTests.PushAndDeployApps`.

**NOTE: The functional tests can take up to 45 minutes to run.**