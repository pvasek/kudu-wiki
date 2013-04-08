## S21
* Add optional support for shallow clones (to save disk space) via SCM_USE_SHALLOW_CLONE setting
* Add etag support to /deployments API to avoid getting data unless necessary
* Improve progress reporting during deployment
* Allow the root of the Kudu service to be used as an alternate git endpoint
* Trim old deployments after reaching some limit
* Several improvement to the diagnostic trace file

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

## S20
* New support for deploying Mercurial sites on Bitbucket and Codeplex
* New support for deploying from Dropbox
* New support zip download/upload API
