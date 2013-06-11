An action hook is a script that runs as part of the deployment on a specific stage of the deployment and can affect the result of that deployment.

- Initially we are going to support **post deployment** action hooks which are called after the deployment script steps are done as actually the last step of that deployment (before calling the **PostDeployment** web hooks).

- To add a **post deployment** action hook you simply need to place script file (.bat or .cmd) under ```site\deployments\scripts\post_deployment``` and as the last step of the deployment we will call all scripts under this directory.

- The location of these scripts is set as a default setting which can be overwritten (SCM_HOOKS_POST_DEPLOYMENT).

- The scripts will be run one by one in alphabetical order.

- If a script returns an error exit code (<>0) then the deployment fails and the next script will not be called.

- The script will have the same environment variables as the deployment script and *possibly* new environment variables set by the deployment script.

- The deployment script will be the one which calls the *post deployment* action hook.

- P1 feature is to add an api for uploading/removing an action hook (as first step it will be done using VFS).

## Design Questions ##

- Do we want the action hook to be called outside of the deployment script (by the kudu services)?

    **Pros**

    - This feature will work for users that **currently** have custom deployment script (otherwise they'll need to update it).

    **Cons**

    - Goes against the "deployment script owns all deployments steps" concept we have so far.

    - Harder to keep the environment state that the deployment script has.
