---
title: Tips for splitting small MR
date: 2024-07-16 14:34:08
tags: 
    - git
---

## Introduction
During code reivew, you may be asked to split a large MR into smaller ones. It would be helpful for reviewers to reduce their cognitive load and focus on spcific part.
However, sometimes it's very challenging for developers to split a large MR to smaller ones.

## What is a large MR? And why reviewing a large MR is very hard?
1. Reviewer need to take a lot log time to go thorugh all the changed files
2. Reviewer may lost in huge amount of code changes, and it's hard to understand the code changes.

## Workflow
### 1. Checkout a branch from master to start implementation
```shell
$ git checkout -b feature master
```

### 2. Once you finished development, create a patch that helps you partialy review the MR
```shell
$ git checkout -b feature-mr-1
$ git diff master feature > path/to/path_file
```

### 3. Apply patch and start review
```shell
$ git fetch
$ git checkout -b feature-mr-1 origin/master
$ git apply path/to/patch_file

# Staged changes you want to put in the MR
$ git add some_files.js
$ git commit


# Save changes
$ git stash -uk
```

### 4. Create other parallel MRs
```shell
$ git fetch
$ git checkout -b feature-mr-2 origin/master
$ git stash apply

# Staged changes you want to put in the MR
$ git add some_files.js
$ git commit
```

### 5. Once your MR is merged, rebase your feature branch with master
```shell
$ git fetch
$ git checkout feature
$ git rebase origin/master
```

### 7. Revision MR
#### Compare you current branch with your patch file
```shell
$ git checkout -b tmp master
$ git apply path/to/patch_file
$ git commit -am "tmp"
$ git diff mr tmp
```

## References
- [protip-how-to-split-large-branches-into-small-pull-requests](https://medium.com/@groksrc/protip-how-to-split-large-branches-into-small-pull-requests-81d607660c05)