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

### Branches
 * As an abstraction, we usually think of branches as a separate timelines that
 contain separate versions of the code base
 * It is often more useful to ditch the abstraction and think about how branches
 are implemented while working with them
    * This is part of what operations on branches very cheap and fast
 * **Key insight**: a named branch is nothing more than an alias or reference to
 a single commit

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

<img src="todo.png"/> TODO: Add illustration

### Commits


### TODO
 * HTTPS vs SSH
