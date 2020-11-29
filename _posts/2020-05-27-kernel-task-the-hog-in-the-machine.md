---
layout: post
title: "kernel_task: the Hog in the Machine"
description: Dealing with Mac OS's kernel_task and sluggish MacBook Pro performance
tags: mac-os
---

Just a couple days ago, I started bumping into an issue I had never had with my 2017 MacBook Pro.

Out of the blue it would start becoming *incredibly* sluggish to the point where it would take a good second just to switch between tabs on Google Chrome or type anything into VSCode.

The Activity Monitor pointed to a culprit: a mysterious task called `kernel_task` that was adding another 60-70% load onto my CPU on top of my meager 5-10%. The name tipped me off to the fact that it wasn't something I was running from some background application, and further digging online said it was a "collection of processes that are part of the Mac OS Kernel", according to the Apple support site.

Furthermore, it seemed to also have the responsibility of throttling the CPU whenever it detects an increase in temperature.

I thought this rather odd because I'd never had the problem before. Furthermore, I wasn't running anything that seemed to incur some predicted increase in CPU temperature. I'd run Slack, Chrome, VSCode, and the Chisel compiler before without having such sluggish performance so there had to be some kind of new, confounding variable.

## The Culprit

From the numerous Apple hardware forums online, it seems this isn't something I'm just experiencing and there are a range of possible causes from peripherals to kexts (Kernel Extensions).

Some suggested fixes are:

* Uninstalling kexts or the applications which introduce the kexts entirely
* Unplugging peripherals
* Switching the Thunderbolt port used for charging
* Resetting PRAM

And a couple of others I can't remember off the top of my head.

I gave resetting PRAM a go as I always had great luck with it when I had issues with my older 2012 MacBook Pro.

Unfortunately, that didn't seem to do anything so I went the route of unplugging peripherals.

Sure enough, when I unplugged the Thunderbolt to DVI adapter for my second monitor, `kernel_task` seemed to immediately die off and the sluggishness disappear!

The odd thing is I've had the adapter for years and it's worked without a hitch. If the peripheral wasn't the one causing issues, there was only one other variable left and it was one I had suspected in the beginning: the ambient temperature.

Here in Sacramento, it's very easy for summer temperatures to hit above 100 degrees Fahrenheit. Furthermore, the AC in my house is busted making the perfect conditions for `kernel_task` to "save the day".

As a precaution, I've also switched the Thunderbolt port I use for charging as it appears that one side of the MacBook Pro's Thunderbolt ports is apparently [more sensitive to temperature changes](https://www.zdnet.com/article/can-a-simple-charging-mistake-cause-your-macbook-pro-to-overheat/)

## Addendum

*Note: This section was added 6/5/2020 after still encountering the kernel_task issue despite following all the steps listed here*

It looked like the issue went away for a bit but came back with a vengeance when I needed to use my computer most (study sessions over Google Hangouts with OneNote as my trusty digital chalkboard).

The secondary fix I found was take one of those USB powered cooling pads and place it underneath the machine, with an additional miniature stand so the entire laptop sits above the plastic stand I had it on at a ~30 degree incline.

It might be that the machine wasn't getting proper ventilation sitting on a flat surface although I'm still greatly annoyed such an expensive piece of hardware has these kinds of problems.

## Conclusion

I'm disappointed such a sturdy machine is starting to show this kind of behavior but happy that I found a fix for it.

I might consider getting one of those fan-powered cooling pads you can put underneath laptops to cool them down as well as cleaning the fans out one day.

I just hope the issue doesn't kick in when I take my remote exams.

*Fingers crossed!*

__tl;dr__

* `kernel_task` will throttle your CPU if it detects an increase in CPU temperature
  * Unplugging peripherals and moving them to other ports could potentially help
  * If you're using a Thunderbolt port on the left-hand side to charge, consider switching to the right-hand side