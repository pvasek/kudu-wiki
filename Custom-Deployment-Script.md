## Requirements

- Users are able to customize their deployment of their website.
- Customization is of the actions taken *after* repository is updated with their latest update (for example: the user starts a “git push” command, …).
- Users will be able to use the same deployment features as the original deployment script.
- Users will be able to test their deployment script on their own machine (Windows and Mac).


## Non Goals

- The script will not cancel an update to the website repository (only by returning a failed status from the script).


## Scenarios

[[Deployment Logic Flow]]

### Precondition

In order to use the custom deployment script feature, the user will have to install node.js and download the Azure SDK package (will include: smart-copy, script builder wizard and whatever is required for it).

### User wants to add a flag to msbuild command line.

**Suggested Behavior**

- User runs the "Custom Deployment Script Wizard" from the Azure SDK node.js package, selects the project type and where the project (and/or solution) file is located at.
- The Wizard will generate a custom deployment script file.
- The user will update the msbuild command with the required changes (add a flag).
- The user will place this file in the root of his web project and will commit this file to git repository.

**Previous Suggested Behavior (for comparison) - Options:**

1. User updates the custom deployment configuration file (deployment.config) with the new flags and they are appended as a postfix to the msbuild commandline.
2. The configuration file already has an entry with all msbuild flags and the user simply adds his new flags to all the current ones in the configuration.

`MsBuildProjectCommand = "MSBuild MyWebApp\MyWebApp.csproj /t:pipelinePreDeployCopyAllFilesToOneFolder /p:_PackageTempDir="%DEPLOYMENT_TARGET%";AutoParameterizationWebConfigConnectionStrings"`

### User wants to change the msbuild command line entirely.

**Suggested Behavior**

Same as previous one but the user updates the entire msbuild command line.

**Previous Suggested Behavior (for comparison) - Options:**

1. The configuration file already has an entry with all msbuild flags and the user simply adds his new flags to all the current ones in the configuration.
2. The user updates the custom deployment script where the msbuild line is.

### User wants to add tests before deploying with a .NET project.

**Suggested Behavior**

- User generates script with the Wizard.
- User updates script with proper command to run the tests.

**Previous Suggested Behavior (for comparison) - Options:**

1. User updates a custom deployment script hook (post build hook) with the commands that run the tests.
2. User updates the custom deployment script (a batch file) and write the "run tests" command after the build command.

### User wants to add tests before deploying with a node.js project.

**Suggested Behavior**

Same as previous, just by selecting a node.js project, the script being generated will be a node.js javascript script which the user will have to update in order to run his tests.

**Previous Suggested Behavior**

Same as the previous one only this time the script being updated is a node.js JavaScript script.

### User with node.js project needs more control on how npm is run.

**Suggested Behavior**

- User generates the custom script with the Wizard.
- User updates the download npm command.

**Previous Suggested Behavior (for comparison) - Options:**

1. User updates the custom deployment script (node.js JavaScript file) and changes the npm command.
2. User updates the configuration file with the npm command he wishes to use.

`NpmCommand = npm install -whatever`

### User wants to fully customize (have full control on) the deployment process.

**Suggested Behavior**

- User generates script.
- User updates script.

**Previous Suggested Behavior**

User updates the custom deployment script (node.js JavaScript file or batch file according to his preference) and changes the npm command.


## Design

- To provide a **simple** way for users to get a custom deployment script that is **simple**, we will provide a Wizard that receives 2 inputs (project type and project file) and from those it'll create a custom tailored script for that specific website, the script will be in batch file for .NET projects and node.js javascript for other (node.js, PHP, ...) projects, this script can then be updated and needs to be in the website repository (committed) for it to run when deploying the site to Azure.
- This script will also be able to run locally.
- In order for the custom script to work on Windows Azure, Windows and Mac while avoiding code duplication between different platforms, We will use node.js to run common logic of the custom deployment script (like smart copy) and the scripts themselves will be per project type: .NET == batch file, others == node.js javascript file.
- Another step to avoid code duplication will be to create our default deployment script as the Wizard creates it, this is by finding the project type and project file (with the same logic we have today), running the Wizard with these inputs to create a deployment script and running that deployment script.
- The Wizard itself will be written as a node.js module as part of the Azure SDK node.js package.

## Implementation

- We would need to extract (a part of) the code that currently deploys a website out to 4 separate template scripts: .NET Web Application, .NET Website, node.js and other (including PHP, ...).
- We will create a Wizard in node.js that takes input from the user (by argument or console) and populates the correct template and outputs the result to a script file.
- Our deployment service' project type and project file detection will remain the same but after that it'll call the Wizard to create the script and will run that script (which will include smart-copy and running ms build and for node.js also download npm packages).

**Legacy - These files will be joined to one and also split to 4 different scripts for simplicity**
- Prototype for how a batch script would look like - [[Kudu Deployment Script .cmd]].
- Prototype for how the deployment configuration would look like - [[Kudu Deployment Configuration Prototype]].

## Misc
- We should consider changing the concept here to a self contained "Smart Publish" script that has no dependencies on kudu.
