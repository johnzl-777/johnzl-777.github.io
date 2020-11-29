---
layout: post
title: Github "Mass" Merge Conflict Resolution
description: Two neat commands to handle special large scale merges
tags: git
---

You may come across a time when you need to perform a merge and you're certain you just want to accept either

1. all the incoming changes or
2. all the changes you have locally

You can use the following (as found from [this blog](http://inlehmansterms.net/2014/12/14/resolving-conflicts-in-git-with-ours-and-theirs/))

```bash
# a merge that wants to resolve all conflicts
# by taking the current branch's changes
git merge -s ours merge_branch
# a merge that wants to resolve all conflicts
# with the merge_branch changes
git merge -s theirs merge_branch
```
__BEWARE!!!__ I strongly recommend only using these commands when you are __ABSOLUTELY CERTAIN__ that you want all of one or the other. For cases where you are uncertain, just open you favorite text editor if a conflict comes up after doing a normal `git merge` and resolve it the old fashioned way. 

