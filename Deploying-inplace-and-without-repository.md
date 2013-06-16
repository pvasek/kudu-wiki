## Overview

There are a couple of settings that can be used to optimize the deployment.  Below describes what those are and how one or more can be used to fit your scenario.

## Settings

### SCM_REPOSITORY_PATH
This represents the repository path.  The value is a relative path of site directory that will be the root of repository (see [[File structure on azure]]; default is repository).
  
### SCM_NO_REPOSITORY
This indicates whether repository should be created with source control (Git/Hg).  By default, the source control is always enabled.  With an overhead of additional source control files, the benefit is one can rollback and deploy from a certain changeset id in the history.  If set to `1`, no source control is enabled and deployment history will only be read-only (not rollback-able).  

### PROJECT
This indicates which sub folder in the repository folder to be deployed.  This is often used where there are multiple projects in the repository, this knob will dictate which project/folder to be deployed.  

### SCM_TARGET_PATH
This indicates which sub folder in the wwwroot to deploy to.  This is often used where only part of your site (wwwroot) is under source control. 

## Scenarios
### Inplace deployment
### Inplace deployment with readonly history
### Deploy from a subfolder of a repository
### Deploy to a subfolder of a wwwroot
### Deploy from a subfolder of a repository to a subfolder of a wwwroot
### Initial Clone


By simply connecting to <kudu-service-url>/logstream, you will be able to get the live streaming of traces.  The /logstream watches (FileSystemWatcher) all the log/txt files changes under /LogFiles and its sub folders.   

To scope the live traces to certain providers/folders, you may additional specify the path.

* `/logstream/git/trace` => kudu related traces intended for diagnostic kudu issues.
* `/logstream/git/deployment` => kudu deployment traces intended for application developers/operators.
* `/logstream/application` => application traces.
* `/logstream/http` => iis logs.

etc.

## trace_level knobs

Most trace providers allow users to adjust the verbosity of the traces.  Kudu trace provider uses Diagnostic's TraceLevel; Off=0 (default), Error=1, Warning=2, Info=3, Verbose=4.  

Its trace level can be adjusted via settings.

* set kudu trace_level to Verbose.

  `$ curl <kudu-service-url>/settings -X POST -H "Content-Type: Application/json" -d "{'trace_level':'4'}"` 

* get kudu trace_level.

  `$ curl <kudu-service-url>/settings/trace_level` 
 
TBD: setting and getting application trace_level.

## filtering and last N

TBD.

## Lifetime /logstream

As long as there exists active traces, the client request will be keep alive.  After 10 mins of idle without traces, the client connection will be terminated to optimize server resources.  