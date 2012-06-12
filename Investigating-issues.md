This page gives instructions on investigating git issues that occur in Azure Web Sites.

# How to do initial investigation

## Make sure the site itself works

When you run into a git issue, the first thing to try is to hit the site itself (e.g. http://somesite.azurewebsites.net/). If you get some kind of error instead of the default _"The web site is under construction"_ page, then Azure Web Sites are likely down, and nothing related to git will work.

## Try hitting the root of the git service

If the main site is up but Kudu is broken in some way, try hitting the root of the Kudu service directly from the browser. This is useful whether you fail to Enable Git, get a failure on the Deployment tab, or get a failure when you git push.

e.g. if your site URL is http://somesite.azurewebsites.net/, the kudu service root URL will be **https**://somesite.**scm**.azurewebsites.net/. So use https, and add the scm token. You will be prompted for credentials, which are the same as what you use to 'git push'.

### If you get an error BEFORE getting prompted for credentials

That usually means something pretty bad is wrong, causing things to fail before it even reaches the git engine.

### If you get an error AFTER getting prompted for credentials

That could mean two different things:

1. An issue with Azure Web Sites
2. An issue with the git support itelf

Record what the error is and report it.

## Getting the git service log

If you are able to hit the git service (per previous section), but you get an error during a git command (e.g. push/pull/clone), the next step is to download the git service log.

To do this:

* Go to the root of the git service (per previous section)
* On that page, you'll see a Download link next to 'Diagnostics Dump'
* Just click it to download a zip file which you can provide to help investigation
