## tl;dr

When reporting a WebJobs issue, there are a few key things that need to be specified:

1. Is your WebJobs continuous or triggered (triggered includes both manual and scheduled)
1. How are you deploying it? e.g. zip upload, VS deployment, git deployment, FTP, ...
1. What is it written in? e.g. .NET, Node, PHP, batch file, etc...
1. If .NET, are you using the WebJobs SDK?

More details below...

## What is your WebJob type?

Continuous WebJobs work significantly differently from triggered WebJobs. As such, it's very important to always specify which type you are using anytime you ask a WebJobs related question.


## How are you deploying your WebJobs?

The runtime discovers WebJobs by folder convention, as detailed in [[Web-jobs]]. But there are lots of ways that you can get the files there. You can:

- upload a zip through the Azure portal
- drag and drop the files using the Kudu console
- copy the files via FTP
- deploy them from Visual Studio
- deploy them using Continuous deployment

Make sure you mention what you are using when you have a question.


## What is your WebJob written in?

WebJobs can be written in any of the languages supported by Azure Web Apps, e.g.

- an exe file, which could be managed or native
- a batch file or PowerShell file
- a PHP, Python or Node script

Make sure you specify what you are using.


## Are you using the WebJobs SDK

The WebJobs SDK is a separate piece which is technically not part of the core WebJobs feature, but makes it easier to write certain types of WebJobs.

When reporting an issue or asking a question, please make sure you specify whether the issue is about WebJobs in general, or specific to the SDK.  