### Git publishing doesn't work when the site is set to use .NET 2.0

The workaround here is to set the site to run .NET 4.0. Most 2.0 and 3.5 sites should work fine under 4.0.


### You get a 404 durting 'git push'

That can happen when you have a large site, and you have the git http.postBuffer setting set to a high value.

The workaround is to unset it, using:

    git config --global --unset http.postBuffer
