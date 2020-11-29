---
layout: post
title: "Upgrading Jekyll Themes: Is There a Better Way?"
description: A dilemma with upgrading Jekyll Themes
tags: jekyll themes
---

In the past couple of days I spent some time "upgrading" the default [sidey](https://github.com/ronv/sidey) theme this website is based off to my liking.

These included such things as:
* Making the navbar stay fixed in the window (no need to scroll all the way up to reach the navbar)
* Fixing the indentation of results on the "search" page (the number for each result lines up with the edge of the search bar versus protruding about 1-2 em)
* Making the actively viewed page more prominent in the navbar through highlighting and inverting the text color (black to white)

As these modifications started to grow in number, I figured it would be a good idea to repurpose the fork I made on GitHub of *sidey*. After all, there might be someone else who would find it nice to re-use the templates I've created for project and work experience pages. 

As a tip of the hat to its lightweight nature, I renamed the forked repo to [__H+__](https://github.com/johnzl-777/H-Plus), after the lightweight Hydrogen ion. 

To be safe, I had done all my development on the theme with a dummy site. The dummy site is very similar to this site except it's filled with boilerplate (think (*Lorem Ipsem*), code examples, MathJax snippets, etc. to make sure I didn't break anything during the development process. 

Now that I was absolutely certain the theme was to my liking, I found myself in a bit of a rut:

> How do I "upgrade" my website theme?

The method I came up with was to just replace the files manually (drag and drop from Mac OS Finder to VSCode). 

I'm aware that Jekyll also allows themes to be packaged as Gems but since I'm hosting this website on Github Pages, I'm uncertain as to how Github will react to this method.

Oh well, another problem to be noted and put on the backburner.

I'll keep my eyes peeled for any better solutions but that's the method I found that works for me.
