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

## Limitations

### It cannot get dumps for crashed processes

This feature is about getting dumps for running processes, and not for crashed processes.

### w3wp needs to be healthy enough to process the dump request

Since the Kudu service runs in the same w3wp process as the site, w3wp needs to be able to process the dump request. In most cases, it will be, but it's possible for w3wp to be corrupted enough not to be able to handle the request.

### In multi-instance scenarios, the instance cannot be selected

If you have an Azure site that's running on multiple instances, the dump request will end up getting sent to an arbitrary instance.

Ideally, the specific instance could be selected, and hopefully it will work that way in the future.


### Full dump could cause download quota to be reached

The resources used by the dump feature are counted toward the site limits. For the default small dump, it's not an issue, but if requesting a full dump for a Free Azure site, you could reach the quota which could cause some site downtime.
