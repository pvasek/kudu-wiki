## S24
* Added diagsession and gcdump support to the [[Process API|Process-list-and-minidump]]
* Dump files now include matching mscordacwks.dll and SOS.dll for convenience
* Added threads to the [[Process API|Process-list-and-minidump]]
* Fix [selection issue](https://github.com/chrisdone/jquery-console/issues/32) in the Console.

## S23 (8/27/2013)
* [[Web Hooks]]
* [[Post Deployment Action Hooks]]

## S22
* Add support for [in-place and repo-less deployments](https://github.com/projectkudu/kudu/wiki/Deploying-inplace-and-without-repository)
* Add support for [getting process information and mini-dumps](https://github.com/projectkudu/kudu/wiki/Process-list-and-minidump)
* Add new kudexec page giving a console windows from within the browser

## S21.5

### Features
* Add vstest support (including test frameworks: mstest, nunit and xunit)
* Add support for F# sites
* Add support for RIA sites
* Add ability to override some deployment settings (e.g. `SCM_BUILD_ARGS`) using .deployment file

## S21

### Features
* Add optional support for shallow clones (to save disk space) via SCM_USE_SHALLOW_CLONE setting
* Add etag support to /deployments API to avoid getting data unless necessary
* Improve progress reporting during deployment
* Allow the root of the Kudu service to be used as an alternate git endpoint
* Trim old deployments after reaching some limit
* Several improvement and fixes to the diagnostic trace file

### Perf improvements
* Cache deployment script to avoid having to generate it each time
* Use unbuffered input stream during git push operations
* Removed some unnecessary operations when fetching from external git repository
* Removed OData and DotNetZip dependencies, reducing memory usage

### Key fixes (many more small fixes not listed)
* Fixed issue that was causing some failures git pushing from Macs
* Fixed several issues that were causing timeouts when pushing large repositories
* Improved detection for processes that are waiting for user input
* Improved recovery after a crash (previously git locks and unwanted git remotes would remain)
* Added retry loop when fetching from external repos to deal with network reliability issues
* Improved how Kudu detects if the git repository already exists

## S20
* Add support for deploying from Dropbox
* Add support for deploying Mercurial sites on Bitbucket and Codeplex
* Switch to use script generator as default deployment logic
* Add support for vfs REST API to access the file system
* Add support for zip download/upload API
* Improvements to Log streaming support
* Automatically update git submodules
* Support simple payload to force fetch and deploy
* Add support for Codebase, GitlabHq and Kiln in Kudu (no portal UI though)
* Switch Kudu to use .NET 4.5

## Earlier releases

Sorry, this page was not maintained earlier. Ideally, we'd go back in fill this in, but probably won't :)