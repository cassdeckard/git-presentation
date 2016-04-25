Git
===
An introduction

Presented by Matt Deckard

Sponsored by WWT Asynchrony Labs

The Basics
----------

### Distributed Version Control System (DVCS)

 * Server can be anywhere (or nowhere)
 * Commits are made locally
 * Push and Pull to get in sync with Server
 * You always (usually) have a local copy of the whole repo

### Pros and Cons

##### Pros
 * Distributed
   * Super easy to use locally, worry about pushing to a remote later (or never)
   * Add multiple remotes
 * Widely used
   * GitHub, GitLab
 * Very powerful and fast

##### Cons
 * Thin abstraction layer
 * Must grok at low-level to utilize full power
 * Inconsistent, confusing command-line UI
   * http://stevelosh.com/blog/2013/04/git-koans/

### Workflow basics
 * Initialize a local repo
```
$ git init
```
 * Add a remote
```
$ git remote add https://github.com/mattdeckard/git-presentation.git
$ git remote add git@github.com:mattdeckard/git-presentation.git
```
 * Fetch changes from remote
```
$ git fetch
```
 * Pull changes from remote into checked out local branch
```
$ git pull
```
 * Add local changes to index
```
$ git add ./outline.md
$ git add .
```
 * Commit indexed changes to local branch
```
$ git commit -m"Add workflow basics section"
$ git commit -v
```
 * Push local commits to remote branch
```
$ git push -u origin HEAD   ## New remote branch
$ git push                  ## Existing remote branch
```
 * View state of working repo
```
$ git status
```
 * View history
```
$ gitk
$ git log --graph --oneline --decorate
```

### Remotes
 * If you are used to centralized VCS systems like CVS or Subversion, it is
 tempting to think of a remote as equivalent to the remote server under such
 systems.
 * However, there are some key differences:
    * With DVCS like Git, `commit` is a purely _local_ operation
    * Rather than a centralized server, a remote repo is simply a different
    copy of your local repo which you sync with commands like
    `fetch` and `push`
    * In fact, all operations on commits and branches happen locally, then
    are synced with one or more remotes later.
 * Most of this presentation is going to cover local operations

### Commits
 * As an abstraction, a commit represents a distinct state of all of the files
 in your codebase that are tracked by Git
 * Git does all kinds of magic under the hood to make commit operations cheap
 and fast.
    * It is _not_ literally saving a new snapshot of your codebase in each
    commit
    * As another abstraction, it can be useful to think of each commit as a
    _delta_ or _patch_ of the previous codebase state. Even though this also
    isn't quite what Git is really doing under the hood, much of its CLI uses
    this metaphor.

### The index
 * Git has the concept of the "index". Its CLI and documentation provides an
 array of mixed metaphors to describe this concept
    * The index / adding to the index / indexed
    * The stage / staging / staged
    * The cache / cached
 * In day-to-day usage, all you need to know is that versions of a file in your
 repo _must_ be in the index before they can be included in a commit
 * The typical way to add versions of files to your index is through the `add`
 command:
```
$ git add src/bundle.js    ## Add a single file
$ git add .                ## Add all files in working dir
```
 * One shortcut to add _and_ commit all changes to tracked files is:
```
$ git commit -a
```
    * Note this will only add changes to files that are already tracked by Git.
    It will not add completely new files.

### Branches
 * As an abstraction, we usually think of branches as a separate timelines that
 contain separate versions of the code base
 * It is often more useful to ditch the abstraction and think about how branches
 are implemented while working with them
 * **Key insight**: a named branch is nothing more than an alias or reference to
 a single commit
    * This is part of what operations on branches very cheap and fast

#### How to
 * Create a new local branch
```
$ git branch feature/github-integration
$ git checkout -b feature/github-integration
```
 * Merge `feature/foo` into `master`
```
$ git checkout master
$ git merge feature/foo
```

![TODO](todo.png) TODO: Add illustration

### Merging

### Rebasing

### Checkout versus Reset

### TODO
 * HTTPS vs SSH - maybe, but not necessarily
 * Talk about DAGs, graph theory
 * Tracking branches
 * Patches
 * In Commit section, talk about how it moves the branch head
 * Talk about head
    * With branch checked out
    * In detached head state
 * Foreign language metaphor?
 * Plumbing / porcelain metaphor
 * SHAs
 * Show where branches live in .git dir?
 * Orphans / unreachable / lost
   * reflog
 * Stash
 * Bisect
