### Getting a function

    get /subscriptions/{sub}/resourceGroups/{rg}/providers/Microsoft.Web/functions?api-version=2015-08-01

Response:

```json
{
  "id": "/subscriptions/5f6d41f5-c638-4ef0-94be-15784b3c4be9/resourceGroups/MyFuncRG/providers/Microsoft.Web/functions/MyFunction",
  "name": "MyFunction",
  "type": "Microsoft.Web/functions",
  "location": "West US",
  "properties": {
    "name": "MyFunction",
    "function_app_id": "/subscriptions/5f6d41f5-c638-4ef0-94be-15784b3c4be9/resourceGroups/MyFuncRG/providers/Microsoft.Web/sites/funcapp123",
    "trigger_url": "https://funcapp123.azurewebsites.net/api/MyFunction?code=AlLHwKCuXxFjqOCWFvGiBJNJKIR7zBeHnZM0GbqwJLT32SVU6Kq3Ag==",
    "script_root_path_href": "https://funcapp123.scm.azurewebsites.net/api/vfs/site/wwwroot/MyFunction/",
    "script_href": "https://funcapp123.scm.azurewebsites.net/api/vfs/site/wwwroot/MyFunction/index.js",
    "config_href": "https://funcapp123.scm.azurewebsites.net/api/vfs/site/wwwroot/MyFunction/function.json",
    "secrets_file_href": "https://funcapp123.scm.azurewebsites.net/api/vfs/data/functions/secrets/MyFunction.json",
    "config": {
      "bindings": [
        {
          "webHookType": "genericJson",
          "direction": "in",
          "type": "httpTrigger"
        },
        {
          "type": "http",
          "direction": "out"
        }
      ]
    }
  }
}
```

### Listing functions in a resource group 

    get /subscriptions/{sub}/resourceGroups/{rg}/providers/Microsoft.Web/functions?api-version=2015-08-01

Response:

```json
{
  "value": [
    {
      ...
    }
  ]
}
```

### Creating a function in a resource group 

    put /subscriptions/{sub}/resourceGroups/{rg}/providers/Microsoft.Web/functions/MyFunction?api-version=2015-08-01

Request body (sample):

```json
{
  "location": "West US",
  "properties": {
    "config": {
      "bindings": [
        {
          "webHookType": "genericJson",
          "type": "httpTrigger",
          "direction": "in",
          "name": "req"
        },
        {
          "type": "http",
          "direction": "out",
          "name": "res"
        }
      ],
      "disabled": false
    },
    "files": {
      "index.js": "module.exports = function (context, data) {\r\n    context.res = {\r\n        body: { greeting: 'Hello ' + data.first + ' ' + data.last + '!'}\r\n    };\r\n\r\n    context.done();\r\n};\r\n"
    },
    "test_data": "{ 'name': 'David' }"
  }
}
```

Response: same as doing a GET above.