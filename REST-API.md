This API is accessed the same way as the git endpoint. e.g. if your git URL is `https://yoursite.scm.azurewebsites.net/yoursite.git`, then the API to get the list of deployments will be `https://yoursite.scm.azurewebsites.net/deployments`.

The credentials you use are the same as when you git push. See [[Deployment-credentials]] for more details.


## scm

Note: in older builds, use `live/scm` instead of `scm`

    GET scm/info
    Get information about the repository
    
    POST scm/clean	
    Clean the repository, using 'git clean -xdff'
    
    DELETE scm	
    Delete the repository
    	
## Command

    POST command	
    Executes an arbitrary command line and return its output
    	
## Deployment

    GET deployments	
    Get the list of all deployments
    
    GET deployments/{id}	
    Get a deployment
    
    PUT deployments/{id}	
    Deploy a previous deployment
    
    DELETE deployments/{id}	
    Delete a deployment
    
    GET deployments/{id}/log	
    Get the list of log entries for a deployment
    
    GET deployments/{id}/log/{logId}	
    Get the list of log entry details for a log entry
    	
## SSHKey

    PUT sshkey	
    Set the private key. The supported key format is privacy enhanced mail (PEM)
    	
## Environment

    GET environment	
    Get the Kudu version
    	
## Settings

    POST settings	
    Create or change a setting
    
    GET settings	
    Get the list of all settings
    
    GET settings/{key}	
    Get the value of a setting
    
    DELETE settings/{key}	
    Delete a setting
    	
## Diagnostics

    GET dump	
    Get all the diagnostic logs as a zip file

## Diagnostics/Settings

    POST diagnostics/settings	
    Create or change a setting
    
    GET diagnostics/settings	
    Get the list of all settings
    
    GET diagnostics/settings/{key}	
    Get the value of a setting
    
    DELETE diagnostics/settings/{key}	
    Delete a setting

Sample of available settings.

    {
        "AzureDriveEnabled": false(|true),
        "AzureDriveTraceLevel": "Error(|Information|Warning|Verbose)",
        "AzureTableEnabled": false(|true),
        "AzureTableTraceLevel": "Error(|Information|Warning|Verbose)",
        "AzureBlobEnabled": false(|true),
        "AzureBlobTraceLevel": "Error(|Information|Warning|Verbose)",
    }

    