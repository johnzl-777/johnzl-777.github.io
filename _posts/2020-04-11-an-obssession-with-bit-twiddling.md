---
layout: post
title: An Obsession With Bit Twiddling
description: My new obsession with bit-level manipulations
tags: bit intel c
---

A couple days ago, I stumbled upon an old folder on my desktop filled with information on the Intel x86 architecture and embedded systems development.

If I recall correctly, I was preparing for an interview with the Folsom Intel site. If you go to the [work section]({{ site.baseurl }}{% link _pages/work.md %}) of my website you'll note that I interned there before but only as a High School intern. This time I was shooting for a college internship (better pay and the possiblity of working with embedded systems).

I turned that offer down after a slew of unfortunate complications but I had a blast digging up a bunch of old bit twiddling hacks within that folder.

For those that don't know *bit twiddling* (also known as *bit manipulation* or just *bit hacks*) are incredibly low-level manipulations of individual bits (usually stored in some kind of integer, `char`, etc.)

Some examples of bit hacks include:
* Counting the number of set bits
* Swapping bits within a value
* Interleaving bits from two other integers
* Find the leftmost 1
* Right-propagate the rightmost 1

If you're wondering why any of this is useful, you're in the same boat as I am.

From what I can tell, many of these hacks are nice for embedded systems, where every byte and every processor cycle counts. The origins of many of these hacks seem to come from rather old CPU-level optimizations of certain arithmetic operations and graphics operations. 

Either way, I find myself absolutely loving these little hacks. Using a small set of operations (AND, OR, XOR, NOT) you can perform this kind of "bit wizardry". 

I feel like it's the equivalent of reading a good haiku. It's short but it leaves you with the impression that you've witnessed true grace.

My only wish is that for every bit hack I encountered there would be a little more information on __*why*__ this stuff was (and hopefully still is) useful. 

For example, I found this rather cool example (courtesy of [this page](https://graphics.stanford.edu/~seander/bithacks.html))of using a lookup table to count the number of bits set in an unsigned integer
```c
static const unsigned char BitsSetTable256[256] = 
{
#   define B2(n) n,     n+1,     n+1,     n+2
#   define B4(n) B2(n), B2(n+1), B2(n+1), B2(n+2)
#   define B6(n) B4(n), B4(n+1), B4(n+1), B4(n+2)
    B6(0), B6(1), B6(1), B6(2)
};

unsigned int v; // count the number of bits set in 32-bit value v
unsigned int c; // c is the total bits set in v

// Option 1:
c = BitsSetTable256[v & 0xff] + 
    BitsSetTable256[(v >> 8) & 0xff] + 
    BitsSetTable256[(v >> 16) & 0xff] + 
    BitsSetTable256[v >> 24]; 

// Option 2:
unsigned char * p = (unsigned char *) &v;
c = BitsSetTable256[p[0]] + 
    BitsSetTable256[p[1]] + 
    BitsSetTable256[p[2]] +	
    BitsSetTable256[p[3]];


// To initially generate the table algorithmically:
BitsSetTable256[0] = 0;
for (int i = 0; i < 256; i++)
{
  BitsSetTable256[i] = (i & 1) + BitsSetTable256[i / 2];
}
```
I was pretty amazed by the set of macros used to generate the table (how does that even work?) but I was more concerned with
> *why would I ever need to count the number of set bits in a piece of data?*

I wasn't willing to just dismiss this as "a neat trick" and sure enough, I learned about the *hamming weight* of a value, which (in the context of binary number system) is just the number of 1's that have been set. 

Turns out this is pretty important for a couple of other operations [according to Wikipedia](https://en.wikipedia.org/wiki/Hamming_weight). 

Unfortunately, not all bit hacks have this nice 1:1 where I can look up what the hack does and see its applications. 

Perhaps one day, I'll be a true bit wizard but for now, it suffices for me to just enjoy and pick apart what gems currently exist, whether they are useful or not. 

