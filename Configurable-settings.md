There are various things that you can configure using App Settings, which you can find on the Configure page in the Windows Azure portal.

Note that in addition to using App Settings, you can specify those settings in your [[.deployment file|Customizing-deployments]]. This is useful if you want the setting to be part of your repository.


## Repository and deployment related settings

### Using a git shallow clone in Continous Deployment scenarios

For large repos, you can make Kudu use a shallow clone when it clones your repo from GitHub or Bitbucket, which can save disk space. Shallow clones can be tricky, so make sure you understand what they are before using this. It is off by default. Tu turn it on:

	SCM_USE_SHALLOW_CLONE=1

### Changing the repo and deployment paths, and not using a repo at all

Please see [[Deploying inplace and without repository]] for information on using the `SCM_REPOSITORY_PATH`, `SCM_NO_REPOSITORY`, `PROJECT` and `SCM_TARGET_PATH` flags.

### Adding flags to the msbuild command line

Use this flag to add things to the msbuild command line. e.g. to choose the build configuration, you could have:

    SCM_BUILD_ARGS=/p:Configuration=Release


## Diagnostic related settings

### Changing the trace level

By default, it is set to 1, but you can get more tracing with higher values, up to 4. e.g.

	SCM_TRACE_LEVEL=4

### Changing the timeout before external commands are killed

By default, when your build process launches some command, it's allowed to run for up to 60 seconds without producing any output. If that is not long enough, you can make it longer, e.g. to make it 10 minutes:

	SCM_COMMAND_IDLE_TIMEOUT=600


### Changing the timeout of the log streaming feature

When using the log streaming feature, by default it times out after 30 minutes of inactivity. To change it to 15 minutes (unit is seconds):

	SCM_LOGSTREAM_TIMEOUT=900
