Steps to set up continuous deployment from an externally hosted repo (e.g. GitHub/CodePlex/Bitbucket):

## Getting the hook URL

The hook URL is the /deploy path on the Kudu service. So it looks like `https://mysite.scm.azurewebsites.net/deploy`. But since the service uses basic auth, you need to pass the creds in the URL. Typically, you'll want to use the site-level credentials here and not the user credentials (see [[Deployment-credentials]]). So the full URL will look like `https://$mysite:BigRandomPassword@mysite.scm.azurewebsites.net/`

It expects a POST, and supports the payload from those various sites (which are all different).

Note that you can also obtain this URL from the Azure Portal under the Configure tab, in the Deployment Trigger Url field.


## Giving the private key to the Kudu service

If the repo is private, you will also need to set up an SSH key.

The public key is given to GitHub/CodePlex/Bitbucket, either manucally or through their OAuth API.

The private key is given to the Kudu service using the /sshkey RES API. The supported key format is privacy enhanced mail (PEM).
