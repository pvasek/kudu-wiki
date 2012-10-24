### This script will be run after:
- The repository was updated.
- Configuration values are put as environment settings.
- Temporary path is created (and passed as an environment setting).
- Project type is determined and project file/solution file are found (also passed as environment settings).

### Post script actions are:
- Remove temporary path.
- Add iis node configuration (if needed).
- Select node version (if needed).


    ```Batchfile
    @echo off  
  
    REM ----------------------  
    REM KUDU Deployment Script
    REM ----------------------
    REM
    REM INPUTS:
    REM -------
    REM REPOSITORY_ROOT_PATH
    REM TEMPORARY_DIRECTORY_PATH
    REM WEB_ROOT_PATH
    REM MANIFEST_PATH
    REM PROJECT_TYPE
    REM PROJECT_FILE_PATH
    REM SOLUTION_FILE_PATH
      
    REM Switch on the project type
    IF /I  "%PROJECT_TYPE%"=="DOTNET_WEB_APPLICATION" goto wap
    IF /I  "%PROJECT_TYPE%"=="DOTNET_WEB_SITE" goto website
    IF /I  "%PROJECT_TYPE%"=="OTHER" goto other
    goto error
      
      
    :wap
    echo Handling .NET Web Application deployment.
    
    REM Build to the temporary path
    %MSBUILD_TO_TEMP_COMMAND%
    
    REM SmartCopy [From] [To] [Manifest]
    %SMART_COPY_COMMAND% %TEMPORARY_DIRECTORY_PATH% %WEB_ROOT_PATH% %MANIFEST_PATH%
    
    goto end
    
    
    :website
    echo Handling .NET Web Site deployment.
    
    REM Build to the repository root path
    %MSBUILD_TO_REPOSITORY_COMMAND%
    
    REM SmartCopy [From] [To] [Manifest]
    %SMART_COPY_COMMAND% %REPOSITORY_ROOT_PATH% %WEB_ROOT_PATH% %MANIFEST_PATH%
    
    goto end
    

    :other
    echo Handling web site deployment.

    REM SmartCopy [From] [To] [Manifest]
    %SMART_COPY_COMMAND% %REPOSITORY_ROOT_PATH% %WEB_ROOT_PATH% %MANIFEST_PATH%
    
    %DOWNLOAD_NPM_PACKAGES_COMMAND%
    
    goto end
    
    
    :error
    echo An error has occured during web site deployment.


    :end
