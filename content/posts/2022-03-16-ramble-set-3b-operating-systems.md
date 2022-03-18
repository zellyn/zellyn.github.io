---
title: "CS Ramble — Set 3b - operating systems"
slug: "cs-ramble/3b"
date: 2022-03-16T20:10:00-04:00
tags: ['ramble', 'set3']
draft: true
---

_This is post is part of [set 3](../3a/) of [A Ramble Around CS](../../../02/cs-ramble/)._

We'll get to processes, threads, and fibers soon, but first we need to
talk about how all the things your computer is doing at any given time
are organized.

# What is an Operating System?

Early personal computers more or less gave the currently-running
program full control of the machine.

For instance, a simplified map of the memory of the Apple II computer looks like this:

```pikchr
down
LOW: box width 2 height 0.3 fill 0xd8ecf3 thin "Free RAM" bold
TEXT: box same fill lightblue height 0.2 "Text screen data"
GR: box same height 0.5 "Graphical screen data"
RAM: box same as LOW height 3 "Free RAM" bold "(not to scale)" italic
IO: box same as GR height 0.05 "“Memory” that controls devices" above "" above
ROM: box same height 0.4 "ROM"

# Border
box with nw at 1st box.nw width 2 height (first box.n.y - last box.s.y)

arrow from IO.ne + (-0.15,0.17) down until even with IO.n - (0,0.01) thin
```
<div align="center"><i>Apple II Memory Map</i></div>

Within limits[^1], your code was pretty much free to use the **Free
RAM** (“Random Access Memory”) however it wanted.

[^1]: The ROM (“Read-Only Memory”) used certain locations to keep
    track of things like the current position on the screen, current
    drawing color, etc., so you had to avoid those if you wanted to
    keep using the ROM routines.

_We'll discuss TBD in the next part._
