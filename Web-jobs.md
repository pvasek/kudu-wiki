Ability to run a script (called job) in 2 different ways:

1. Trigger - run the job every time an API is called or a schedule is reached.
2. Continuous - make sure the job is always running (job should have a while loop but when it goes down we bring it back up).

Jobs are deployed by copying them to the right place in the file-system (or using a designated API which will do the same).

To deploy a triggered job copy your binaries to:
```site/wwwroot/app_data/jobs/triggered/{job name}```

To deploy a continuous job copy your binaries to:
```site/wwwroot/app_data/jobs/continuous/{job name}```

**Note:** see the Deployment section below for alternative ways of deploying WebJobs.

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
* Note: make sure .bat/.cmd files don't include the UTF-8 BOM, which can break things!


## Configuration Settings ##

* **WEBJOBS_RESTART_TIME** - Timeout in seconds between when a continuous job's process goes down (for any reason) and the time we re-launch it again (Only for continuous jobs).
* **WEBJOBS_IDLE_TIMEOUT** - Time in seconds after which we'll abort a running triggered job's process if it's in idle, has no cpu time or output (Only for triggered jobs).
* **WEBJOBS_HISTORY_SIZE** - Maximum number of runs kept in the history directory for a triggered job.
* **WEBJOBS_STOPPED** - Set this setting to **1** to disable running any job (will also stop all currently running jobs).

## Environment Settings ##

When a job is invoked, several settings are added to its environment that the job process can use:

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

## Deployment ##

There are multiple ways to deploy WebJobs.

### Using the Azure portal

From the portal, you can directly upload a zip file that contain the WebJob's files.

### Copying files directly in their proper location

You can deploy a WebJob by directly copying the files under the locations discussed above (i.e. `wwwroot/app_data/jobs/...`). This can be done either by FTP, WebDeploy, Kudu Console or git.

### Deploying .NET Console WebJobs alongside an ASP.NET application

If you have Visual Studio 2013 Update 3 (or later), it lets you do exactly this (more info [here](http://azure.microsoft.com/en-us/documentation/articles/websites-dotnet-deploy-webjobs/)). Previously, this was done using the [WebJobsVs](http://visualstudiogallery.msdn.microsoft.com/f4824551-2660-4afa-aba1-1fcc1673c3d0) VS add-in, but that is now obsolete.

High level:

- You have both an ASP.NET app and one or more WebJobs (Console apps) in your solution
- You ask the ASP.NET app to include the WebJobs apps with itself when deploying

This works both when deploying directly from Visual Studio (WebDeploy), or via git. 


## Graceful Shutdown ##

Graceful shutdown is the ability to notify the job process that it's going to shutdown and a wait period for the process to go down by itself before killing it.

The shutdown can happen on this cases: the site is getting stopped/restarted, the job is getting stopped, the app settings configuration (of the site) is changed, ```web.config``` is updated, the job binaries are updated.

The graceful shutdown feature works slightly different for continuous and triggered jobs:

### Continuous ###

When a shutdown request is detected a file will be created in the path: ```%WEBJOBS_SHUTDOWN_FILE%``` (environment variable which can be obtained by the job's process).

Then there is a default period of **5** seconds waiting for the job process to shutdown before getting killed.

### Triggered ###

When a shutdown request is detected there is a **30** seconds default waiting period for the job process to stop.

> You can change the grace period of a job by specifying it (in seconds) in the settings.job file (should be in the same root directory as the job's script) where the name of the setting is **stopping\_wait_time** like so:

> ```{ "stopping_wait_time": 60 }```

> this sample will have the job grace period at 60 seconds instead of the default.


## WebJob Working Directory ##

There are 2 options for where the WebJob is running from (what is the working directory):

1. The WebJob is copied to a temporary directory under `%TEMP%\jobs\{job type}\{job name}\{random name}` and will run from there
  This option prevents the original WebJob binaries from being locked which might cause issues redeploying the WebJob.
  For example updating an .exe file that is currently running.
2. The WebJob is run directly from the WebJob binaries directory. We call this option **in place**.
  This option can have the locking issue and should only be used when there is no risk of locking the file.
  For example if the WebJob only consists of a `.cmd` file.

By default the first option is used (`in place = false`) other than for `.js` node.js scripts (`in place = true`).

To explicitly configured this option add a file to the root of your WebJob called `settings.job` with this content:

    { "is_in_place": true/false }

## Scheduling a triggered WebJob ##

To schedule a triggered WebJob you need to add a **schedule** property to the `settings.job` file.
The value of the **schedule** is [cron expression](https://code.google.com/p/ncrontab/wiki/CrontabExpression) that has **6** fields to represent the schedule: `{second} {minute} {hour} {day} {month} {day of the week}`.

Each field can have a specific value (1), a range (1-10), a set of values (1,2,3), all values (*), an interval value (*/2 == 0,2,4,6,...) or a mix of these (1,5-10).

Whenever the `settings.job` file is updated with a new schedule, the WebJob's schedule will automatically update, it is best to deploy the `settings.job` as part of the WebJob (should be in the root of the WebJob's files).

> For the schedule to work it requires the website to be configured as **Always On** and is not an Azure Scheduler but an internal implementation of a scheduler.

#### Sample `settings.job` file ####

```
{
  "schedule": "* */1 * * * *"
}
```

#### Sample schedules ####

- `0 */5 * * * *` - run every 5 minutes (specifically on minute 0, 5, 10, 15, ...).
- `0 0 0 * * 1-5` - run at midnight on Monday, Tuesday, Wednesday, Thursday and Friday.

[More samples](https://code.google.com/p/ncrontab/wiki/CrontabExamples)

> Note samples are missing the seconds field, to make them work just add `0` as the first field to make them 6 fields total.

## API ##

[[WebJobs API]]