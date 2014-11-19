A **post deployment action hook** is a script/executable that runs after the deployment has completed successfully as part of the default deployment script.

- To add a **post deployment action hook** you simply need to place a script file (.bat or .cmd) under ```site\deployments\tools\PostDeploymentActions```.

- The location of these scripts is set as a default setting which can be overwritten by updating the setting: `SCM_POST_DEPLOYMENT_ACTIONS_PATH`

- You can add more than one script files, we'll run all scripts one by one.

- A script which returns an error exit code (<>0) will fail the deployment and the next script will not be called.

- The script will have the same environment variables as the deployment script and *possibly* new environment variables set by the deployment script.
