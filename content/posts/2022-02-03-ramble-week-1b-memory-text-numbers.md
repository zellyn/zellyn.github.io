---
title: "CS Ramble — Week 1b - memory, text, numbers"
slug: "cs-ramble/1b"
date: 2022-02-03T08:00:00-05:00
tags: ['ramble', 'week1']
---

_This is post is part of [week 1](../1a/) of [A Ramble Around
CS](../)._


# Computer memory

You can imagine all your computer's memory as a series of little boxes,
numbered from 0 (because we're computer people), and going up, and up,
and up. The laptop I'm typing this on has 32MB of memory, or
34,359,738,368 little boxes![^1]

[^1]: That's 32×1024×1024×1024. 1024 bytes is a “kilobyte”, because
      1024 is close to 1000, 1024×1024=1048576 bytes is a
      “megabyte”, because it's close to a million bytes, and
      1024×1024×1024=1073741824 bytes is a “gigabyte”.[^2]

[^2]: Technically, according to the [SI
      prefixes](https://en.wikipedia.org/wiki/Metric_prefix), a
      “kilobyte” (KB) is exactly 1000 bytes, and you should use
      “kibibyte” (KiB) to refer to 1024 bytes. Since “kibibyte” sounds
      more like a Pokémon or a kind of dog food for small, yappy dogs,
      _nobody_ uses it. Except hard disk manufacturers, who insist on
      using exact powers of 10 to make their hard disks sound 10%
      bigger (for terabytes) than they really are.


```pikchr
boxwid /=4
boxht /= 2
box fill lightblue
box same
box same
box same
box same
box same
box same
box same
box same
box same
box same
box same

move width boxwid*2

box same //  "34,359,738,367" above

text at last move "· · ·" big bold

text at 1st box.n "0" above
text at 2nd box.n "1" above
text at 3rd box.n "2" above
text at 4th box.n "3" above
text at 5th box.n "4" above
text at 6th box.n "5" above
text at 7th box.n "6" above
text at 8th box.n "7" above
text at 9th box.n "8" above
text at 10th box.n "9" above
text at 11th box.n "10" above
text at 12th box.n "11" above
text at 13th box.nw "34,359,738,367" above ljust
```

We'll get into “bits” and “bytes” and counting in “binary” later, but
for now, let's just take it as given that each of those boxes holds a
single “byte”: a number from 0 to 255.

# Text

If we want to store letters in the boxes, we have to come up with some
kind of “[Character
Encoding](https://en.wikipedia.org/wiki/Character_encoding)”,
assigning a number to each letter.

_I'd use 1 for “A”, 2 for “B”, ..., except it's probably better to
just jump straight to the actual numbers your computer (usually) uses
(called “[ASCII](https://en.wikipedia.org/wiki/ASCII)”[^3]) so we get used
to seeing the real numbers:_

[^3]: If you're on Linux or MacOS, you can type `man ascii` in your
terminal to see a list of ASCII codes. Ignore the hexadecimal (for
now) and octal (forever) sections; the decimal section at the bottom
is useful.

```pikchr
right
text "“Hello, world”"
arrow from last text.e - (0.12,0) width linewid/2

box width 0.5 * linewid height 0.5*linewid fill lightblue "H" bold
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

box fill 0xd8ecf3 with n at 1st box.s height H.height width H.width "72"
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

text at 1st box.n "0" above
text at 2nd box.n "1" above
text at 3rd box.n "2" above
text at 4th box.n "3" above
text at 5th box.n "4" above
text at 6th box.n "5" above
text at 7th box.n "6" above
text at 8th box.n "7" above
text at 9th box.n "8" above
text at 10th box.n "9" above
text at 11th box.n "10" above
text at 12th box.n "11" above
```

# Numbers

Storing numbers in the boxes is easy if they're between 0 and
255. Just stick them in there!

```pikchr
box width 0.5 * linewid height 0.5*linewid fill lightblue "42" bold
box same "17" bold
box same "0" bold
box same "255" bold

text at 1st box.n "0" above
text at 2nd box.n "1" above
text at 3rd box.n "2" above
text at 4th box.n "3" above
```

For negative numbers, we pretend some of the numbers are
negative:

```pikchr
down
box width linewid height 0.5*linewid "0"
box same "1"
box same "2"
text  "⋮"
box same "126"
box same "127"
box same "128"
box same "129"
box same "130"
text "⋮"
box same "253"
box same "254"
box same "255"

text at 1st box.n "Value" bold above

I1: box same as 1st box with w at 1st box.e + (0.5*linewid,0) "0"
box same "1"
box same "2"
text  "⋮"
box same "126"
box same "127"
box same "-128"
box same "-127"
box same "-126"
text "⋮"
box same "-3"
box same "-2"
box same "-1"

text at I1.n "Interpretation" bold above
```

Probably, it's better to think of it in
[modular](https://www.youtube.com/watch?v=5OjZWSdxlU0)
([clock](http://math.ucdenver.edu/~wcherowi/clockar.html)) arithmetic:

```pikchr
circle radius 1.5 fill 0xd8ecf3

define $inner {
  line from (0,0) then 1.3 heading $1 invisible
  text at last line.end $2
}

define $outer {
  line from (0,0) then 1.65 heading $1 invisible
  text at last line.end $2
}

$inner(0, "0"); $outer(0, "0" bold)
$inner(15, "1"); $outer(15, "1" bold)
$inner(30, "2"); $outer(30, "2" bold)
$inner(45, "3"); $outer(45, "3" bold)
$inner(60, "4"); $outer(60, "4" bold)
$inner(75, "·" bold); $inner(73, "·" bold); $inner(71, "·" bold)
$outer(75, "·" bold); $outer(73, "·" bold); $outer(71, "·" bold)
$inner(345, "255"); $outer(345, "-1" bold)
$inner(330, "254"); $outer(330, "-2" bold)
$inner(315, "253"); $outer(315, "-3" bold)
$inner(300, "252"); $outer(300, "-4" bold)
$inner(285, "·" bold); $inner(287, "·" bold); $inner(289, "·" bold)
$outer(285, "·" bold); $outer(287, "·" bold); $outer(289, "·" bold)

$inner(135, "·" bold); $inner(137, "·" bold); $inner(139, "·" bold)
$outer(135, "·" bold); $outer(137, "·" bold); $outer(139, "·" bold)
$inner(150, "126"); $outer(150, "126" bold)
$inner(165, "127"); $outer(165, "127" bold)
$inner(180, "128"); $outer(180, "-128" bold)
$inner(195, "129"); $outer(195, "-127" bold)
$inner(210, "130"); $outer(210, "-126" bold)
$inner(225, "·" bold); $inner(223, "·" bold); $inner(221, "·" bold)
$outer(225, "·" bold); $outer(223, "·" bold); $outer(221, "·" bold)


text at (0, 1) "Value" big bold
text at (0, 1.9) "Interpretation" big bold
text at (0,0) "Signed interpretation" italic "of byte values" italic
```

Since there are 256 spots in a full circle, going 255 spaces clockwise
(adding 255) is the same as going one space counter-clockwise
(subtracting 1).

But that still only gives us 256 different values.

## Bigger numbers

For bigger numbers, we'll have to combine pairs of bytes, or groups of
4 or 8 (or more) bytes.

In normal arabic numerals, we have **ten** choices, 0–9, and then
spill over to the next space, whose value is multiplied by **ten**.

We can do the same with bytes: we have **256** choices, 0–255, and
then spill over to the next space, whose value is multiplied by
**256**.

```pikchr
down
box width linewid height 0.5*linewid "0"
box same "1"
box same "2"
text  "⋮"
box same "254"
box same "255"
box same "256"
box same "257"
box same "258"
text  "⋮"
box same "65534"
box same "65535"

text at 1st box.n "Value" bold "" "" ""

I1: box same as 1st box with w at 1st box.e + (0.5*linewid,0) width 0.25 fill 0xd8ecf3 "0"
box same "0"
box same "0"
text  "⋮"
box same "0"
box same "0"
box same "1"
box same "1"
box same "1"
text  "⋮"
box same "255"
box same "255"

I2: box same as I1 with w at I1.e "0"
box same "1"
box same "2"
text  "⋮"
box same "254"
box same "255"
box same "0"
box same "1"
box same "2"
text  "⋮"
box same "254"
box same "255"

text at I1.ne "Representation" bold "as 2 bytes" bold "" ""
```

So, `255 = 0×256 + 255`, and `258 = 1×256 + 2`. I've put the “×256”
byte first, to match how in the number “12”, the “×10” digit goes
first. Which byte you put first is a choice, and most current
computers actually put the littlest byte first and the “×256” byte
second. This is called “little-endian”, because the littlest byte
comes first. The opposite
“[endianness](https://en.wikipedia.org/wiki/Endianness)” is of course
“big-endian”. When you're storing things on disk, or [sending numbers
to a Raspberry Pi with a
laser](https://hackaday.com/2015/12/12/raspberry-pi-communication-via-laser/),
you can pick your own endianness!

Just like with individual bytes, you can use half the space for
negative numbers, and turn the range 0…65535 into -32768…32767.

## A note on names for things

All of these things have multiple names you might run into:

| Bytes | Signed? | Min    | Max   |  Common names                                         |
|:-----:|:-------:|:------:|:-----:|:------------------------------------------------------|
| 1     |         | 0      | 255   | `(unsigned)` _`byte`_, `(unsigned)` _`char`_, `uint8` |
| 1     | ✓       | -128   | 127   | `(signed)` _`char`_, _`byte`_, `int8`                 |
| 2     |         | 0      | 65535 | _`unsigned int`_, `unsigned short`, `uint16`          |
| 2     | ✓       | -32768 | 32767 | _`int`_, `short`, `int16`                             |
| 4     |         | 0      | 2³²-1 | _`unsigned int`_, _`unsigned long`_, `uint32`         |
| 4     | ✓       | -2³¹   | 2³¹-1 | _`int`_, _`long`_, `int32`, `rune`                    |
| 8     |         | 0      | 2⁶⁴-1 | _`unsigned long`_, `uint64`, _`uint`_                 |
| 8     | ✓       | -2⁶³   | 2⁶³-1 | _`long`_, `int64`, _`int`_                            |

Note that many of the terms are ambiguous, between programming
languages, or even between computers.  In Go, for example, “int” can
mean 32 or 64 bits, depending on whether your computer is running in
32- or 64-bit mode. I've marked ambiguous names in _`italics`_.

_In the [next part](../1c/), we'll discuss how your program's
variables point to things in memory._
