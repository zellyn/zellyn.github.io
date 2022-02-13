---
title: "CS Ramble — Week 1c: variables"
slug: "cs-ramble/1c"
date: 2022-02-03T20:12:00-05:00
tags: ['ramble', 'week1']
# draft: true
# _build:
#  list: never
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
text at last arc.start "z" below "(258 = 2 + 1×256)" below
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

## Pointers

To talk about text, we first need to introduce something called a
“pointer”. You may have run into that term before, and it may have
been confusing or intimidating.

Actually, it's a way of talking about something we've already seen.

Remember when we said we'd put `42` in `a`, and that our program
happened to put `a` in location `1234` in memory? Here's the diagram
again:

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

Well, `1234` is called the “address” of `a`. If you put that in a
variable, say `b`, then we say `b` is a “pointer” to `a`:

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
box same "210" bold
box same "4" bold

text at 1st box.n "1233" above
text at 2nd box.n "1234" above
text at 3rd box.n "1235" above
text at 4th box.n "1236" above

$brace(2nd box.nw.x, 2nd box.nw.y+0.2, 0.5, 0.1)
text at last arc.start + (0,0.03) "a" big bold above

$brace(last box.se.x, 2nd last box.se.y-0.1, -1, -0.1)
text at last arc.start "b" big bold below "(1234 = 210 + 4×256)" below
```

In Go, you'd do that like this:

```go
a := 42
b := &a

// `b` now points to `a`. It's value is the address at which a's value
// is stored. In Go (and most languages), the computer still keeps
// track of the fact that it is a "pointer to integer", not just a
// generic "pointer", so that it knows what to do with it if you, say,
// print the value:

fmt.Println(a)  // → 42
fmt.Println(*b) // → 42, *b is "the value that `b` points" to (`a`)
fmt.Println(b)  // → 0xc00009c000, when I ran it; of course it varies
fmt.Println(&b) // → 0xc000094018, this is where `b` is stored
```
_([live example for you to play
with](https://go.dev/play/p/DlLW6MtzNSd) - click “run”)_

That last value—`0xc00009c000`—brings up a good point. The example we
showed above, with `b` getting a value of `1234`, that fits in just
two bytes, is a little outdated. Two bytes was enough to store
addresses on machines like the Apple II or Commodore 64, which had a
[chip](http://www.visual6502.org/JSSim/) that maxed out at 64k
(256×256 = 65536 bytes). Nowadays, your computer and operating system
are in 32- or 64-bit mode, so pointers take 32 or 64 bits (4 or 8
bytes) to store. So the example above would more accurately look
something like this:

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
box same "210" bold
box same "4" bold
box same "0" bold
box same "0" bold
box same "0" bold
box same "0" bold
box same "0" bold
box same "0" bold
box same "0" bold

text at 1st box.n "1233" above
text at 2nd box.n "1234" above
text at 3rd box.n "1235" above
text at 4th box.n "1236" above
text at 5th box.n "1237" above
text at 6th box.n "1238" above
text at 7th box.n "1239" above
text at 8th box.n "1240" above
text at 9th box.n "1241" above
text at 10th box.n "1242" above
text at 11th box.n "1243" above

$brace(2nd box.nw.x, 2nd box.nw.y+0.2, 0.5, 0.1)
text at last arc.start + (0,0.03) "a" big bold above

$brace(2nd last box.se.x, 2nd last box.se.y-0.1, -4, -0.1)
text at last arc.start "b" big bold below "(&a = 1234 = 210 + 4×256 + 0×65536 + 0×16777216 + 0× ...)" below
```

We don't usually care about exactly how big pointers are (unless we're
in a phase of caring how big everything is), and often draw that last
picture like this:

```pikchr {toggle=true}
text "a:" big bold
box width 0.5 height 0.25 fill lightblue "42"

move to 1st text.w + (0,-0.5)
text "b:" big bold
box width 0.5 height 0.25 fill lightblue
dot at last.c radius 0.03
arrow from last to first box.s
```

