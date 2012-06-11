Preliminary notes in the deployment hook feature

## Non-WAP ##

- git push --> git database update
- Checkout
- NEW: pre-deployment hook
- xcopy to wwwroot (possibly subfolder per .deployment)
- NEW: post-deployment hook
- Run npm (maybe only if there is no post-deployment hook?)


## WAP ##

- git push --> git database update
- Checkout
- NEW: pre-build hook (e.g. like NuGet package restore)
- locate csproj and run msbuild, producing artifacts in TEMP
- NEW: post-build (aka pre-deployment) hook
- xcopy artifacts to wwwroot
- NEW: post-deployment hook
