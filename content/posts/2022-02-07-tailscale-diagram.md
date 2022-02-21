---
title: "Tailscale diagram in Pikchr"
slug: "tailscale-diagram-in-pikchr"
date: 2022-02-07T00:49:00-05:00
tags: ['pikchr', 'tailscale']
---

Just for fun, I decided to try to use [Pikchr](https://pikchr.org) to
reproduce one of the diagrams in Tailscale's [How NAT traversal
works](https://tailscale.com/blog/how-nat-traversal-works/) article,
which often receive praise.

Pikchr is not intended for this kind of diagram:

> Pikchr is not intended as a replacement for point-and-click diagrams
> creation software. Pikchr is to point-and-click systems as Markdown
> is to MS-Word or Google-Docs. Point-and-click interfaces have their
> place. But so do text-based systems such as Markdown and Pikchr.<br/>
> &nbsp; &nbsp; --- [The Intended Scope And Purpose Of Pikchr](https://pikchr.org/home/doc/trunk/doc/purpose.md)

And indeed, reproducing the diagram took me hours, whereas I could
have reproduced it in a point-and-click diagram editor in 10 or 15
minutes. The diagrams I made about [memory and
variables](../cs-ramble/1b/) are a better example of the kind of thing
you can whip up quickly while writing.

Still, it was fun, and hopefully instructive, and those diagrams
really are great.

<!-- make the image wider than the usual 600px in this theme -->
<div style='width:700px;padding:auto;position:relative;left:-75px'>

```pikchr
topmargin = 1
fontscale = 3
color = 0xc65835 # Orange

define $boxlet {
  oval at -0.18,0 height 0.75 width 0.33 color none fill 0xd6d6d6
  box same as last at -0.12,0.28 height 0.2 width 0.2 radius 0
  box at 0,0 height 1 width 1 color none fill 0x343433 radius 0.2 behind last box
}

define $wall {
  box at 0,0 height 3.1 width 0.76 color none fill 0x496495
  line from -0.38*$1,0.34 to $1*0.38,0.16 to $1*0.38,-0.16 to -0.38*$1,-0.34 close color none fill 0xaec0e0
}

define $wall_info {
  text at last.n + (0.0, 0.5) color 0x496495 $2 bold
  circle at last.n + (0.0,0.3) radius 0.25 thickness 0.05 color 0x5a79a6
  line from (last.x-$1*0.11,last circle.c.y) \
    right $1*0.22 then \
    up $1*0.11 left $1*0.11 then \
    down $1*0.11 right $1*0.11 then \
    down $1*0.11 left $1*0.11 then \
    up $1*0.11 right $1*0.11 \
    color 0x496495 thickness 0.05
}

define $area {
  box at ($1,$2) width $3 height $4 color none fill 0xf9f9f8 radius 0.2
}

# Servers

$area(0.55,0,5.2,3.1)

WS1: [ $boxlet() ] at (0, 0)
text at last.n + (0.0, 0.1) color 0x595857 "VPN Client" above
text at last.s - (0.0, 1) color 0x030303 $2 "Workstation" below

$area(0,-5.25,4.1,3.1)
$area(2.83,-5.25,0.8,3.1)

WS2: [ $boxlet() ] at (0, -5.25)
text at last.n + (0.0, 0.1) color 0x595857 "VPN Client" above
text at last.s - (0.0, 1) color 0x030303 "Workstation" below

$area(8.3,-2.3,6,5.25)

HUB: [ $boxlet() ] at (8.3, -2.4)
text at last.n + (0.0, 0.1) color 0x595857 "VPN Hub"  above
text at last.s - (0.0, 1.78) color 0x030303 "Server" below

$area(16.10,HUB.c.y,4.1,3.1)
$area(13.27,HUB.c.y,0.8,3.1)

WS3: [ $boxlet() ] at (16.10, HUB.c.y)
text at last.n + (0.0, 0.1) color 0x595857 "VPN Client"  above
text at last.s - (0.0, 1) color 0x030303 "EC2 VM" below

[ $wall(1) ] at (3.20,0)
$wall_info(1, "Windows Firewall")

[ $wall(1) ] at (3.2,-5.25)
$wall_info(1, "Office Firewall")

[ $wall(-1) ] at (12.9,HUB.c.y)
$wall_info(-1, "AWS" bold "Security Group" bold "")

dot at WS1.e radius 0.05
dot at WS2.e radius 0.05
dot at WS3.w radius 0.05

line from WS1.e right 4 then down until even with (0,HUB.y+0.2) right 2.3 then right until even with (HUB.w-0.03,0) thickness 0.04 radius 1
line up 0.15 left 0.15 down 0.15 right 0.15 down 0.15 left 0.15 up 0.15 right 0.15 thickness 0.04

line from WS2.e right 4 then up until even with (0,HUB.y-0.2) right 2.3 then right until even with (HUB.w-0.03,0) thickness 0.04 radius 1
line up 0.15 left 0.15 down 0.15 right 0.15 down 0.15 left 0.15 up 0.15 right 0.15 thickness 0.04

line from WS3.w to 0.03 right of HUB.e thickness 0.04
line up 0.15 right 0.15 down 0.15 left 0.15 down 0.15 right 0.15 up 0.15 left 0.15 thickness 0.04
```
</div>

_(Click image to see Pikchr source)_
