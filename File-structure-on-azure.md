See [[Accessing files via FTP]] for info on getting at all the files that relate to your site.

In there, you will see the following structure:

      /
        LogFiles
          Git
            trace
              trace.xml	// Trace generated during git deployments
        site
          deployments
            [commit id 1]
              log.xml		// The deployment log, similar to what the protal shows
              status.xml	// The status of the deployment (success/failure)
              manifest	// The list of files that were deployed
            [commit id 2]
              ...
            repository			// Your repo, including working files (i.e. not bare)
              .git
            wwwroot
              hello.htm		// The files that are live in your app
        kuduservice
          Used when your site uses private kudu bits