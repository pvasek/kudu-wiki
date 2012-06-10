When deploying a git repository via kudu, the rules for picking a specific project are as follows:

1. If there's a .deployment file at the root of the repository go to step 4, otherwise go to the next step.
2. Scan for solution files, if there's multiple solutions, fail, if there's 1 solution file go to next step, if none, go to step 6.
3. Now that we have a solution, pick the first website or WAP in that solution and deploy it, if there's none, then fail.
4. Find the target project file, if there's multiple projects, fail, if there's 1 project file, deploy it, otherwise go to next step.
5. look for a website project in the specified folder (by finding a solution that has a website with the same path), if more than one, fail, otherwise deploy the website project.
6. Find the target WAP, if there's none then Xcopy deploy all the repository files (excluding .git and .hg) to the web root.

# .deployment file
Deployment configuration files let you override the default heuristics of deployment by allowing you to specify a project to be deployed. It has to be at the root of the repository and it's in .ini format:

    [config]
    project = WebProject/WebProject.csproj

The above configuration specifies that we want to deploy WebProject.csproj in the WebProject folder. You can also specify just a path (if there's only one project file in that path).

    [config]
    project = WebProject

In case you have an solution/project file in your repository but you want it to be ignored and have your full repo deployed as a web site, you can use the following:

    [config]
    project = .