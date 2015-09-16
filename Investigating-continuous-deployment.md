This is a guide to help investigate why the continuous deployment from GitHub or Bitbucket not working. 

## Look at service log  

Follow the "Get git service log" step in [[Investigating issues]].   Once you have a zipped logfile, look for Git\trace\trace.xml.  This should contain all the incoming/outgoing request logs.  See if we have received any POST /deploy request from GitHub or Bitbucket (notification of repo push) or any issues.   You may share us that trace.xml and we can help investigate.    

## Check service hook settings

If you can't find any related requests in trace.xml, we may have trouble receiving anything from GitHub or Bitbucket in the first place.  Look at your service hook settings in GitHub or Bitbucket.  For instance, in Bitbucket, it is under Services section of your repo settings. See if the azure service url (https://...scm.../deploy) is registered.

## Try POST simple payload

If there is, try POST simple payload to that URL.   The payload need to just contain repo url info such as {"url":"https://.../myrepo"}; in case of SSH, {"url":"git@...:user/myrepo"}.   You may also pass is_hg:true if your repo is mercurial.  Check if there is a deployment to your site.

## Verify Service hook

Create a http request bin at http://requestb.in/ and add it as one of Services of your repo setting.  Push to your repo and see if there exist any POST requests.  If not, then contact GitHub or Bitbucket.

Here is the Bitbucket status page: http://status.bitbucket.org/