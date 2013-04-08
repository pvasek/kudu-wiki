## S21
* Add optional support for shallow clones (to save disk space) via SCM_USE_SHALLOW_CLONE setting
* Add etag support to /deployments API to avoid getting data unless necessary
* Improve progress reporting during deployment
* Trim old deployments after reaching some limit
* Several improvement to the diagnostic trace file

### Perf improvements
* Cache deployment script to avoid having to generate it each time
* Use unbuffered input stream during git push operations

### Key fixes
* Fixed issue that was causing some failures git pushing from Macs
* Improved detection for processes that are waiting for user input
* Improved recovery after a crash (previously git locks and unwanted git remotes would remain)

## S20
* New support for deploying Mercurial sites on Bitbucket and Codeplex
* New support for deploying from Dropbox
* New support zip download/upload API
