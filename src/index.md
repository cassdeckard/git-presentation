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
 * Hard to mess things up in a totally unrecoverable way

##### Cons
 * Thin abstraction layer
 * Must grok at low-level to utilize full power
 * Inconsistent, confusing command-line UI
   * http://stevelosh.com/blog/2013/04/git-koans/
 * Hard to figure out how to recover from mistakes if you are not a Git guru
 * Intimidating

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
 * As an abstraction, a commit represents a distinct snapshot of the complete
 state of your files that are tracked by Git
 * Git does all kinds of magic under the hood to make commit operations cheap
 and fast.
    * It is _not_ literally saving a new copy of all of your files in each
    commit
    * As another abstraction, it can be useful to think of each commit as a
    _delta_ or _patch_ of the previous codebase state. Even though this also
    isn't quite what Git is really doing under the hood, much of its CLI uses
    this metaphor.
 * A commit can be referred to in multiple ways:
    * Each commit has a unique SHA-1 hash identifier represented as 40-digit
    hexadecimal number. You can use a prefix of this SHA to refer to a commit.
    * A branch is just a reference to a commit, so you can use a branch name
    as a commit. In certain contexts (like checkout) a Git command will treat
    a local branch differently than a commit but for many they will be treated
    the same.
    * A tag, which likewise is just a reference to a commit and outside of the
    `tag` command is always treated as such
    * There are various "spec" notations that allow you to get commits
    relative to a reference commit or branch (tilde, carrot, `@{}`)
    * Collectively, each of these methods of referring to commits is a 
    "treeish"

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
 * If you have a branch checked out, then the pointer is advanced to each new
 commit you make
 * Committing and branching in Git are so cheap and fast you should consider
 them as free and use them very liberally to help give yourself and others
 a clearer view of your code history.

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
 * Set up `foo` to track `origin/foo`
```
$ git checkout foo
$ git branch -u origin/foo
```

### HEAD
 * There is a special branch called `HEAD` that is Git's way of tracking:
    * Which commit your working changes and index are in reference to
    * What branch, if any, you currently have checked out
 * It is possible to not have any branch checked out. This is refered to as
 being in a "detached head" state. When in this state, any new commits will
 be "orphaned", i.e. not pointed to by any branch unless you create a new one.

### Merging
 * Merging is the process of integrating the changes made in one branch into
 another
 * Git is usually pretty good at integrating the textual changes automatically
 * If both branches made changes in the same section of a file, Git will ask
 you to resolve the conflict for it.
 * If the target branch is "reachable" by the branch being merged in, Git will
 by default do a "fast-forward" merge meaning it will simply reset the branch
 head
    * This can be prevented with the `no-ff` option

### Rebasing
 * Rebasing is the process of moving a chain of commits (i.e. a branch) to
 be based off a different part of the commit tree
 * A workflow that involves rebasing feature branches off of the latest main
 branch before merging it back in can result in an easier to understand commit
 tree.
 * Rebasing can also be used as a tool to tackle merge conflicts one commit
 at a time which can be easier than trying to resolve them all at once with
 a merge.
 * If you are familiar with the `cherry-pick` tool, rebasing is essentially
 like cherry-picking a series of commits in a branch and then resetting the
 branch head to the new cherry-picked location.
 * Rebasing locally is just fine. Rebasing and force pushing branches others
 may be working with should not be done lightly.

### Checkout versus Reset
You've surely used `checkout` to switch between branches and undo uncommitted
changes to files.
Perhaps you have used `reset` to unstage changes from the index.

But what do these commands _actually do_?

#### Checkout
 * `git checkout BRANCH` moves the HEAD to a different branch (or a commit,
 leaving you in a detached head state)
    * This will only change files that don't have in-work changes. It will
    abort if there are any merge conflicts (even ones it could trivially
    resolve in a merge)
    * `checkout --merge` or `-m` will make it try to do a merge (so no
    need to stash / stash pop when doing a checkout!)
 * `git checkout FILEPATH` brings local files in sync with HEAD
    * Safest to do `checkout -- FILEPATH` to avoid ambiguity
    * This is one of the few operations in Git which is unrecoverable so use
    it carefully!
 * `git checkout BRANCH -- FILEPATH` checks out the BRANCH versions of the
 given filepath _without checking out the branch itself_

#### Reset
 * `git reset COMMIT` moves the current branch's reference itself to the target
 COMMIT
   * What it does with working changes and the index depends on the type of
   reset
    * `--soft` does not modify any files on disk, or the index (may find
    "extra" stuff in your index)
    * `--mixed` (default) does NOT modify any files on disk, but completely
    resets the index (do a `status` and there will be no green)
    * `--hard` changes all files on disk to match destination and resets the
    index (do a `status` and there will be nothing except possibly untracked
    files)
 * All forms of `reset` specifying a commit other than HEAD have the potential
 of leaving unreachable commits
 * A hard reset will blow away any uncommitted changes. This is one of the few
 operations in Git that is unrecoverable so use it carefully!

### Patches
 * Patch versions of commands (add, checkout, reset, stash)

### Orphans
 * Garbage collection
 * The reflog

### Stash

### Finding things in history
 * `git blame`
 * `git log -S`, a.k.a. the pickaxe
 * `git bisect`
