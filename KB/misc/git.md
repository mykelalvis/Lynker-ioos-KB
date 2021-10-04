---
date updated: '2021-05-07T06:17:09-05:00'
tags:
  - '#TODO'

---

# Git

Git is a [[source code]] repository application written initially by Linus Torvalds as a replacement for a less-desirable source code repository that he was using previously.

Collections of source in git are referred to by name as a repository.  The name of such a repository can be defined a number of ways, but usually either as #TODO

Git is ostensibly a distributed source code repository system, although it is rarely used as such.  Every "clone" of a git repository is a full copy of all the history of that repository, and any clone can be used to update another clone.  In practice, most git work is done in a centralized fashion, using a service such as GitHub or BitBucket

## Branching
Git provides a mechanism for local and lightweight [branching](https://git-scm.com/book/en/v2/Git-Branching-Branches-in-a-Nutshell).  These branches, when applied to the various [[#Services]] that are available to host git, often allow for relatively fine-grained control of the source on an individual or team basis.

### Branching Strategy
Because they are lightweight, the population of branches served by git could quickly grow out of control.  A "branching strategy" is usually adopted by teams, allowing them to predict what a branch is for, when it can and cannot be merged, and when it can be deleted.

[Examples](https://www.gitkraken.com/learn/git/best-practices/git-branch-strategy) of branching strategies are available.  Picking the correct one for a given team is a decision that needs to be carefully considered.

## Merges
Git allows the [merging](https://git-scm.com/book/en/v2/Git-Branching-Basic-Branching-and-Merging) of code with a common commit ancestry.   Maintaining continuity with history within git is not _required_, but is _highly recommended_.

Merges are the application of changes from one branch onto another.  There are numerous variations of this.  Relevant details are available in the above link.

## Services

There are numerous hosting services for git and other [[source code]] management systems such as Mercurial, Subversion, and Bazaar.

These services often provide additional adjunct services beyond just hosting the centralized repository, such as [[continuous integration|CI]]/[[continuous delivery|CD]] and [[release]] management capabilities.  Most of these additional services are designed to provide greater lock-in on the hosted code by facilitating the needs of the consumers.

### GitHub

[GitHub](https://github.com) is a service that hosts Git repositories.  It was purchased by [Microsoft](https://microsoft.com) in 2018.  As of 2020, it had approximately 56 million users, making the largest such software hosting service in history.  It shows no signs of losing that title.

### BitBucket

[BitBucket](https://bitbucket.org) is a service similar to GitHub but owned and hosted by [Atlassian](https://atlassian.com)
