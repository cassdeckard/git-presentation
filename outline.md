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
 * Widely used
   * GitHub, GitLab
 * Very powerful and fast

##### Cons
 * Thin abstraction layer
 * Must grok at low-level to utilize full power
 * Inconsistent, confusing command-line UI
   * http://stevelosh.com/blog/2013/04/git-koans/


### Branches
 * As an abstraction, we usually think of branches as a separate timelines that contain separate versions of the code base
 * With Git it is often more useful to ditch the abstraction and think about how branches are implemented while working with them
 * **Key insight**: a named branch is nothing more than an alias or reference to a single commit

### Commits
