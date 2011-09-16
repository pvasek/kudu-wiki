## Source Control

- /scm/kind: returns 0 (no repo), 1 (git) or 2 (Mercurial)
- /scm/id: returns the id (aka the hash) of the current change set
- /scm/branches: returns the list of branches. e.g.
  - [{"Id":"27892443eb9c35e582252bf5e7a8d8be8e027abe","Name":"SomeBranch","Active":true},{"Id":"adea8b70ac4fce788ffd6266e7bb78d1b50f9116","Name":"master","Active":false}]
- /scm/status: returns the working tree status. e.g.
 - [{"Path":"foo.txt","Status":3},{"Path":"Tools/qqq.txt","Status":4}]
 - Status codes: 0: None, 1: Added, 2: Deleted, 3: Modified, 4: Untracked, 5: Renamed
- /scm/log
- /scm/details/{id}
- /scm/working

- /scm/add
