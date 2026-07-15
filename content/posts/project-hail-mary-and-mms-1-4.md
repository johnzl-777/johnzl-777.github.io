+++
title = "Project Hail Mary and MMS 1-4"
description = "A Beatles-related connection between Project Hail Mary and the Magnetospheric Multiscale Mission"
date = 2026-07-14
draft = false

[taxonomies]
tags = ["space", "film"]

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

As an enthusiast of all things space-related, one of my favorite things to do when I’m waiting for some program to finish running at work is look at the [NASA Deep Space Network (DSN) Dashboard](https://eyes.nasa.gov/apps/dsn-now/dsn.html) on my portable monitor[^1] :

{{ figure(src="/posts/project-hail-mary-and-mms-1-4/portable-monitor-dsn.jpeg", caption="The Work Page on my old site. It will be missed!", caption="Please pardon the dust (and desk trinkets)! Can you tell I also love my home state of California a bunch? :P" )}}

It’s really cool to me seeing lots of familiar names like Voyager 2, New Horizons, and Hayabusa2[^2] all sending and receiving data from Earth live! The dashboard/monitor also plays a similar role to the [Hollerith Cards](https://en.wikipedia.org/wiki/Punched_card) I used to have lining my cubicle at Intel: it’s a great way to start conversation with new folks on a topic I find really enjoyable.

One nice consequence of looking at the DSN Dashboard long enough is that I’ve come to learn about satellites and missions I didn’t know about before. For example, [Solar TErrestrial RElations Observatory (STEREO) A](https://science.nasa.gov/mission/stereo/) (which, along with STEREO B obtained awesome 3D images of our sun) was not something I had any awareness of whatsoever until I saw the letters “STA” on the dashboard:

{{ figure(src="/posts/project-hail-mary-and-mms-1-4/stereo-a-dsn.png")}}

Another set of satellites I wasn’t aware of was the [Magnetospheric Multiscale Mission (MMS)](https://mms.gsfc.nasa.gov/) which consists of four satellites (numbered 1-4) studying how the Earth and Sun's magnetic fields interact in the process of *magnetic reconnection*[^3]. I have to admit that it wasn’t so much seeing “MMS 3” on the dashboard that led me to research this mission further as it was the intriguing shape of the satellites themselves:


{{ figure(src="/posts/project-hail-mary-and-mms-1-4/mms-art-nasa-gsfc.jpg" , caption="Image courtesy of NASA/GSFC") }}


It has these rather long booms sticking from the top and bottom, with a similar “scaffolding” structure that made me think of the Voyager satellites when I first saw them[^4]. 

I also find the pancake/spinning-top like structure of the satellites to be distinct from what I’ve usually come to expect from most satellites I’ve seen: some kind of cube/cylinder with the solar panels sticking out:


{{ figure(src="/posts/project-hail-mary-and-mms-1-4/mro-art.jpg" ,caption="Artist's Concept of the Mars Reconnaissance Orbiter - Image courtesy of NASA/JPL" ) }}


### What Does This Have to Do with Project Hail Mary?


{% caution(title="PROJECT HAIL MARY MOVIE SPOILERS AHEAD!") %}
If you haven’t seen the film, I strongly encourage you to (saying this as somebody that’s watched it three times in theaters ❤️). Please consider reading the book too! I haven’t read it myself but some of the excerpts seem quite entertaining. 
{% end %}

Remember how part of the mission was to send back findings from the mission on “Beetle” probes? 

{{ figure(src="/posts/project-hail-mary-and-mms-1-4/phm-probes-slide.png", caption="Please forgive the terrible image quality, I had to screenshot this from a video on YouTube")}}

And how the probes have the names of the members of the Beatles engraved/printed on them?

{{ figure(src="/posts/project-hail-mary-and-mms-1-4/beetle-beatles-name.png") }}

Well guess what the other names for the MMS satellites are:

{{ figure(src="/posts/project-hail-mary-and-mms-1-4/gunters-space-page-mms.png" ,  caption="Obtained from Krebs, Gunter D. “MMS 1, 2, 3, 4”. Gunter's Space Page. Retrieved July 13, 2026, from https://space.skyrocket.de/doc_sdat/mms.htm") }}

I don't think Andy Weir was specifically thinking of the MMS mission so much as The Beatles but I thought this was a rather fun coincidence. I at least hope the folks on the MMS mission find it entertaining (:

## Research Note

While Gunter’s Space Page was the first one I ran into that showed the alternate naming scheme I couldn’t find any more detail on what its exact origins are. I gave Google Gemini a shot and while it did dig up the [OSCAR page](https://space.oscar.wmo.int/satellites/view/mms_a) which had a bullet point giving more confirmation of the naming, it also ended up hallucinating explanations from other sources.

If anybody reading this knows somebody on the MMS mission that could give some more detail as to how this naming came about, I would love to hear about it! That and if there are any other missions that leverage The Beatles in naming (I imagine it would have to be some configuration of four satellites though...)

{% note(title="Thanks Audrey!") %}
I want to give a shout out to Audrey Dakis, a life-long friend of mine that I’ve known since elementary school. She obtained her Bachelor’s in Microbiology with minors in Chemistry, Physics, and Astronomy from Sacramento State and always comes to my mind when I learn more about space. I’m grateful beyond words to have her as a friend and hope there are more folks like her that will continue humanity's quest to explore the universe, how it works, and our place in the cosmos ✨
{% end %}

_If you'd like to leave a comment or have a suggestion/concern, please feel free to reach out to me via my e-mail at: (my first name)z(first letter of my last name).dev@gmail.com_



[^1]: I actually have a whole thing where, depending on how I’m feeling, I have different kinds of “background visual noise” I put on that display. While I love the DSN Dashboard I also like to display:
    - the [NOAA Space Weather Enthusiasts Dashboard](https://www.spaceweather.gov/communities/space-weather-enthusiasts-dashboard)
    -  [HAL 9000 display animations in 4K](https://www.youtube.com/watch?v=AlN9WQaTp-s)
    -  [Internet Engineering Task Force RFCs slowly printed](https://github.com/brunobraga/termsaver)
    -  as well as [tracking the stars from my terminal](https://github.com/da-luce/astroterm)!
    If I feel like I _really_ need the noise I’ll set up tmux so I can see the two CLI apps mentioned previously run at once. Honorable mention to [btop](https://github.com/aristocratos/btop) as well, which actually does have utility for me in hunting down certain hungry processes while looking super cool!
[^2]: Congratulations to all involved at JAXA for Haybusa2’s [successful flyby of Torifune](https://global.jaxa.jp/press/2026/07/20260706-3_e.html)! It always amazes me I live in an age where we can obtain such close images of asteroids.
[^4]: This [American Scientist article by James L. Burch and James F. Drake](https://www.americanscientist.org/article/reconnecting-magnetic-fields) has a great explanation of what magnetic reconnection is along with the history of the research surrounding it.
[^3]: I have to admit I actually always thought the Voyager booms were antennae but some Googling has informed me they’re intended for sensors like a magnetometer. [This NASA Site](https://science.nasa.gov/mission/voyager/where-are-voyager-1-and-voyager-2-now/) has a helpful diagram of the instrumentation on the Voyager satellites while this [Goddard Space Flight Center page](https://mms.gsfc.nasa.gov/spacecraft.html) also confirms the structures in the images are booms. 