_Note: this feature is in development, and is not available in Azure_

The web hooks feature is about being able to register a (callback) url(s) that will be called once a deployment has finished with the deployment information on whether the deployment was successful or not, the author, commit id and more.

With this feature we'll be able to hook a kudu site as a trigger in web services such as [zapier.com](https://zapier.com) which will allow a user to do a wide array of actions when a deployment is done, for example:
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
  "event": "PostDeployment"
}
```

**Response**

201 Created or 409 Conflict (if url already exists as a hook)

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
    "event": "PostDeployment"
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
  "event": "PostDeployment"
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
  "receivedTime": "2013-06-06T01:24:12.1077132Z",
  "startTime": "2013-06-06T01:24:16.5873293Z",
  "endTime": "2013-06-06T01:24:17.63342Z",
  "lastSuccessEndTime": "2013-06-06T01:24:17.63342Z",
  "complete": true
}
```
