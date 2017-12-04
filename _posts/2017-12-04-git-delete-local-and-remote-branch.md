---
layout: post
title: Git - deleting local and remote branches, finding merged branches
date: 2017-12-04 23:00
excerpt: "git, delete, local, remote, branche"
tags: [git, delete, merged, local, remote, branch]
comments: true
---

# Delete local and remote branch

```
# delete local branch
git branch -D branchName
# delete remote branch
git push --delete origin remoteBranchName
```

# Finding merged branches for deletion

```
# find merged branches into master
git branch --merged master
# when you're on master you can omit 'master' at the end
git branch --merged
# count results/line output
git branch --merged | wc -l
# find remote
git branch -r --merged master
```
