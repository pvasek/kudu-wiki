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

We use the following logic to decide which file is the script to run in side the job's directory:

* Per file type we look first for a file named: ```run.{file type extension}```.
* If it doesn't exist we look for any other file with that {file type extension}.
* If we still didn't find one we go to the next file type.
* The order of file types we use is: ```.cmd```, ```.bat```, ```.exe```, ```.sh```, ```.php```, ```.py```, ```.js```.
* The recommended script file to have in your job directory is: ```run.cmd```.
* Note: We'll only look for a script under the root directory of that job (not under sub directories of it).

# API #

    GET jobs
    List all web jobs

## Triggered Jobs ##

    GET jobs/triggered
    List all triggered jobs

    GET jobs/triggered/{job name}
    Get a specific triggered job by name

    PUT jobs/triggered/{job name}
    Add/Replace a triggered job using a zip file containing the files for it, or just a single file (e.g foo.exe)

    DELETE jobs/triggered/{job name}
    Delete a triggered job

    POST jobs/triggered/{job name}/invoke
    Invoke a triggered job

    POST jobs/triggered/{job name}/kill
    Kill a triggered job

    GET jobs/triggered/{job name}/history
    List all triggered job runs history

    GET jobs/triggered/{job name}/history/{id}
    Get a specific run for a specific triggered job

    GET jobs/triggered/{job name}/history/{id}/output
    Get the standard (stdout) output for a specific run

    GET jobs/triggered/{job name}/history/{id}/error
    Get the error (stderr) output for a specific run

## Continuous Jobs ##

    GET jobs/continuous
    List all continuous jobs

    GET jobs/continuous/{job name}
    Get a specific continuous job by name

    PUT jobs/continuous/{job name}
    Add/Replace a continuous job using a zip file containing the files for it, or just a single file (e.g foo.exe)

    DELETE jobs/continuous/{job name}
    Delete a continuous job

    POST jobs/continuous/{job name}/stop
    Stop a continuous job (if running)

    POST jobs/continuous/{job name}/start
    Start a continuous job (if stopped)

    GET jobs/continuous/{job name}/log
    Get the log for a specific continuous job