Some ways of talking about pointers. We “dereference” `b` to get the
value it points to, `42`. In Go, as mentioned above, and in fact in
many languages that descend from C, this is written `*b`. Conversely,
we can “get a reference to `a`” (`a`'s address in memory) by writing
`&a`. People often talk about “following a pointer”, which means
exactly what it sounds like: just follow the arrow!

You actually now know enough to understand what a “linked list” is!

```pikchr {toggle=true}
text "a:" big bold
box width 0.5 height 0.25 fill lightblue
dot at last radius 0.03

arrow from last dot
box same as first box thin "42" bold
box same
dot at last radius 0.03
text at 2nd last box.n "data" above
text at last box.n "next" above
box same with w at 2nd last box.w width 1 thickness 0.015 fill none

arrow from last dot
box same as 2nd box "17" bold
box same
dot at last radius 0.03
text at 2nd last box.n "data" above
text at last box.n "next" above
box same with w at 2nd last box.w width 1 thickness 0.015 fill none

arrow from last dot
box same as 2nd box "23" bold
box same
dot at last radius 0.03
text at 2nd last box.n "data" above
text at last box.n "next" above
box same with w at 2nd last box.w width 1 thickness 0.015 fill none

arrow from last dot
line up 0.07 down 0.14 up 0.07
move right 0.04
line up 0.07 down 0.14 up 0.07
```
That last pointer is a “null pointer”[^1]: by convention, a pointer with
the value `0`—also called `null`—points to nothing, and is drawn as ⟶‖

[^1]: Many consider null pointers to be a [big
      mistake](https://news.ycombinator.com/item?id=12427069), and
      many modern languages make them impossible.

Let's create that linked list in Go:

```go
// Each Node has some data, and a pointer to the next Node (possibly
// null)
type Node struct {
	data int
	next *Node
}

// Construct the linked list in the diagram above, the laborious way:
node1 := Node{data:42}
node2 := Node{data:17}
node3 := Node{data:23}
node1.next = &node2
node2.next = &node3
node3.next = nil // Go spells "null" as "nil".
a := &node1

// Construct the linked list in a more concise way. `&Node{}` is Go
// shorthand for creating a struct value and then immediately taking
// its address.
a = &Node{
	data: 42,
	next: &Node{
		data: 17,
		next: &Node{
			data: 23,
			next: nil,
		},
	},
}

// Print the linked list by repeatedly following the `next` pointer
// until we get a null pointer.
for a != nil {
	fmt.Println(a.data)
	a = a.next
}

// Prints:
// 42
// 17
// 23
```
_([playground version](https://go.dev/play/p/rh067JWmftb))_

### Values vs References

Once last thing for this part. Remember our earlier `MyThing` example?

```go
// Define a “struct” with two fields, both bytes, named `x` and `y`:
type MyThing struct {
  x byte
  y byte
}

// Create a new variable named `a`, of type MyThing, and give it some
// initial values:
a := MyThing{ x: 42, y: 17 }
b := a
```

which copied `a`'s _value_ into `b`, giving us:
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

What if `a` and `b` were pointers instead?
```go
// Create a new variable named `a`, of type pointer-to-MyThing, and
// give it some initial values:
a := &MyThing{ x: 42, y: 17 }
// Copy a's value into b:
b := a
```

Now, `a`'s value is a pointer: a 64-bit number, the address of that
`MyThing` structure. So copying that value into `b` makes `b` point to
the _same_ structure!

```pikchr {toggle=true}
text "a:" big bold
box width 0.5 height 0.25 fill lightblue
dot at last radius 0.03

arrow from last dot go 1 east
box same "42" thin
box same "17"
text at 2nd last box.n "x" above
text at last box.n "y" above
box same with w at 2nd last box.w width 1 thickness 0.015 fill none

move to 1st text.w + (0,-0.5)
text "b:" big bold
box same as first box
dot at last radius 0.03
arrow from last dot go right 0.33 then right 0.33 up 0.43 then right 0.33 radius 0.3
```

If you now change part of what `b` points to, you're changing the
thing `a` points to too, since they point to the same value!

```go
// In Go, you can refer to the `y` field of `b` with `b.y`, whether
// `b` is a struct value, or a pointer to a struct value. In C, you'd
// need to use `b->y` if `b` was a pointer.
b.y = 23

fmt.Println(b.y) // 23
fmt.Println(a.y) // 23
```

# Text

We finally have enough background to write about how text is
stored. Most languages have a `string` type, which stores pieces of
text. A common value for a `string` type, under the covers, is a
pointer to an area of memory to hold the text, and integer holding the
length in bytes.[^2]

[^2]: At least, that's how [Go does
it](https://github.com/golang/go/blob/6f327f7b889b81549d551ce6963067267578bd70/src/runtime/string.go#L238-L241). In
C, it's just a pointer, and you count on a `0` byte to terminate the
string. If that sounds risky, like you could miss the `0`, and just
run off into some other part of memory, well, yep!

The actual characters are stored in memory as we described in the
[last post](../1b):

```go
a := "Hello, world"
```

```pikchr {toggle=true}

text "a:" big bold
box width 0.5 height 0.25 fill lightblue thin
dot at last radius 0.03
box same with w at last box.e "12"
text at 2nd last box.n "str" above
text at last box.n "len" above
box same with w at 2nd last box.w width 1 thickness 0.015 fill none

box at 1 se of first box.e width 0.5 * linewid height 0.5*linewid fill lightblue "H" bold
box same "e" bold
box same "l" bold
box same "l" bold
box same "o" bold
box same "," bold
box same "␣"
box same "w" bold
box same "o" bold
box same "r" bold
box same "l" bold
box same "d" bold

box fill 0xd8ecf3 with n at H.s height H.height width H.width "72"
box same "101"
box same "108"
box same "108"
box same "111"
box same "44"
box same "32"
box same "119"
box same "111"
box same "114"
box same "108"
box same "100"

arc from last dot to H.w ->
```

How about letters and words in foreign scripts? Well, for that, you
need a “character encoding”, a way of giving each character a number
and a representation as bytes. The most common one used to be
[ASCII](https://www.asciitable.com/)[^3], but the modern answer the
problem is [Unicode](https://home.unicode.org/), an ambitious scheme
to assign a unique number to every letter in every alphabet of every
written language. And a bunch of
[Emojis](https://emojipedia.org/poodle/). And [other
things](https://unicode-table.com/en/blocks/box-drawing/).

[^3]: On Unix systems, like Linux and Mac OS, you can type `man ascii`
in a terminal window to see the ASCII values. The bottom set shows
them in decimal, and is probably what you want. We'll get into Octal
and Hexadecimal and all that some other time.

Unicode takes care of giving each character/symbol a unique
number[^4], but the numbers get pretty big, and you still have to
figure out how to represent them in just little old bytes. For that
you need an _encoding_. Unless you have good reasons and know what
you're doing, you'll mostly want to use
[UTF-8](https://sethmlarson.dev/blog/utf-8), which leaves the lower
128 ASCII characters alone, and then uses a clever encoding to encode
all the other Unicode code points. The [origin
story](https://www.cl.cam.ac.uk/~mgk25/ucs/utf-8-history.txt) is fun.

[^4]: Actually, it gives each _code point_ a number, and then one or
more of those combine to make one “user-perceived character”, as
Wikipedia puts it. It's complicated. [This post by Manish
Goregaokar](https://manishearth.github.io/blog/2017/01/14/stop-ascribing-meaning-to-unicode-code-points/)
goes into lots of details.

# Summary

Well, that's been a fun ramble; thanks for sticking with us!
Remember, you can start at any of the concepts we've discussed here,
and explore as deeply as you want. There are is nothing wrong with not
knowing, questions, disinterest, or _way_ too much interest, and no
such thing as bad questions or things you're “supposed” to know.
