---
title: "gopikchr: a yakshave"
date: 2022-01-30T13:11:00-05:00
tags: ['pikchr']
aliases:
  - '/gopikchr-a-yakshave/'

---

I just completed part of a long
[yak-shave](https://en.wiktionary.org/wiki/yak_shaving) to add
[Pikchr](https://pikchr.org) support to my blog, generated with
[Hugo](https://gohugo.io). I hope it helps to spread of Pikchr, and
spurs implementations in other publishing pipelines.

Actually, it's just the bottom part of an even longer yak-shave!

```pikchr {toggle=true,widthlimit=FALSE}
down
box width 3.5in height 0.2in fill 0xc5d8ef \
                       "open my garage with a raspberry pi"
box same fill 0xd9e6f2 with .nw at linewid/4 east of 1st box.sw "achieve accurate timing on a raspberry pi"
box same as 1st box with .nw at linewid/4 east of 2nd box.sw    "understand how to use DMA for gpio sequences"
box same as 2nd box with .nw at linewid/4 east of 3rd box.sw   "“I should document this in a blog post”"
box same as 1st box with .nw at linewid/4 east of 4th box.sw   "“I need Pikchr for diagrams”: make Pikchr work in hugo"
box same as 2nd box with .nw at linewid/4 east of 5th box.sw   "make Pikchr work in hugo"
box same as 1st box with .nw at linewid/4 east of 6th box.sw   "port Pikchr to go"
box same as 2nd box with .nw at linewid/4 east of 7th box.sw   "port the lemon parser to go"
text at 5th box.e  " ✓" ljust
text at 6th box.e  " ✓" ljust
text at 7th box.e  " ✓" ljust
text at 8th box.e  " ✓" ljust
text at last box.s - (0,0.15) "(Click to see source)" italic small
```

[Pikchr](https://pikchr.org) is a modern version of Brian Kernighan's
[PIC markup language](https://en.wikipedia.org/wiki/PIC_(markup_language)),
created by the folks who make SQLite and Fossil. Like so many things from
Bell Labs, it is extremely powerful, once you invest just a little time
to understand how it works.

Pikchr is implemented in a single file,
[`pikchr.y`](https://pikchr.org/home/doc/trunk/doc/build.md),
processed using the [Lemon
Parser](https://sqlite.org/src/doc/trunk/doc/lemon.html) to generate
`lemon.c`.

_I decided to port both the Lemon Parser and Pikchr from C to Go, by
hand._

## Why?

Go and Pikchr are great! A pure-Go port of Pikchr might be useful to
others.

Hugo might more readily accept pure-Go additions.

_Sheer bloody-mindedness._ At work we always have to make pragmatic
decisions. The repressed urge to fully shave the yak comes out in my
own time.

I respect the SQLite folks: their software is an amazing gift to our
community. Think of this as a [gift](https://apenwarr.ca/log/20211229)
too.

## How?

By hand. I looked at [ccgo](https://pkg.go.dev/modernc.org/ccgo/v3),
the amazing C-to-Go compiler used to generate [modernc
sqlite](https://pkg.go.dev/modernc.org/sqlite), a pure-Go translation
of sqlite. However, the code it generats to perform its magic is
(necessarily) complicated and ugly, which seems contrary to the spirit
of Pikchr. Nothing for it, but to dive in by hand. It can be almost
meditative:

1. Translate `lemon.c` to Go, but still generate C code
2. Find all the bugs until it compiles and generates the exact same C
   code
3. Translate the parser template, `lempar.tpl` to Go, and generate Go
   code
4. Laboriously find all the bugs
5. Translate `pikchr.y` from C to Go
6. Once again, fix all the bugs until the Go version generates exactly
   the same Pikchr output

In all, it was almost 12,000 lines of code. I started out trying to be
clever, but learned to keep things as similar as possible.

- I tried to replace `char *` with `[]rune` at first, to handle
  unicode rather than ascii. But I ended up using `[]byte`, and even
  zero-terminating the string! To move forward through the input, `z
  += 3` becomes `z = z[3:]`
- I tried replacing dynamically-sized arrays represented by `struct *`,
  `current_size`, and `n` (current index) with just slices. However, it turns
  out to be cleaner to keep `n` around, because the code will often
  reference `s[n+1]` or `s[n-1]`.

## Links

- [golemon](https://github.com/gopikchr/golemon), the port of lemon to Go
- [gopikchr](https://github.com/gopikchr/gopikchr), the port of Pikchr to Go
- [goldmark-pikchr](https://github.com/gopikchr/goldmark-pikchr), a
  goldmark plugin to render Pikchr diagrams in markdown
- [hugo commit](https://github.com/zellyn/hugo/commit/87bbe9f2140a21e6d2759e7005c9e5a787651832),
  the tiny change needed to enable Pikchr support in Hugo in my fork

## What next?

- Find the remaining bugs. I'm certain I didn't find them all
- Set up github actions to create issues in the golemon and gopikchr
  repositories whenever the C versions in Fossil change. Both change
  infrequently, so keeping up should be little work
- Possibly, use these techniques to port the SQLite parser to Go, if
  [someone else doesn't do it
  first](https://github.com/kyleconroy/sqlc/issues/161#issuecomment-1022541349). A
  Go SQLite-compatible parser seems more likely to get widely used,
  and depended on, and I'm not certain I want to be the maintainer of
  a [popular](https://xkcd.com/2347/) open source package
- Enjoy using Pikchr, and seeing other people use it!

&nbsp; <!-- vertical spacer -->

```pikchr {toggle=true}
circle radius 1 fill yellow
arc from -0.75,0 to 0,-0.75 thick
arc same from 0,-0.75 to 0.75,0
ellipse height 0.5 width 0.25 at -0.3,0.4 fill black
ellipse same at 0.3,0.4
```

&nbsp; <!-- vertical spacer -->

## Comments? Reach me on Twitter

{{< tweet user="zellyn" id="1488960414595620870" >}}