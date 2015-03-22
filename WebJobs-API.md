### List all web jobs ###

    GET /api/webjobs

## Triggered Jobs ##

### List all triggered jobs ###

    GET /api/triggeredwebjobs

**Response**

    [
      {
        name: "jobName",
        runCommand: "...\run.cmd",
        type: "triggered",
        url: "http://.../triggeredwebjobs/jobName",
        history_url: "http://.../triggeredwebjobs/jobName/history",
        extra_info_url: "http://.../",
        latest_run:
          {
            id: "20131103120400",
            status: "Success",
            start_time: "2013-11-08T02:56:00.000000Z",
            end_time: "2013-11-08T02:57:00.000000Z",
            duration: "00:01:00",
            output_url: "http://.../vfs/data/jobs/triggered/jobName/20131103120400/output_20131103120400.log",
            error_url: "http://.../vfs/data/jobs/triggered/jobName/20131103120400/error_20131103120400.log",
            url: "http://.../triggeredwebjobs/jobName/history/20131103120400"
          }
      }
    ]

### List all triggered jobs in swagger format###

    GET /api/triggeredwebjobsswagger

**Response**
      {
        name: "jobName",
        runCommand: "...\run.cmd",
        type: "triggered",
        url: "http://.../triggeredwebjobs/jobName",
        history_url: "http://.../triggeredwebjobs/jobName/history",
        extra_info_url: "http://.../",
        latest_run:
          {
            id: "20131103120400",
            status: "Success",
            start_time: "2013-11-08T02:56:00.000000Z",
            end_time: "2013-11-08T02:57:00.000000Z",
            duration: "00:01:00",
            output_url: "http://.../vfs/data/jobs/triggered/jobName/20131103120400/output_20131103120400.log",
            error_url: "http://.../vfs/data/jobs/triggered/jobName/20131103120400/error_20131103120400.log",
            url: "http://.../triggeredwebjobs/jobName/history/20131103120400"
          }
      }

### Get a specific triggered job by name ###

    GET /api/triggeredwebjobs/{job name}

**Response**

    {
      "swagger": "2.0",
      "info": {
        "version": "v1",
        "title": "WebJobs"
      },
      "host": "placeHolder",
      "schemes": [
        "https"
      ],
      "paths": {
        "/api/triggeredjobs/jobName/run": {
          "post": {
            "deprecated": false,
            "operationId": "jobName",
            "consumes": [],
            "produces": [],
            "responses": {
              "200": {
                "description": "Success"
              },
              "default": {
                "description": "Success"
              }
            },
            "parameters": [
              {
                "name": "arguments",
                "in": "query",
                "description": "Web Job Arguments",
                "required": false,
                "type": "string"
              }
            ]
          }
        }
      }
    }

### Upload a triggered job as zip ###

Using a zip file containing the files for it, or just a single file (e.g foo.exe).

    PUT /api/zip/site/wwwroot/App_Data/jobs/triggered/{job name}/

or

    PUT /api/triggeredwebjobs/{job name}

    Use "Content-Type: application/zip" for zip otherwise it's treated as a regular script file.

	The file name should be in the Content-Dispostion header, example:
    Content-Disposition: attachement; filename=run.cmd


### Delete a triggered job ###

    DELETE /api/vfs/site/wwwroot/App_Data/jobs/triggered/{job name}?recursive=true

or

    DELETE /api/triggeredwebjobs/{job name}


### Invoke a triggered job ###

    POST /api/triggeredwebjobs/{job name}/run

**To run with arguments use the arguments parameters that will be added to the script when invoked**

    POST /api/triggeredwebjobs/{job name}/run?arguments={arguments}

Note: if the site has multiple instances, the job will run on one of them arbitrarily. This is the same behavior as regular requests sent to the site.

### List all triggered job runs history ###

    GET /api/triggeredwebjobs/{job name}/history

**Response**

    {
      runs:
        [
          {
            id: "20131103120400",
            status: "Success",
            start_time: "2013-11-08T02:56:00.000000Z",
            end_time: "2013-11-08T02:57:00.000000Z",
            duration: "00:01:00",
            output_url: "http://.../vfs/data/jobs/triggered/jobName/20131103120400/output_20131103120400.log",
            error_url: "http://.../vfs/data/jobs/triggered/jobName/20131103120400/error_20131103120400.log",
            url: "http://.../triggeredwebjobs/jobName/history/20131103120400"
          }
        ]
    }

### Get a specific run for a specific triggered job ###

    GET /api/triggeredwebjobs/{job name}/history/{id}

**Response**

    {
      id: "20131103120400",
      status: "Success",
      start_time: "2013-11-08T02:56:00.000000Z",
      end_time: "2013-11-08T02:57:00.000000Z",
      duration: "00:01:00",
      output_url: "http://.../vfs/data/jobs/triggered/jobName/20131103120400/output_20131103120400.log",
      error_url: "http://.../vfs/data/jobs/triggered/jobName/20131103120400/error_20131103120400.log",
      url: "http://.../triggeredwebjobs/jobName/history/20131103120400"
    }

## Continuous Jobs ##

### List all continuous jobs ###

    GET /api/continuouswebjobs

**Response**

    [
      {
        name: "jobName",
        status: "Running",
        runCommand: "...\run.cmd",
        log_url: "http://.../vfs/data/jobs/continuous/jobName/job.log",
        extra_info_url: "http://.../",
        url: "http://.../continuouswebjobs/jobName",
        type: "continuous"
      }
    ]

### Get a specific continuous job by name ###

    GET /api/continuouswebjobs/{job name}

**Response**

    {
      name: "jobName",
      status: "Running",
      runCommand: "...\run.cmd",
      log_url: "http://.../vfs/data/jobs/continuous/jobName/job.log",
      extra_info_url: "http://.../",
      url: "http://.../continuouswebjobs/jobName",
      type: "continuous"
    }

The `status` can take the following values:

- Initializing
- Starting
- Running
- PendingRestart
- Stopped

### Upload a continuous job as zip ###

Using a zip file containing the files for it.

    PUT /api/zip/site/wwwroot/App_Data/jobs/continuous/{job name}/

or

    PUT /api/continuouswebjobs/{job name}

    Use "Content-Type: application/zip" for zip otherwise it's treated as a regular script file.

	The file name should be in the Content-Dispostion header, example:
    Content-Disposition: attachement; filename=run.cmd


### Delete a continuous job ###

    DELETE /api/vfs/site/wwwroot/App_Data/jobs/continuous/{job name}?recursive=true

or

    DELETE /api/continuouswebjobs/{job name}

### Start a continuous job ###

    POST /api/continuouswebjobs/{job name}/start

### Stop a continuous job ###

    POST /api/continuouswebjobs/{job name}/stop

### Get continuous job settings ###

    GET /api/continuouswebjobs/{job name}/settings

**Response**

    {
      "is_singleton": true
    }

### Set a continuous job as singleton ###

If a continuous job is set as singleton it'll run only on a single instance opposed to running on all instances.

    PUT /api/continuouswebjobs/{job name}/settings

Body

    {
      "is_singleton": true
    }

> To set a continuous job as singleton during deployment (without the need for the REST API) you can simply create a file called ```settings.job``` with the content: ```{ "is_singleton": true }``` and put it at the root of the (specific) WebJob directory.
