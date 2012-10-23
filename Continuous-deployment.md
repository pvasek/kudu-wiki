Steps to set up continuous deployment from an externally hosted repo (e.g. GitHub/CodePlex/Bitbucket):

## Getting the hook URL

The hook URL is the /deploy path on the Kudu service. So it looks like `https://mysite.scm.azurewebsites.net/deploy`. But since the service uses basic auth, you need to pass the creds in the URL. Typically, you'll want to use the site-level credentials here and not the user credentials (see [[Deployment-credentials]]). So the full URL will look like `https://$mysite:BigRandomPassword@mysite.scm.azurewebsites.net/deploy`

Note that you can also obtain this URL from the Azure Portal under the Configure tab, in the Deployment Trigger Url field.

One you have the URL, you can set it as a GitHub/CodePlex/Bitbucket hook, either via OAuth, or manually from those sites.

The hook expects a POST request, and supports the payload from those various sites (which are all different).


## Giving the private key to the Kudu service

If the repo is private, you will also need to set up an SSH key.

The public key is given to GitHub/Bitbucket, either manually or through their OAuth API.

The private key is given to the Kudu service. This can be done in two ways:

### Using the REST API

 using the PUT `/sshkey` REST API. The supported key format is privacy enhanced mail (PEM). Since Kudu runs as a service (no interactive), make sure the private key PEM is not encrypted with `passphrase`. The ssh-keygen command, by default, will solicit passphrase, do leave it blank.

One possible way to call the REST API is using curl. e.g.

    curl --user yourname -X PUT -H "Content-Type: application/json; charset=UTF-8" --data "{key: 'full private key goes here!'}" https://yoursite.scm.azurewebsites.net/sshkey

### Manually over ftp (this is probably simpler)

You need to copy two files over FTP (see [[Accessing-files-via-ftp]] and [[File-structure-on-azure]]).

Under the `site` folder, create a .ssh folder and create two files.

The first is named config (so `site\.ssh\config`) and should contain exactly:

    HOST *
    	  StrictHostKeyChecking no

The second is `id_rsa` and contains, your private key, e.g.

    -----BEGIN RSA PRIVATE KEY-----
    MIIEoAIBAAKCAQEAxFkKAmZSruEVhq4g0LsDTBiKe/FXYcy1dzi/eHd4Cw32bq9H
    EpUBFrMykK3TVc/pdL9PVD/oq3ikvAkDeZFYUDMso4vvMf5/FfmU2oCwRaESF545
    etc...
    -----END RSA PRIVATE KEY-----
    
## Setting up continuous deployment from the portal after a manual push

If you have done any 'git push' directly to the Azure repo, the option to set up continous deployment disappears from the Deployment page in the portal.

Ideally, there would be a way to bring it back, but until then you'll have to use a workaround:

- Access your sites file using FTP (see [[Accessing-files-via-ftp]])
- Got into the `site` folder (see [[File-structure-on-azure]])
- Delete the entire `deployments` folder
- Back in the portal, the Deployment page should offer to set up Continuous Deployment again.
