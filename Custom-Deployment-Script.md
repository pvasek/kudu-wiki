## Requirements

- Users are able to customize their deployment of their website.
- Customization is of the actions taken *after* repository is updated with their latest update (for example: the user starts a “git push” command, …).
- Users will be able to use the same deployment features as the original deployment script.
- Users will be able to test their deployment script on their own machine (Windows and Mac).


## Non Goals

- The script will not cancel an update to the website repository (only by returning a failed status from the script).


## Scenarios

- User wants to add a flag to msbuild command line.
- User wants to change the msbuild command line entirely.
- User wants to add tests before deploying with a .NET project.
- User wants to add tests before deploying with a node.js project.
- User with node.js project needs more control on how npm is run.
- User wants to fully customize (have full control on) the deployment process.


## Design

- In order for the custom script to work on Windows Azure, Windows and Mac while avoiding code duplication between different platforms, We will use node.js to run the custom deployment script.
- Our original deployment script should also be rewritten (C# --> JavaScript) to run on node.js (including all of our important deployment logic like: project build and smart copy), This will allow our users to only update the original script for a quick start.
- The original deployment script will be available as an npm package.
- .NET devs should have to start learning how to use node.js and JavaScript in order to write (or update) a custom deployment script.
- We should encapsulate our node.js code in a way that we can still have a batch file for non node.js/JavaScript being able to easily write custom deployment (the batch file can have calls like: "node smartCopy.js [args]").


## Implementation

- We would need to extract the code that currently deploys a website from the C# project to a JavaScript node.js project (considering on trying out TypeScript if that makes sense).
- The line between code in JavaScript and C# should be whatever is used to deploy a website (any kind), including: copy/delete/move of files/directories, smart copy and project compilation/build.
- Most of the code is within the kudu.exe project, but there are cases where deployment related code is being called bypassing kudu.exe, will need to look into these cases closely to choose carefully between duplicating the code in 2 places (C# and JavaScript) or calling from the C# project to node.js (seems possible at first sight).

## Misc
- We should consider changing the concept here to a self contained "Smart Publish" script that has no dependencies on kudu.
