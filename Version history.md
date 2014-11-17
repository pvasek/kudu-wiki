## S42 (11/14/2014)
* Add **in place** mode for WebJobs
* Change tracing to use one file per entry instead of a single trace.xml
* Add better Python support
* Add username to process info
* Don't ask why it jumped from S30 to S42. The reasons are bizarre and uninteresting :)

## S30 (10/27/2014)
* Support for auto-swap in continuous deployment scenarios
* Support passing feed url when installing site extensions
* Handle copying over a DLL in use by trying to rename it first (relevant to NewRelic)

## S29-QFE1 (9/29/2014)
* Disable deployment with app setting SCM_DISABLE_DEPLOY_ON_PUSH
* Gravatar support

## S29 (9/18/2014)
* New `SITE_BITNESS` environment variable

## S28-QFE6 (9/10/2014)
* Add initial support for ProjectK apps (aka ASP.NET vnext)
* New feature to download custom deployment scripts
* Upgrade Kudu to SignalR 2.x
* Marking processes with `scm` and `webjob` labels in Process Explorer
* Return LastModified header from `/api/vfs` API

## S28-QFE5 (8/22/2014)
* Temporary remedy for `npm install` failing when %APPDATA%\npm is not present
* Fixed issue of reading xml files in Debug Console

## S28-QFE4 (8/20/2014)
* Add support for viewing process command line in process API and UI
* Improve WebJobs error handling
* Fixed deallock issue when OnLockAcquired throws exception

## S28-QFE3 (8/15/2014)
* Adding Environment Variables view to Process API and UI
* Improving webjobs logging error handling
* change (add) api for webjobs from `jobs` to `* change (add) api for processes from `api/diagnostics/process` to `api/process`.
* Add httpErrors PassThrough to default Node config

## S28-QFE2 (7/20/2014)
* Enable commit-ish for generic deploy handler
* Support copy-paste several lines to Debug console
* Some visual changes for FileExplorer

## S28-QFE1 (7/2/2014)
* Cleanup index.lock for ScmVfs operations.
* Use Newtonsoft instead of DataContract for Json output annotations.
* Fixes for DebugConsole UI and autocomplete.

## S28 (6/19/2014)
* Fix small issue with site extensions API

## S27-QFE4 (6/17/2014)
* Add support for `install.cmd`/`uninstall.cmd` scripts in gallery Site Extensions
* Add support for F# Web Applications (sample [here](https://github.com/KuduApps/fsharp-owin-sample))

## S27-QFE3 (6/2/2014)
* More Site Extension tweaks

## S27-QFE2 (5/29/2014)
* Misc Site Extension and other fixes

## S27-QFE (5/8/2014)
* Improve tab auto-complete behavior in Kudu console

## S27 (5/6/2014)
* Typescript 1.0 support
* Added graceful shutdown support for WebJobs
* Improve deployment logic when disconnecting from a repo and reconnecting to a different one
* New logic to use the custom start script from package.json

## S26-QFE2 (3/31/2014)
* New Process Explorer UI
* Webhooks support for triggered WebJobs (on run complete)

## S26-QFE (3/22/2014)
* Support dropping Zip files into Dev Console
* New WebJobs dashboard for both SDK and non-SDK Jobs
* Improve Site Extension Gallery UI
* Add F# 3.1 support

## S26 (3/10/2014)
* Initial Preview of Site Extension Gallery

## S25-QFE4 (2/28/2014)
* Support new `SCM_DISABLE_SUBMODULES=1` flag to turn off git submodule update
* Add direct PowerShell support in the Debug Console
* Add auto refresh support to the Debug Console tree view when files change

## S25-QFE3 (2/11/2014)
* Add support for PowerShell WebJobs
* Add `/logs/recent` endpoint
* Add add/remove APIs for WebJobs
* Update to MVC5.1 and other newer packages
* Switch from aspx to Razor pages

## S25-QFE2 (1/30/2014)
* Add support for TypeScript
* Add Node v0.10.24 (and matching npm 1.3.21)
* Fix Kudu Console drag and drop in newest Chrome

## S25-QFE (1/16/2014)
* New [[Web Jobs]] feature
* Web hook payload now includes the site name ([issue](https://github.com/projectkudu/kudu/issues/906))
* New Kudu Console mode is now default. Can move back to old one optionally
* Correct npm version is put on the PATH ([issue](https://github.com/projectkudu/kudu/issues/950))
* Lots of [fixes](https://github.com/projectkudu/kudu/issues?direction=desc&labels=&milestone=16&page=1&sort=created&state=closed)

## S25 (12/17/2013)
* Support for deploying Console 'worker' apps
* Made SqlCmd.exe available to deployment scripts
* New API to return the list of available node/npm versions (https://github.com/projectkudu/kudu/issues/784)
* Process API now returns the list of open file handles for each process
* Updated msysgit to version 1.8.4
* Kudu now uses `d:\home` in Azure to refer to the site's files (instead of much longer folder)
* New optional alpha version of the Kudu Console that works more interactively

## S24.5 (11/21/2013)
* Added .NET 4.5.1 SDK to help build 4.5.1 projects more cleanly. Note that the **runtime** is still 4.5

## S24 (11/1/2013)
* New [Site Extension](https://github.com/projectkudu/kudu/wiki/Azure-Site-Extensions) feature
* Added diagsession and gcdump support to the [[Process API|Process Threads list and minidump gcdump diagsession]]
* Dump files now include matching mscordacwks.dll and SOS.dll for convenience
* Added threads to the [[Process API|Process Threads list and minidump gcdump diagsession]]
* New [SCM_SCRIPT_GENERATOR_ARGS](https://github.com/projectkudu/kudu/wiki/Configurable-settings#taking-over-the-script-generator-command-line) to get better control over the script generator
* Fix [selection issue](https://github.com/chrisdone/jquery-console/issues/32) in the Console.
* Moved .ssh folder to the root instead of inside the Site folder.

## S23 (8/27/2013)
* [[Web Hooks]]
* [[Post Deployment Action Hooks]]
* Enable automatic NuGet package restore, based on NuGet 2.7 semantic

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