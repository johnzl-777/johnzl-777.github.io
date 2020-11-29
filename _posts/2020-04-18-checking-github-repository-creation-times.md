---
layout: post
title: Finding Github Repository Creation Times
description: A neat way to check when a Github repository was created
tags: git github
---
In the middle of updating my resume, I ran into a problem of determining when I had started working on a certain personal project.

This particular project was something I had done in High School, hence my poor memory of when it began.

A little bit of digging on Stack Overflow produced this command by [legends2k](https://stackoverflow.com/users/183120/legends2k), which takes advantage of the Github Repos API, `curl`, and `grep` to get information on any repository and print the timestamp of its creation. 

```bash
curl -s https://api.github.com/repos/{put-repo-owner-here}/{put-repo-name-here} | grep 'created_at' | cut -d: -f2-
```

The timestamp comes out in IS0 8601 format, so something like:

```
2016-03-11T02:02:33Z
```

Where the `T` is just a separator between the date and the time, and the `Z` denotes UTC.

It should be pretty easy to add a little more to replace the `T` with a proper space and remove the `Z` or perform some conversion if desired (maybe pipe the string to another text manipulation command?).

Despite that, the format is still very readable and the command just makes for a nice way to check such a small piece of information without too much hassle. 