There are various things that you can configure using App Settings, which you can find on the Configure page in the Windows Azure portal.

## Adding flags to the msbuild command line

Use the `SCM_BUILD_ARGS` to add things to the msbuild command line. e.g. to choose the build configuration, you could have:

    SCM_BUILD_ARGS=/p:Configuration=Release
