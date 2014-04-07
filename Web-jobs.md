Ability to run a script (called job) in 2 different ways:

1. External trigger - run the job every time an API is called.
2. Continuous - make sure the job is always running (job should have a while loop but when it goes down we bring it back up).

Jobs are deployed by copying them to the right place in the file-system (or using a designated API which will do the same).

To deploy a triggered job copy your binaries to:
```site/wwwroot/app_data/jobs/triggered/{job name}```

To deploy a continuous job copy your binaries to:
```site/wwwroot/app_data/jobs/continuous/{job name}```

The following file types are accepted as runnable scripts that can be used as a job:

* .cmd, .bat, .exe (using windows cmd)
* .ps1 (using powershell)
* .sh (using bash)
* .php (using php)
* .py (using python)
* .js (using node)

We use the following logic to decide which file is the script to run within the job's directory:

* Per file type we look first for a file named: ```run.{file type extension}``` (for example ```run.cmd``` or ```run.exe```.
* If it doesn't exists for all file types, we'll then look for the first file with a supported file type extension.
* The order of file types extension used is: ```.cmd```, ```.bat```, ```.exe```, ```.ps1```, ```.sh```, ```.php```, ```.py```, ```.js```.
* The recommended script file to have in your job directory is: ```run.cmd```.
* Note: We'll only look for a script under the root directory of that job (not under sub directories of it).


## Configuration Settings ##

* **WEBJOBS_RESTART_TIME** - Timeout in seconds between when a continuous job's process goes down (for any reason) and the time we re-launch it again (Only for continuous jobs).
* **WEBJOBS_IDLE_TIMEOUT** - Time in seconds after which we'll abort a running triggered job's process if it's in idle, has no cpu time or output (Only for triggered jobs).
* **WEBJOBS_HISTORY_SIZE** - Maximum number of runs kept in the history directory for a triggered job.
* **WEBJOBS_STOPPED** - Set this setting to **1** to disable running any job (will also stop all currently running jobs).

## Environment Settings ##

When a job is invoked, several settings are added to it's environment that the job process can use:

* **WEBJOBS_PATH** - The root path of currently running job (will be under some temporary directory).
* **WEBJOBS_NAME** - The current job name.
* **WEBJOBS_TYPE** - The current job type (triggered/continuous).
* **WEBJOBS_DATA_PATH** - The current job meta data path, this includes the job's logs/history and any artifact of the job can go there.
* **WEBJOBS_RUN_ID** - The current run id of the job (used for triggered jobs).

## Logging ##

For **continuous** WebJobs - Console.Out and Console.Error are routed to the "application logs", they will show up as file, blob or table storage depends on your configuration of the application logs (similar to your Website).

Also the first 100 lines in each invocation will also go to the WebJob log file (to ease debugging pain when the WebJob fails at startup) accessible using the Azure portal (but also saved on your site's file system at data/jobs/continuous/jobName).

For **triggered** WebJobs - Console.Out/Console.Error are routed to the WebJobs specific run log file also accessible using the Azure portal and stored at data/jobs/triggered/jobName/runId.

Console.Out is treated (marked) as INFO and Console.Error as ERROR.
 
# API #

### List all web jobs ###

    GET /api/jobs

## Triggered Jobs ##

### List all triggered jobs ###

    GET /api/jobs/triggered

**Response**

    [
      {
        name: "jobName",
        runCommand: "...\run.cmd",
        type: "triggered",
        url: "http://.../jobs/triggered/jobName",
        history_url: "http://.../jobs/triggered/jobName/history",
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
            url: "http://.../jobs/triggered/jobName/history/20131103120400"
          }
      }
    ]

### Get a specific triggered job by name ###

    GET /api/jobs/triggered/{job name}

**Response**

    {
      name: "jobName",
      runCommand: "...\run.cmd",
      type: "triggered",
      url: "http://.../jobs/triggered/jobName",
      history_url: "http://.../jobs/triggered/jobName/history",
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
          url: "http://.../jobs/triggered/jobName/history/20131103120400"
        }
    }

### Upload a triggered job as zip ###

Using a zip file containing the files for it, or just a single file (e.g foo.exe).

    PUT /api/zip/site/wwwroot/App_Data/jobs/triggered/{job name}/

or (in the next version not out yet)

    PUT /api/jobs/triggered/{job name}

    Use "Content-Type: application/zip" for zip otherwise it's treated as a regular script file.

	The file name should be in the Content-Dispostion header, example:
    Content-Disposition: attachement; filename=run.cmd


### Delete a triggered job ###

    DELETE /api/vfs/site/wwwroot/App_Data/jobs/triggered/{job name}?recursive=true

or (in the next version not out yet)

    DELETE /api/jobs/triggered/{job name}


### Invoke a triggered job ###

    POST /api/jobs/triggered/{job name}/run

Note: if the site has multiple instances, the job will run on one of them arbitrarily. This is the same behavior as regular requests sent to the site.

### List all triggered job runs history ###

    GET /api/jobs/triggered/{job name}/history

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
            url: "http://.../jobs/triggered/jobName/history/20131103120400"
          }
        ]
    }

### Get a specific run for a specific triggered job ###

    GET /api/jobs/triggered/{job name}/history/{id}

**Response**

    {
      id: "20131103120400",
      status: "Success",
      start_time: "2013-11-08T02:56:00.000000Z",
      end_time: "2013-11-08T02:57:00.000000Z",
      duration: "00:01:00",
      output_url: "http://.../vfs/data/jobs/triggered/jobName/20131103120400/output_20131103120400.log",
      error_url: "http://.../vfs/data/jobs/triggered/jobName/20131103120400/error_20131103120400.log",
      url: "http://.../jobs/triggered/jobName/history/20131103120400"
    }

## Continuous Jobs ##

### List all continuous jobs ###

    GET /api/jobs/continuous

**Response**

    [
      {
        name: "jobName",
        status: "Running",
        runCommand: "...\run.cmd",
        log_url: "http://.../vfs/data/jobs/continuous/jobName/job.log",
        extra_info_url: "http://.../",
        url: "http://.../jobs/continuous/jobName",
        type: "continuous"
      }
    ]

### Get a specific continuous job by name ###

    GET /api/jobs/continuous/{job name}

**Response**

    {
      name: "jobName",
      status: "Running",
      runCommand: "...\run.cmd",
      log_url: "http://.../vfs/data/jobs/continuous/jobName/job.log",
      extra_info_url: "http://.../",
      url: "http://.../jobs/continuous/jobName",
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

or (in the next version not out yet)

    PUT /api/jobs/continuous/{job name}

    Use "Content-Type: application/zip" for zip otherwise it's treated as a regular script file.

	The file name should be in the Content-Dispostion header, example:
    Content-Disposition: attachement; filename=run.cmd


### Delete a continuous job ###

    DELETE /api/vfs/site/wwwroot/App_Data/jobs/continuous/{job name}?recursive=true

or (in the next version not out yet)

    DELETE /api/jobs/continuous/{job name}

### Start a continuous job ###

    POST /api/jobs/continuous/{job name}/start

### Stop a continuous job ###

    POST /api/jobs/continuous/{job name}/stop

### Set a continuous job as singleton ###

If a continuous job is set as singleton it'll run only on a single instance opposed to running on all instances.

    PUT /api/jobs/continuous/{job name}/settings

Body

    { "is_singleton": true }

> To set a continuous job as singleton during deployment (without the need for the REST API) you can simply create a file called ```settings.job``` with the content: ```{ "is_singleton": true }``` and put it at the root of the (specific) WebJob directory.
