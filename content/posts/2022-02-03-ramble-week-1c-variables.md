---
title: "CS Ramble — Week 1c: variables"
slug: "cs-ramble/1c"
date: 2022-02-03T20:12:00-05:00
tags: ['ramble', 'week1']
draft: true
---

_This is post is part of [week 1](../1a/) of [A Ramble Around
CS](../)._

# Variables

When you say `a = 42`, what is happening in memory? Well, `a` is
actually just a synonym for a particular piece of your computer's
memory.

Let's say your program happend to put `a` in memory location `1234`:


```pikchr {toggle=true}
box width 0.5 height 0.25 fill lightblue "0" bold
box same "42" bold
box same "0" bold
box same "0" bold

text at 1st box.n "1233" above
text at 2nd box.n "a" big bold above "1234" above
text at 3rd box.n "1235" above
text at 4th box.n "1236" above
```

If we don't care about the exact memory location (and we usually
don't), we can draw that like this:

```pikchr {toggle=true}
box width 0.5 height 0.25 fill lightblue "42"
text at last box.w "a:  " bold rjust big
```

Now, when you say `b = a` (and assuming your program happens to pick
`1235` for `b`), we get:

```pikchr {toggle=true}
box width 0.5 height 0.25 fill lightblue "0" bold
box same "42" bold
box same "42" bold
box same "0" bold

text at 1st box.n "1233" above
text at 2nd box.n "a" big bold above "1234" above
text at 3rd box.n "b" big bold above "1234" above
text at 4th box.n "1236" above
```

or, alternatively:

```pikchr {toggle=true}
down
box width 0.5 height 0.25 fill lightblue "42"
move
box width 0.5 height 0.25 fill lightblue "42"

text at first box.w "a:  " bold rjust big
text at last box.w "b:  " bold rjust big
```

As you can see, the program has made a _copy_ of the value of `a`, and
given `b` the same value.

What if we have a more complicated value? Many languages have
“structure” or “struct” types, which can combine pieces of data together. For
instance, here's a snippet of Go:

```go
// Define a “struct” with two fields, both bytes, named `x` and `y`:
type MyThing struct {
  x byte
  y byte
}

// Create a new variable named `a`, of type MyThing, and give it some
// initial values:
a := MyThing{ x: 42, y: 99 }
```

Now, what we have is this:


```pikchr {toggle=true}
box width 0.5 height 0.25 fill lightblue "0" bold
box same "42" bold
box same "99" bold
box same "0" bold

text at 1st box.n "1233" above
text at 2nd box.n "1234" above
text at 3rd box.n "1235" above
text at 4th box.n "1236" above

arc from 2nd box.nw + (0.05,0.25) to 2nd box.nw + (0,0.2) thin
arc from 3rd box.ne + (0,0.2) to 3rd box.ne + (-0.05,0.25) thin
arc from 2nd box.ne + (-0.05,0.25) to 2nd box.ne + (0,0.3) thin
arc from 2nd box.ne + (0,0.3) to 2nd box.ne + (0.05,0.25) thin
line from 4th last arc.ne to 2nd last arc.sw thin
line from last arc.se to 3rd last arc.nw thin
text at last arc.nw + (0,0.03) "a" bold above
```

or, alternatively:

```pikchr {toggle=true}
text "a:" big bold
box width 0.5 height 0.25 fill lightblue "42" thin
box same "99"
text at 2nd last box.n "x" above
text at last box.n "y" above
box same with w at 2nd last box.w width 1 thickness 0.015 fill none
```

We can update the individual fields of `a`:

```go
// Update the `9` field of `a` to 17:
a.y = 17
```

And now we have:
```pikchr {toggle=true}
text "a:" big bold
box width 0.5 height 0.25 fill lightblue "42" thin
box same "17"
text at 2nd last box.n "x" above
text at last box.n "y" above
box same with w at 2nd last box.w width 1 thickness 0.015 fill none
```

If we now create a variable `b` with `a`'s value:

```go
// Create a new variable named `b`, also of type MyThing, and give it
// the same value as `a`:
b := a
```

We get:
```pikchr {toggle=true}
text "a:" big bold
box width 0.5 height 0.25 fill lightblue "42" thin
box same "17"
text at 2nd last box.n "x" above
text at last box.n "y" above
box same with w at 2nd last box.w width 1 thickness 0.015 fill none

move to 1st text.w + (0,-0.5)
text "b:" big bold
box width 0.5 height 0.25 fill lightblue "42" thin
box same "17"
text at 2nd last box.n "x" above
text at last box.n "y" above
box same with w at 2nd last box.w width 1 thickness 0.015 fill none
```

Once again, our program created a **copy** of the **value** of `a`.

What if we have wider types inside the struct? No problem, they work
just as described in the [last post](../1b):

```go
// Define a struct with two fields, one a byte, and one a 16-bit
// unsigned integer (which takes up 2 bytes):
type YourThing struct {
  w byte
  z uint16
}

// Create a new variable named `c`, of type YourThing, and give it some
// initial values:
c := YourThing{ w: 42, z: 258 }
```

Which looks like:

```pikchr {toggle=true}
define $brace {
  $offsety = $4 / 2
  $offsetx = $3/abs($3) * abs($offsety)
  $pointx = $1+0.5*$3
  $pointy = $2+$4
  arc from ($1+$offsetx,$2+$offsety) to ($1,$2) thin
  arc from ($1+$3,$2) to ($1+$3-$offsetx, $2+$offsety) thin
  arc from ($pointx-$offsetx,$pointy-$offsety) to ($pointx,$pointy) thin
  arc from ($pointx,$pointy) to ($pointx+$offsetx,$pointy-$offsety) thin
  line from 4th last arc.start to 2nd last arc.start thin
  line from last arc.end to 3rd last arc.end thin
}

box width 0.5 height 0.25 fill lightblue "0" bold
box same "42" bold
box same "2" bold
box same "1" bold
box same "0" bold

$brace(4th last box.nw.x, 4th last box.nw.y+0.1, 1.5, 0.1)
text at last arc.start + (0,0.03) "c" bold above

$brace(4th last box.se.x, 4th last box.se.y-0.1, -0.5, -0.1)
text at last arc.start "w" below

$brace(2nd last box.se.x, 2nd last box.se.y-0.1, -1, -0.1)
text at last arc.start "z" below "(2+1×256 = 258)" below
```

Or, alternatively:

```pikchr {toggle=true}
text "c:" big bold
box width 0.5 height 0.25 fill lightblue "42" thin
box same width 1 "258"
text at 2nd last box.n "w" above
text at last box.n "z" above
box same with w at 2nd last box.w width 1.5 thickness 0.015 fill none
```
