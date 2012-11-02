### Git publishing doesn't work when the site is set to use .NET 2.0

The workaround here is to set the site to run .NET 4.0. Most 2.0 and 3.5 sites should work fine under 4.0.


### You get a 404 during 'git push'

That can happen when you have a large site, and you have the git http.postBuffer setting set to a high value.

The workaround is to unset it, using:

    git config --global --unset http.postBuffer

### Create Application (Kudu service portal) is "stuck"

One of the steps "Create Application" is doing is to grant security ACLs to a new user by the application's name to the c:\windows\temp folder, this can be a long process (and may take up system resources) if this folder is large (> GB).
