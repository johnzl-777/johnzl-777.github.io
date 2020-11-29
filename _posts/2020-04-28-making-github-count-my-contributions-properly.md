---
layout: post
title: Making Github Count My Contributions Properly
description: Some tips to make sure Github properly attributes contributions
tags: git github
---

I don't know about you, but I've always liked seeing those green little contribution squares on my personal Github profile slowly go up in number.

![my github contributions](/images/my-github-contributions.png)

As a result, I've tried to make it a habit to get at least one commit in a day, regardless of whether it's touching up documentation or a full-blown refactor of some old project.

Unfortunately, I found out a week ago that the commits I was pushing to a private repository weren't showing up in those green squares!

I had to get to the bottom of this. 

*For the glory of green squares!*

## The Setup

I have a private repository that I use for my [ECS 154B Class](https://cs.ucdavis.edu/schedules-classes/ecs-154b-computer-architecture) (upper division Computer Microarchitecture). The workspace I use is on a remote computer system provided by UC Davis but I have my private repository set up as a remote.

## Investigating 

When I went to take a look at the commit history, I noticed something particularly odd:

![commit history](/images/commit-history.png)

The commits I pushed had my name instead of my username (johnzl-777) and were missing my profile picture.

I had a creeping suspicion I must have misconfigured my user details in `git`. 

Up until this point I'd never set up a git repo directly on the remote computers, opting in prior classes to do all of my development locally, then perform testing on remote computers considering all the autograders prior professors used ran on the same system.

## The Fix

Stuff like usernames and emails that get added to commit information can be configured through the `git config` command. 

You have the option to change your username and email for specific repositories but I wanted to make a system-wide change for myself so I did the following:

```bash
git config --global user.name "johnzl-777"
git config --global user.email my-github-acct-email@email.com
```

Then I added a dummy file (the classic `test.txt` to the rescue), committed, and pushed. 

Going to Github, I saw the following on my private repository:

![fixed commit history](/images/fixed-commit-history.png)

__YES__, I fixed it!

More green squares for me!

## TL;DR

To change your username and email in git, and fix any issues with Github not crediting commits to your account, use the following.

```bash
git config --global user.name "your-username"
git config --global user.email your-github-acct-email@email.com
```
