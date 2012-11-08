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
    