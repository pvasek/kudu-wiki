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
