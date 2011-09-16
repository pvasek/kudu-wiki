## Source Control

GET APIs:

- /scm/kind: returns 0 (no repo), 1 (git) or 2 (Mercurial)
- /scm/id: returns the id (aka the hash) of the current change set
- /scm/branches: returns the list of branches. e.g.
  - [{"Id":"27892443eb9c35e582252bf5e7a8d8be8e027abe","Name":"SomeBranch","Active":true},{"Id":"adea8b70ac4fce788ffd6266e7bb78d1b50f9116","Name":"master","Active":false}]
- /scm/status: returns the working tree status. e.g.
  - [{"Path":"foo.txt","Status":3},{"Path":"Tools/qqq.txt","Status":4}]
  - Status codes: 0: None, 1: Added, 2: Deleted, 3: Modified, 4: Untracked, 5: Renamed
- /scm/log: returns the change log
  - [{"Id":"27892443eb9c35e582252bf5e7a8d8be8e027abe","AuthorName":"davidebbo","AuthorEmail":"david.ebbo@microsoft.com","Message":"    Blah\n","Timestamp":"\/Date(1316156828000)\/"},{"Id":"adea8b70ac4fce788ffd6266e7bb78d1b50f9116","AuthorName":"davidebbo","AuthorEmail":"david.ebbo@microsoft.com","Message":"    Foo\n","Timestamp":"\/Date(1315433098000)\/"}]
- /scm/details/{id}: returns the details of a change set
- /scm/working:  returns the details of the working change set


POST APIs:

- /scm/add: create a new file. Takes in a *path*
- /scm/remove: delete a file. Takes in a *path*
- /scm/Create: create the repository. Takes in a repo *type* (see /scm/kind)
- /scm/Delete: delete the repository
- /scm/Commit: commits the current changes. Takes an author *name* and a *message*

## Deployment

GET APIs:

- /deploy/id: returns the id of the current deployment
- /deploy/log/{id}
- /deploy/details/{id}


POST APIs:

- /deploy/index/{id}
- /deploy/new/{id}

## Files

GET APIs:

- /files?path={path}


POST APIs:

- /files/Save?path={path}: passes *content* in body
- /files/Delete?path={path}

## Connection strings

GET APIs:

- /connectionstrings
- /connectionstrings/Get?key={key} should really be /connectionstrings/Get/{key}


POST APIs:

- /connectionstrings/Set?key=Somekey&value=Somevalue
- /connectionstrings/Remove?key=Somekey


## App Setttings

Identical to Connection strings but with /connectionstrings changed to /appsettings
