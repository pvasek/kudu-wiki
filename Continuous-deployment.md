## Setting up continuous deployment from the Azure portal

The Azure portal makes it easy to set up continuous deployment from GitHub or Bitbucket.

This documents discusses what happens under the cover. In most cases you don't need to worry about this. It can be useful to set up continuous deployment from a git host that the portal doesn't directly support, like GitLab.

## Create an Azure Website with local git enabled

When setting things up manually as described here, you'll need to enable the 'Local Git repository' option in the portal for your site. Even though you will in fact set up continuous deployment, this is the option that allows things to work.  

## Set up a Web Hook so Kudu gets notified when a deployment happens

The hook URL is the `/deploy path` on the Kudu service. So it looks like `https://mysite.scm.azurewebsites.net/deploy`. But since the service uses basic auth, you need to pass the creds in the URL. Typically, you'll want to use the site-level credentials here and not the user credentials (see [[Deployment-credentials]]). So the full URL will look like `https://$mysite:BigRandomPassword@mysite.scm.azurewebsites.net/deploy`

The easiest way to get this URL is to directly copy it from the Azure Portal. You'll find it in the Configure tab, in the Deployment Trigger Url field.

Once you have the URL, you can set it as a GitHub/Bitbucket/GitLab hook.


## For private repos, set up a deploy key

If the repo is private, you will also need to set up an SSH 'deploy key' on GitHub/Bitbucket/GitLab.

This can be done using the following steps:
- Take the full deploy URL above, and replace `/deploy` with `/api/sshkey?ensurePublicKey=1`. So it'll look like `https://$mysite:BigRandomPassword@mysite.scm.azurewebsites.net/api/sshkey?ensurePublicKey=1`
- Run `curl` on this URL, which returns an SSH public key. It's returned as a JSON string, so you'll need to remove the quotes. It should look like `ssh-rsa AAAAB3NzaC1etc...`
- Set this string as a 'deploy key' on your GitHub/Bitbucket/GitLab repository.

## Test that everything works

The easiest way to test it is to got to GitHub/Bitbucket/GitLab and click 'test' on the Web Hook you set up. Or you can try simply pushing a chance to your repo. 