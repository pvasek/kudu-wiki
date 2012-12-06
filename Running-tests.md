First, make sure you look at the [[Getting started]] page for the list of prerequisites.

All tests use xunit; this includes the unit tests and functional tests. 

## Unit tests
To run the unit tests just use build.cmd or use the VS 2012 XUnit.net runner (or [Test Driven .NET](http://www.testdriven.net/)) to run them. They are in `Kudu.Core.Test`.

## Functional tests
The functional tests are located in the `Kudu.FunctionalTests` project. Running them requires the following software to be installed.

* The XUnit.net runner for VS 2012, which you install from Tools / Extensions & Updates
* Both 32-bit and 64-bit versions of [Node](http://nodejs.org/download/). Only the 64 bit one is actally used at runtime, but there are some quirky tests that expect the 32-bit version to also be present. The co-exist fine side by side, so this is harmless.
* x64 [IISNode](https://github.com/tjanczuk/iisnode)
* Under `\Program Files (x86)\nodejs`, you need to have a folder named `0.8.2`, and containing a file named `node.exe`. **it does not matter what this file is, as only its presence is checked!** Yes, this is hacky, but is needed to exercise the version detection logic.
* The Web and WebApplications folders need to be present under `\Program Files (x86)\MSBuild\Microsoft\VisualStudio\v10.0`. If you don't have them, you may need to copy them from a machine that has VS 2010 installed. This is needed because some of the test apps target these. **TODO**: we need to make this easier!

You can also use the [Web Platform Installer](http://go.microsoft.com/fwlink/?LinkId=255386) to install iisnode. Once you have this software, you should be able to run all functional tests. 

After each test is run, there'll be a folder created in `bin\{Configuration}\TestResults\{appname}` where the app name is the name of the test. If the test fails, it will *NOT* be deleted from IIS, you should clean this up manually by deleting {site}, kudu_service_{site} and the app pool {site}.

**NOTE: The functional tests take about 10 minutes to run.**