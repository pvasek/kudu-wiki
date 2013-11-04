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
* .sh (using bash)
* .php (using php)
* .py (using python)
* .js (using node)

We use the following logic to decide which file is the script to run within the job's directory:

* Per file type we look first for a file named: ```run.{file type extension}``` (for example ```run.cmd``` or ```run.exe```.
* If it doesn't exists for all file types, we'll then look for the first file with a supported file type extension.
* The order of file types extension used is: ```.cmd```, ```.bat```, ```.exe```, ```.sh```, ```.php```, ```.py```, ```.js```.
* The recommended script file to have in your job directory is: ```run.cmd```.
* Note: We'll only look for a script under the root directory of that job (not under sub directories of it).

# API #

### List all web jobs ###

    GET jobs

## Triggered Jobs ##

### List all triggered jobs ###

    GET jobs/triggered

**Response**

    [
      {
        name: "jobName",
        runCommand: "...\run.cmd",
        url: "http://.../jobs/triggered/jobName",
        history_url: "http://.../jobs/triggered/jobName/history",
        log_url: "http://.../vfs/data/jobs/triggered/jobName/job.log",
        latest_run:
          {
            id: "20131103120400",
            status: "Running",
            output_url: "http://.../vfs/data/jobs/triggered/jobName/20131103120400/output_20131103120400.log",
            error_url: "http://.../vfs/data/jobs/triggered/jobName/20131103120400/error_20131103120400.log",
            url: "http://.../jobs/triggered/jobName/history/20131103120400"
          }
      }
    ]

### Get a specific triggered job by name ###

    GET jobs/triggered/{job name}

**Response**

    {
      name: "jobName",
      status: "Running",
      runCommand: "...\run.cmd",
      url: "http://.../jobs/triggered/jobName",
      history_url: "http://.../jobs/triggered/jobName/history",
    }

### Add/Replace a triggered job ###

Using a zip file containing the files for it, or just a single file (e.g foo.exe).

    PUT jobs/triggered/{job name}

### Delete a triggered job ###

    DELETE jobs/triggered/{job name}

### Invoke a triggered job ###

    POST jobs/triggered/{job name}/run

### Stop a triggered job ###

    POST jobs/triggered/{job name}/stop

### List all triggered job runs history ###

    GET jobs/triggered/{job name}/history

**Response**

    {
	  log_url: "http://.../vfs/data/jobs/continuous/jobName/job.log",
      runs:
        [
          {
            id: "20131103120400",
            status: "Running",
            output_url: "http://.../vfs/data/jobs/triggered/jobName/20131103120400/output_20131103120400.log",
            error_url: "http://.../vfs/data/jobs/triggered/jobName/20131103120400/error_20131103120400.log",
            url: "http://.../jobs/triggered/jobName/history/20131103120400"
          }
        ]
    }

### Get a specific run for a specific triggered job ###

    GET jobs/triggered/{job name}/history/{id}

**Response**

    {
      id: "20131103120400",
      status: "Running",
      output_url: "http://.../vfs/data/jobs/triggered/jobName/20131103120400/output_20131103120400.log",
      error_url: "http://.../vfs/data/jobs/triggered/jobName/20131103120400/error_20131103120400.log",
      url: "http://.../jobs/triggered/jobName/history/20131103120400"
    }

## Continuous Jobs ##

### List all continuous jobs ###

    GET jobs/continuous

**Response**

    [
      {
        name: "jobName",
        status: "Running",
        runCommand: "...\run.cmd",
        url: "http://.../jobs/continuous/jobName",
        history_url: "http://.../jobs/continuous/jobName/history"
      }
    ]

### Get a specific continuous job by name ###

    GET jobs/continuous/{job name}

**Response**

    {
      name: "jobName",
      status: "Running",
      runCommand: "...\run.cmd",
      url: "http://.../jobs/continuous/jobName",
      history_url: "http://.../jobs/continuous/jobName/history"
    }

### Add/Replace a continuous job ###

Using a zip file containing the files for it, or just a single file (e.g foo.exe).

    PUT jobs/continuous/{job name}

### Delete a continuous job ###

    DELETE jobs/continuous/{job name}

### Start a continuous job ###

    POST jobs/continuous/{job name}/run

### Stop a continuous job ###

    POST jobs/continuous/{job name}/stop

### Get continuous job history ###

    GET jobs/continuous/{job name}/history

**Response**

    {
	  log_url: "http://.../vfs/data/jobs/continuous/jobName/job.log",
	  output_url: "http://.../vfs/data/jobs/continuous/jobName/output_20131103120400.log",
	  error_url: "http://.../vfs/data/jobs/continuous/jobName/error_20131103120400.log"
      runs:
        [
          {
            id: "ProcessBlob_20131103120400",
            status: "Success",
            output_url: "http://.../vfs/data/jobs/continuous/jobName/ProcessBlob_20131103120400/output_20131103120400.log",
            error_url: "http://.../vfs/data/jobs/continuous/jobName/ProcessBlob_20131103120400/error_20131103120400.log",
            url: "http://.../jobs/triggered/jobName/history/ProcessBlob_20131103120400"
          }
        ]
    }

### Get a specific run for a specific continuous job ###

    GET jobs/continuous/{job name}/history/{id}

**Response**

      {
        id: "ProcessBlob_20131103120400",
        status: "Success",
        output_url: "http://.../vfs/data/jobs/continuous/jobName/ProcessBlob_20131103120400/output_20131103120400.log",
        error_url: "http://.../vfs/data/jobs/continuous/jobName/ProcessBlob_20131103120400/error_20131103120400.log",
        url: "http://.../jobs/triggered/jobName/history/ProcessBlob_20131103120400"
      }
