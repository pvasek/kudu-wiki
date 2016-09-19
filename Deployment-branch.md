By default, only pushes to the **master** branch get deployed. But you can change this by calling a REST API on the Kudu service. One simple way to do this is to use curl.

Here is an example:

```bash
# Switch branch (/settings endpoint)
$ curl -H "Content-Type: application/json; charset=UTF-8" \
       --data "{key: 'branch', value: 'anotherbranch' }" \
       https://user:pass@site.scm.azurewebsites.net/settings


$ cat body.json
{
    "format": "basic",
    "url": "https://github.com/username/projectname"
}


# Trigger a deployment (branch name is read from /settings)
$ curl -X POST https://user:pass@site.scm.azurewebsites.net/deploy \
       --data @body.json \
       -H "Content-type: application/json"
```

Which results in:<br>
![anotherbranch](https://cloud.githubusercontent.com/assets/6472374/18649223/fefa65aa-7ec6-11e6-8696-2b89bec5147e.png)

The Azure portal (portal.azure.com) also has UI to change this:<br>
**Deployment options** blade -> **Disconnect** -> **Setup** -> **Choose branch**.