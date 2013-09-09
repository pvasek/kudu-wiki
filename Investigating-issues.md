This page gives instructions on investigating git issues that occur in Azure Web Sites.

# How to do initial investigation

## Make sure the site itself works

When you run into an issue with a Kudu feature (e.g. git/hg/dropbox deployment), the first thing to try is to hit the site itself (e.g. http://yoursite.azurewebsites.net/). It it seems completely unreachable, then there is a chance that you're dealing with some kind of WAWS outage, which would also disable the Kudu features. When that happens, please check the [Azure Service Dashboard](http://www.windowsazure.com/en-us/support/service-dashboard/) to see if some outage is mentioned.

## Try hitting the root of the git service

If the site is up but a Kudu feature is broken in some way, try hitting the [[root of the Kudu service|Accessing-the-kudu-service]] directly from the browser. This is useful whether you fail to Enable Git, get a failure on the Deployment tab, or get a failure when you git push.

If you get an error, it could mean that there is something wrong with Kudu. Please record what the error is and report it to the [forum](http://social.msdn.microsoft.com/Forums/en-US/azuregit/threads)

## Live traces

See [[Diagnostic Log Stream]].

## Getting the diagnostic dump

If you are able to hit the root of the Kudu service (per previous section), but you get an error during a git command (e.g. push/pull/clone), the next step is to download the Kudu service log.

To do this:

* Go to the [[root of the Kudu service|Accessing-the-kudu-service]]
* On that page, you'll see a Download link next to 'Diagnostics Dump'
* Just click it to download a zip file which you can provide to help investigation

## Error types

* **403** - You don't have access to this site
* **404** - See [[Known issues]]
* **409** - There's a deployment in progress.
* **501** - Git tried to use the dumb protocol and we don't support this ([[Anatomy of a git request]]), this is because the initial request failed.
* **502, 503** - Something went wrong before the request reached the Kudu service.

To get more details for any of the above status codes:

Capture the git client trace for more details (see below).

### Capturing the git http verbose output

1. First set GIT_CURL_VERBOSE to 1. The syntax varies depending on the shell:
  * git bash: `export GIT_CURL_VERBOSE=1`
  * cmd.exe: `set GIT_CURL_VERBOSE=1`
  * Powershell: `$env:GIT_CURL_VERBOSE=1`
2. Run your git command. (e.g. git push url master). **NOTE: To capture output you need to redirect the error stream not output!**
3. [[Analyzing a git client trace]].