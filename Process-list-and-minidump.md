## Overview

To diagnose the issues, one may need to get the list of running processes, their cpu and memory usages or, even, take a minidump.  Kudu service exposes processes endpoint (`/diagnostics/processes`) enabling users to achieve exactly that.

## Using /diagnostics/processes

`<kudu-service-url>/diagnostics/processes` endpoint returns the list of running processes in json format.   Each process info contains process name, id and a link `/diagnostics/processes/{id}` to its detail information (such as memory, cpu usages, etc.).  Note: the id `0` is reserved for current `w3wp.exe` process allowing a shortcut to this via `/diagnostics/processes/0`.  One can take the minidump of any given process by simply connect to `/diagnostics/processes/{id}/dump`.      

## Summary

* `HTTP GET /diagnostics/processes` => list of processes.
* `HTTP GET /diagnostics/processes/{id}` => detail information of process with `id`.
* `HTTP DELETE /diagnostics/processes/{id}` => terminate process with `id` and its children.
* `HTTP GET /diagnostics/processes/0` => detail process information of current `w3wp.exe`.
* `HTTP DELETE /diagnostics/processes/0` => terminate `w3wp.exe` process and its children.
* `HTTP GET /diagnostics/processes/{id}/dump` => minidump of process with `id`.  By default, this is minimum dump without memory (dumptType=0).
* `HTTP GET /diagnostics/processes/{id}/dump?dumpType=2` => full memory minidump of process with `id`.  The available dumpType  value can be found at [MINIDUMP_TYPE](http://msdn.microsoft.com/en-us/library/windows/desktop/ms680519.aspx). 





