## Q: Can't see the repositories in the drop down!

You need to be an admin on the repositories.  To check if you are an admin of the repo, ...
* For github, see if you can browse to https://github.com/{account}/{repo}/settings/hooks.
* For bitbucket, see if you can browse to https://bitbucket.org/{account}/{repo}/admin/hooks.

## Q: Still can't see github org repositories in the drop down!

Github has recently required an explicit authorization for OAuth application to access the org repositories.  To grant the authorization, ...
* Browse to https://github.com/settings/applications
* Click `Azure Management Portal`.   For old Portal, click `Windows Azure`.
* Click `Grant access` to the specific organization