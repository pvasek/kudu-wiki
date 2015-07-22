### You get a 404 during 'git push'

That can happen when you have a large site, and you have the git http.postBuffer setting set to a high value.

The workaround is to unset it, using:

    git config --global --unset http.postBuffer

### post-receive issue with remote: hooks/post-receive: line 4: D:\Program: No such file or directory

Simply follow the step in this [repository](https://github.com/projectkudu/FixPostReceive)