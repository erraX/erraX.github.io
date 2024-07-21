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

## Workflow
### 1. Checkout a branch from master to start implementation
```bash
$ git checkout -b feature master
```

### 2. Once you finished, create a patch that helps you partialy review the MR
```bash
# Target branch : master
# Feature branch: feature

$ git checkout -b feature-mr-1
$ git reset master

$ git add xxx
$ git commit

# Create MR-1
```

## Commands
### Difference between two branches
```bash
$ git diff HEAD..target-branch -- path/to/folder
```

## References
- [protip-how-to-split-large-branches-into-small-pull-requests](https://medium.com/@groksrc/protip-how-to-split-large-branches-into-small-pull-requests-81d607660c05)