+++
title = "Thank you, User!"
description = "A brief reflection on the joys of building and sharing what one builds"
date = 2026-06-27
draft = false

[taxonomies]
tags = ["dev"]

[extra]
lang = "en"
toc = true
comment = false
copy = true
math = false
mermaid = false
featured = false
reaction = false
+++

A couple of months ago I got an email from somebody in Australia asking me if my old [SRAM (Static Random Access Memory)-Read-Write](https://github.com/johnzl-777/SRAM-Read-Write) project could help them diagnose a faulty SRAM chip in a CNC machine. 

The project itself was something I cooked up back when I was in high school, with my own “lab" space at home[^1] and plenty of old electronics to tinker with courtesy of a teacher I knew. I still remember reading the datasheets with the myriad of timing diagrams and the joy of seeing that I had successfully written to and read from the chip!


{{ figure(src="/posts/thank-you-user/arduino-mega-sram-setup.jpg", caption="I’m sure there should be some capacitors/additional components somewhere but the chip seemed perfectly happy being directly plugged into the Arduino MEGA clone.") }}

{{ figure(src="/posts/thank-you-user/timing-diagram.png", caption="Timing diagram for writing to the SRAM chip.") }}

It’s definitely not my finest work[^2] but I learned a great deal about how SRAM worked and for a high schooler it was a welcome addition to a portfolio for college applications[^3].

You can imagine my surprise then that *9 years* after I put this project on GitHub somebody almost 10,000 miles away had run into problems with a CNC machine and thought my work could help!

Unfortunately the particular chip they were interested in testing was different enough that I didn’t feel comfortable recommending code changes without having some version of the chip to test against myself. Furthermore, the memory test in the project was not something I’d consider particularly robust either: it just wrote some random bytes and immediately read it back[^4]. 

They ended up pursuing a different solution but just knowing a project as small and as old as this one got attention from other people is an amazing feeling. 

It reminds me what my mission in being a software engineer has always been:

1. To build tools that enable people to do more
2. To feel good about building such tools (:

May this little post encourage others to more openly share their projects, regardless of how small or niche it seems ❤️ You never know just how impactful the things you build are until you put them out there. 

_If you'd like to leave a comment or have a suggestion/concern, please feel free to reach out to me via my e-mail at: (my first name)z(first letter of my last name).dev@gmail.com_

[^1]: Thanks Mom and Dad!
[^2]: I haven’t had to write C/C++ in some time but I’m skeptical if my usage of the preprocessor would be considered idiomatic…
[^3]: While I still think it would be nice to get the money back from my MIT application, it’s funny to me that I’ve ended up working for a company that has heritage from the university :P 
[^4]: If I had the chance today I would have probably extended the test through things like writing larger patterns and waiting a period of time to verify contents or repeatedly reading and writing to the same area.