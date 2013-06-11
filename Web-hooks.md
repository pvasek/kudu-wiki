_Note: this feature is in development, and is not available in Azure_

**Scenario**: an external component wants to know when deployments are complete. It registers a callback url that will be called each time a deployment has finished, with the deployment information on whether the deployment was successful or not, the author, commit id and more.

**Possible use case**: With this feature you're able to hook a kudu site as a trigger in web services such as [zapier.com](https://zapier.com) which allows you to do a wide array of actions when a deployment is done, for example:
- Send an email when deployment is done
- Call when a deployment has failed
- Tweet when a deployment has succeeded
- And more...

## API ##

### Subscribe / Add Hook ###

    POST /hooks

**Body**

```
{
  "url": "http://www.callback.com/callback",
  "event": "PostDeployment",
  "insecure_ssl": false (set to true to ignore https certificate check, for test purposes only)
}
```

**Response**

201 Created or 409 Conflict (if url already exists as the address of a hook but the event is different)

```
{
  "id": 123,
  "url": "http://www.callback.com/callback",
  "event": "PostDeployment"
}
```

***

### Unsubscribe / Remove Hook ###

    DELETE /hooks/[id]

**Response**

200 OK

***

### List Hooks ###

    GET /hooks

**Response**

200 OK

```
[
  {
    "id": 123,
    "url": "http://www.callback.com/callback",
    "event": "postDeployment"
  },
  {
    ...
  }
  ...
]
```

***

### Get Hook ###

    GET /hooks/[id]

**Response**

200 OK

```
{
  "id": 123,
  "url": "http://www.callback.com/callback",
  "event": "PostDeployment",
  "last_datetime": "08/08/2013 13:24PM",
  "last_status": "OK",
  "last_reason": "internal server SSL issue or whatever",
  "last_context": "The last event content published"
}
```

***

### Hook Call ###

    POST [url]

**Body**

```
{
  "id": "cd5bee7181e74ea38a3522e73253f6ebb8ed72fb",
  "status": "success", (could be pending, building, deploying, failed, success)
  "authorEmail": "someone@somewhere.com",
  "author": "Some One",
  "message": "My fix",
  "deployer": "Some One",
  "startTime": "2013-06-06T01:24:16.5873293Z",
  "endTime": "2013-06-06T01:24:17.63342Z"
}
```

***

### Publish Hook Type ###

    POST /hooks/publish/[hook type]

**Body**

```
{
  ...
}
```

**Response**

200 OK
