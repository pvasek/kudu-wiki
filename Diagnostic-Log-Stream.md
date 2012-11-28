## Overview

One of the keys to diagnose the issues is to look at the trace files.  Kudu service as well as its application write traces to /Logfiles folder (see [[File structure on Azure]]).  Logstream endpoint (/logstream) enables developers to view live traces as they are being written allowing the real-time diagnostics.

## Using /logstream

By simply connecting to <kudu-service-url>/logstream, you will be able to get the live streaming of traces.  The /logstream watches all the files changes under /LogFiles and its sub folders.   To scope the live traces to certain providers/folders, you may additional specify the path.

* /logstream/git/trace => kudu related traces intended for diagnostic kudu issues.
* /logstream/git/deployment => kudu deployment traces intended for application developers/operators.
* /logstream/application => application traces.
* /logstream/http => iis logs.

etc.

## trace_level knobs

Most trace providers allow users to adjust the verbosity of the traces.  Kudu trace provider uses Diagnostic's TraceLevel; Off=0 (default), Error=1, Warning=2, Info=3, Verbose=4.  Its trace level can be adjusted via settings.

* set kudu trace_level to Verbose.

  `$ curl <kudu-service-url>/settings -X POST -H "Content-Type: Application/json" -d "{'trace_level':'4'}"` 

* get kudu trace_level.

  `$ curl <kudu-service-url>/settings/trace_level` 
 
TBD: setting and getting application trace_level.

## filtering and last N

TBD.

## Lifetime /logstream

As long as there exists active traces, the client request will be keep alive.  After 10 mins of idle without traces, the client connection will be terminated to optimize server resources.  
