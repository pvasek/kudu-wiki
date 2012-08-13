*This is a planned feature that is not yet implemented*

By default, Kudu drives the logic to deploy sites. See [[Deployment]] for details on how that works.

But in some scenarios, it can be interesting to override this mechanism and use custom logic to perform the deployment.

The plan is to look for a deploy.cmd file at the root of the repo. If Kudu finds that file, it calls into it and lets it perform the entire deployment instead of using its own logic.

One thing that Kudu does do before executing this file is checkout the correct files in the repo. So deploy.cmd should generally not be doing any git operations. Instead, it's working with an existing source tree that needs to be deployed.

### Environment variables

To make the batch file more useful, there are a couple environment variables that it can access:

- SOURCE: this points to where the source files are, which is normally the root of the repo
- TARGET: the target of the deployment. Typically, this is the wwwroot folder


### Working folder

Also, note that the current folder is always the root of the repo (same as %SOURCE%) when the batch file is executed.


## Examples

Some more complete examples are on [this gist](https://gist.github.com/3342182).

To take a trivial example, let's say this file contains simply:

    @echo "Hello"

When you do a 'git push', you'll actually see Hello getting displayed, and nothing else will happen. i.e. none of your files will actually get copied!

So let's say you wanted to copy all the source files to the web root, you could do something like this:

    @echo off
    echo Deploying files...
    xcopy %SOURCE% %TARGET% /Y

To process a WAP, things get more complicated, as you need to run msbuild with some pretty advanced options (which is what Kudu normally does by default). e.g. you could do:

    @echo off
    MSBuild MyWebApp\MyWebApp.csproj /t:pipelinePreDeployCopyAllFilesToOneFolder /p:_PackageTempDir="%TARGET%";AutoParameterizationWebConfigConnectionStrings=false;Configuration=Debug;SolutionDir="%SOURCE%"
    
Note that this directly deploys from the repo to wwwroot without going through any TMP folder. It also wipes out the content of wwwroot before deploying, so it doesn't do the smart copying that Kudu normally does. That's why Kudu normally deploys to TMP first, and then does smart copying to wwwroot. And you could potentially do something similar here with fancier scripts.