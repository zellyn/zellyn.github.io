---
title: "CS Ramble — Set 2b - bases"
slug: "cs-ramble/2b"
date: 2022-02-16T20:00:00-05:00
tags: ['ramble', 'set2']
---

_This is post is part of [set 2](../2a/) of [A Ramble Around
CS](../)._

# Counting in tens, like a normal human

We humans have 10 fingers, and we use ten digits for counting: **`0,
1, 2, 3, 4, 5, 6, 7, 8, 9`**. You can imagine that counting up works
kind of like a car odometer. We have wheels with 10 digits on each of
them, 0--9. We turn the right-most wheel one digit to add one:

```pikchr
box fill 0x333333 color 0xEEEEEE fit "0" bold fit
box same "0" bold; box same "0" bold; box same "0" bold; box same "0" bold
box same "0" bold
arrow from 0.02 s of 3rd last box.sw then 0.16 s thin; right

box at 0.3 s of 6th last box.s fill 0x333333 color 0xEEEEEE fit "0" bold fit
box same "0" bold; box same "0" bold; box same "0" bold; box same "0" bold
box same "0" bold above "1" bold below
arrow from 0.02 s of 3rd last box.sw then 0.16 s thin; right

box at 0.3 s of 6th last box.s fill 0x333333 color 0xEEEEEE fit "0" bold fit
box same "0" bold; box same "0" bold; box same "0" bold; box same "0" bold
box same "1" bold
arrow from 0.02 s of 3rd last box.sw then 0.16 s thin; right

box at 0.3 s of 6th last box.s fill 0x333333 color 0xEEEEEE fit "0" bold fit
box same "0" bold; box same "0" bold; box same "0" bold; box same "0" bold
box same "1" bold above "2" bold below
arrow from 0.02 s of 3rd last box.sw then 0.16 s thin; right

box at 0.3 s of 6th last box.s fill 0x333333 color 0xEEEEEE fit "0" bold fit
box same "0" bold; box same "0" bold; box same "0" bold; box same "0" bold
box same "2" bold
```

To count to numbers higher that `9`, we start counting up in the
second-rightmost digit. The wheels are rigged up so that when a wheel
goes all the way around back to `0`, it turns the wheel to its left
once, “carrying the 1”:

```pikchr
box fill 0x333333 color 0xEEEEEE fit "0" bold fit
box same "0" bold; box same "0" bold; box same "0" bold; box same "0" bold
box same "9" bold
arrow from 0.02 s of 3rd last box.sw then 0.16 s thin; right

box at 0.3 s of 6th last box.s fill 0x333333 color 0xEEEEEE fit "0" bold fit
box same "0" bold; box same "0" bold; box same "0" bold
box same "0" bold above "1" bold below
box same "9" bold above "0" bold below
arrow from 0.02 s of 3rd last box.sw then 0.16 s thin; right

box at 0.3 s of 6th last box.s fill 0x333333 color 0xEEEEEE fit "0" bold fit
box same "0" bold; box same "0" bold; box same "0" bold
box same "1" bold
box same "0" bold
```

# Counting in eights... like an octopus?

So, let's imagine that octopuses with their eight appendages only use
eight digits for counting: **`0, 1, 2, 3, 4, 5, 6, 7`**.

I suppose they'd count the same way, but start counting up on the
second-rightmost digit after `7`. And the wheels on their odometers
would have only digits 0--7:

```pikchr
box fill 0x333333 color 0xEEEEEE fit "0" bold fit
box same "0" bold; box same "0" bold; box same "0" bold; box same "0" bold
box same "0" bold
arrow from 0.02 s of 3rd last box.sw then 0.16 s thin; right

box at 0.3 s of 6th last box.s fill 0x333333 color 0xEEEEEE fit "0" bold fit
box same "0" bold; box same "0" bold; box same "0" bold; box same "0" bold
box same "0" bold above "1" bold below
arrow from 0.02 s of 3rd last box.sw then 0.16 s thin; right

box at 0.3 s of 6th last box.s fill 0x333333 color 0xEEEEEE fit "0" bold fit
box same "0" bold; box same "0" bold; box same "0" bold; box same "0" bold
box same "1" bold
arrow from 0.06 s of 3rd last box.sw then 0.38 s thin dashed; right

box at 0.6 s of 6th last box.s fill 0x333333 color 0xEEEEEE fit "0" bold fit
box same "0" bold; box same "0" bold; box same "0" bold; box same "0" bold
box same "7" bold
arrow from 0.02 s of 3rd last box.sw then 0.16 s thin; right

box at 0.3 s of 6th last box.s fill 0x333333 color 0xEEEEEE fit "0" bold fit
box same "0" bold; box same "0" bold; box same "0" bold
box same "0" bold above "1" bold below
box same "7" bold above "0" bold below
arrow from 0.02 s of 3rd last box.sw then 0.16 s thin; right

box at 0.3 s of 6th last box.s fill 0x333333 color 0xEEEEEE fit "0" bold fit
box same "0" bold; box same "0" bold; box same "0" bold
box same "1" bold
box same "0" bold
```

So octopi would count:

- 0
- 1
- 2
- 3
- 4
- 5
- 6
- 7 --- _add one, carry the 1:_
- 10
- 11
- 12
- 13
- 14
- 15
- 16
- 17 --- _add one, carry the 1:_
- 20
- 21
- &nbsp;⋮

## Names for these things

We call the way we count “base 10”, or “decimal”. The octopodes use
“base 8”, also known as “octal”.

## Getting used to it

At this point---weirdly!---you probably have most of what you need to
understand different bases. The rest is working through the
implications, getting used to it, figuring out _why_ and _when_ using
different bases is useful, and learning how to write numbers in
different bases in your code.

# Different bases and computers

## Base 2: “Binary”

While it's possible to create an
“[analog](https://www.computerhistory.org/brochures/analog-computers/)”
computer that uses different voltages corresponding smoothly to
different quantities, and there's a rich history of [mechanical
computers](https://en.wikipedia.org/wiki/Mechanical_computer#Examples),
most modern computers are electrical, and “digital”: inside, they use
only **`on`** and **`off`**, like a light switch. Using **`1`** for
“on”, and **`0`** for “off”.

Base 2 is like counting if you're a ... bird? using only your wings?
... and have only two limbs to count on. It works just like the other
bases we've played with, except you're “carrying the 1” a **lot**!

```pikchr
box fill 0x333333 color 0xEEEEEE fit "0" bold fit
box same "0" bold; box same "0" bold; box same "0" bold; box same "0" bold
box same "0" bold
arrow from 0.02 s of 3rd last box.sw then 0.16 s thin; right

box at 0.3 s of 6th last box.s fill 0x333333 color 0xEEEEEE fit "0" bold fit
box same "0" bold; box same "0" bold; box same "0" bold; box same "0" bold
box same "0" bold above "1" bold below
arrow from 0.02 s of 3rd last box.sw then 0.16 s thin; right

box at 0.3 s of 6th last box.s fill 0x333333 color 0xEEEEEE fit "0" bold fit
box same "0" bold; box same "0" bold; box same "0" bold; box same "0" bold
box same "1" bold
arrow from 0.02 s of 3rd last box.sw then 0.16 s thin; right

box at 0.3 s of 6th last box.s fill 0x333333 color 0xEEEEEE fit "0" bold fit
box same "0" bold; box same "0" bold; box same "0" bold
box same "0" bold above "1" bold below
box same "1" bold above "0" bold below
arrow from 0.02 s of 3rd last box.sw then 0.16 s thin; right

box at 0.3 s of 6th last box.s fill 0x333333 color 0xEEEEEE fit "0" bold fit
box same "0" bold; box same "0" bold; box same "0" bold
box same "1" bold
box same "0" bold
arrow from 0.02 s of 3rd last box.sw then 0.16 s thin; right

box at 0.3 s of 6th last box.s fill 0x333333 color 0xEEEEEE fit "0" bold fit
box same "0" bold; box same "0" bold; box same "0" bold
box same "1" bold
box same "0" bold above "1" bold below
arrow from 0.02 s of 3rd last box.sw then 0.16 s thin; right

box at 0.3 s of 6th last box.s fill 0x333333 color 0xEEEEEE fit "0" bold fit
box same "0" bold; box same "0" bold; box same "0" bold
box same "1" bold
box same "1" bold
arrow from 0.02 s of 3rd last box.sw then 0.16 s thin; right

box at 0.3 s of 6th last box.s fill 0x333333 color 0xEEEEEE fit "0" bold fit
box same "0" bold; box same "0" bold
box same "0" bold above "1" bold below
box same "1" bold above "0" bold below
box same "1" bold above "0" bold below
arrow from 0.02 s of 3rd last box.sw then 0.16 s thin; right

box at 0.3 s of 6th last box.s fill 0x333333 color 0xEEEEEE fit "0" bold fit
box same "0" bold; box same "0" bold
box same "1" bold
box same "0" bold
box same "0" bold
```

Instead of a “ones digit”, “tens digit”, “hundreds digit”, etc., we
have a “ones digit”, a “twos digit”, a “fours digit”, an “eights
digit”, a “sixteens digit”, etc. Just like with base 10, each time you
move one digit to the left, you multiply by the base. _(Our gangly
molluscan friends would have a “ones digit”, an “eights digit”, a
“sixty-fours digit”, etc.)_

If we have four switches, we can represent `0101` (`5` in decimal)
using “·” for off and “✓” for on like this:

```pikchr
box "·" fit fill lightblue thin
box same "✓" thick thick
box same "·" thin thin
box same "✓" thick thick

text at 1st box .s "8" below italic small
text at 2nd box .s "4" below italic small
text at 3rd box .s "2" below italic small
text at 4th box .s "1" below italic small

text at 1st box .n "0" above
text at 2nd box .n "1" above bold
text at 3rd box .n "0" above
text at 4th box .n "1" above bold
```

We can then use wires to carry these signals around, like to a [Seven
Segment
Display](https://www.jameco.com/Jameco/workshop/TechTip/working-with-seven-segment-displays.html):

```pikchr
box "·" fit fill lightblue thin
box same "✓" thick thick
box same "·" thin thin
box same "✓" thick thick

$s = 0.06
$b = 0.25
$g = 0.05
$on = red
$off = 0x444444

[
line up $s left $s then up $b then up $s right $s then down $s right $s then down $b close fill $on color $on
line from $g ne of last.n up $s right $s then right $b then down $s right $s then down $s left $s then left $b close fill $on color $on
line from $g se of last.e down $s left $s then down $b then down $s right $s then up $s right $s then up $b close fill $off color $off
line from 1.4*$g s of last.s down $s left $s then down $b then down $s right $s then up $s right $s then up $b close fill $on color $on
line from $g sw of last.s up $s left $s then left $b then down $s left $s then down $s right $s then right $b close fill $on color $on
line from $g nw of last.w up $s left $s then up $b then up $s right $s then down $s right $s then down $b close fill $off color $off
line from $g ne of last.n up $s right $s then right $b then down $s right $s then down $s left $s then left $b close fill $on color $on
] at (3,1)

box at last.c behind last width last.width + 0.2 height last.height + 0.2 fill black
box at last.c width last.width + 0.1 height last.height + 0.1 thick

text at 1st box .s "8" below italic small
text at 2nd box .s "4" below italic small
text at 3rd box .s "2" below italic small
text at 4th box .s "1" below italic small

line from 1st box.n up until even with 0.12 n of last box.c then right until even with last box.w thin color grey
line from 2nd box.n up until even with 0.04 n of last box.c then right until even with last box.w thick color darkred
line from 3rd box.n up until even with 0.04 s of last box.c then right until even with last box.w thin color grey
line from 4th box.n up until even with 0.12 s of last box.c then right until even with last box.w thick color darkred
```

You can play around with a circuit simulation of a seven segment
display I found on everycircuit.com
[here](https://everycircuit.com/circuit/6711990626287616). _Note that
in the diagram above, I left out the “decoder” you need to convert
from 4-wire (4-bit) `0–9` binary data to 7-wire segment on/off data._

### “Bits”

We call each **bi**nary **i**nformation dig**it** a “bit”: **`0`** or
**`1`**. A bit is also the most basic “piece” or “unit” of information
you can have: true/false, yes/no, +/-, on/off, _(heads/tails,
left/right, up/down, ...)_

In many programming languages, you can use binary numbers directly in
your programs by using a prefix of `0b`:

```ruby
# A `0b` prefix indicates a binary number:
a = 0b101      # 5: 1×1 + 0×2 + 1×4
b = 0b11111111 # 255: 1×1 + 1×2 + 1×4 + 1×8 + 1×16 + 1×32 + 1×64 + 1×128

# In many languages, you can even break the digis into
# groups for readability. Here we use groups of 4 bits:
c = 0b1000_0000 # 128
```

## Base 16: “Hexadecimal”

It's very common to use base 16, or “hexadecimal”, numbers in computer
programs. To expand our repertoire to 16 digits, we use the usual
`0–9` and then add on `A–F`: `0, 1, 2, 3, 4, 5, 6, 7, 8, 9, A, B, C,
D, E, F`.

Other than that, things work the same as before; the odometer wheels
just have 16 digits:

```pikchr
box fill 0x333333 color 0xEEEEEE fit "0" bold fit
box same "0" bold; box same "0" bold; box same "0" bold; box same "0" bold
box same "0" bold
arrow from 0.02 s of 3rd last box.sw then 0.16 s thin; right

box at 0.3 s of 6th last box.s fill 0x333333 color 0xEEEEEE fit "0" bold fit
box same "0" bold; box same "0" bold; box same "0" bold; box same "0" bold
box same "0" bold above "1" bold below
arrow from 0.02 s of 3rd last box.sw then 0.16 s thin; right

box at 0.3 s of 6th last box.s fill 0x333333 color 0xEEEEEE fit "0" bold fit
box same "0" bold; box same "0" bold; box same "0" bold; box same "0" bold
box same "1" bold
arrow from 0.06 s of 3rd last box.sw then 0.38 s thin dashed; right

box at 0.6 s of 6th last box.s fill 0x333333 color 0xEEEEEE fit "0" bold fit
box same "0" bold; box same "0" bold; box same "0" bold; box same "0" bold
box same "9" bold
arrow from 0.02 s of 3rd last box.sw then 0.16 s thin; right

box at 0.3 s of 6th last box.s fill 0x333333 color 0xEEEEEE fit "0" bold fit
box same "0" bold; box same "0" bold; box same "0" bold
box same "0" bold
box same "9" bold above "A" bold below
arrow from 0.02 s of 3rd last box.sw then 0.16 s thin; right

box at 0.3 s of 6th last box.s fill 0x333333 color 0xEEEEEE fit "0" bold fit
box same "0" bold; box same "0" bold; box same "0" bold
box same "0" bold
box same "A" bold
arrow from 0.06 s of 3rd last box.sw then 0.38 s thin dashed; right

box at 0.6 s of 6th last box.s fill 0x333333 color 0xEEEEEE fit "0" bold fit
box same "0" bold; box same "0" bold; box same "0" bold; box same "0" bold
box same "F" bold
arrow from 0.02 s of 3rd last box.sw then 0.16 s thin; right

box at 0.3 s of 6th last box.s fill 0x333333 color 0xEEEEEE fit "0" bold fit
box same "0" bold; box same "0" bold; box same "0" bold
box same "0" bold above "1" bold below
box same "F" bold above "0" bold below
arrow from 0.02 s of 3rd last box.sw then 0.16 s thin; right

box at 0.3 s of 6th last box.s fill 0x333333 color 0xEEEEEE fit "0" bold fit
box same "0" bold; box same "0" bold; box same "0" bold
box same "1" bold
box same "0" bold
```

In most programming languages, you can use hexadecimal numbers
directly in your programs by using a prefix of `0x`:

```ruby
# A `0x` prefix indicates a hexadecimal number:
a = 0x0101  # 257: 1×1 + 0×16 + 1×256 + 0×4096
b = 0xFF    # 255: 15×1 + 15×16
```

### Why hexadecimal?

We mentioned [last set](../1b/#computer-memory) that a byte can hold
numbers from 0--255. That's because a byte is made up of 8
bits. _(This is where the
“[8-bit](https://www.hbomax.com/8-bit-christmas)” moniker comes from
with the 8-bit computers of the 1980's: Apple ][, Commodore 64, ZX
Spectrum, Atari 2600, Nintendo Entertainment System, BBC Micro,
etc. They used computer chips that worked with one byte---8 bits---of
information at a time.)_

Well, writing out full binary numbers --- like `0x10111001`--- takes
up a lot of space, and is hard to read. It happens that one
hexadecimal digit holds exactly four bits, and so any byte value can
be represented with two hex digits:

```pikchr
box "1" fit fill lightblue
box same "0"
box same "1"
box same "1"
box same "1"
box same "0"
box same "0"
box same "1"

text at 1st box .s "128" below italic small
text at 2nd box .s "64" below italic small
text at 3rd box .s "32" below italic small
text at 4th box .s "16" below italic small
text at 5th box .s "8" below italic small
text at 6th box .s "4" below italic small
text at 7th box .s "2" below italic small
text at 8th box .s "1" below italic small

box same at (-1st box.width / 2, -0.7) "1"
box same "0"
box same "1"
box same "1"
move last box.width
box same "1"
box same "0"
box same "0"
box same "1"

arrow from 6th last text.s to 0.07 n of 6th last box.nw
arrow from 3rd last text.s to 0.07 n of 3rd last box.ne

text at 8th last box .s "8" below italic small
text at 7th last box .s "4" below italic small
text at 6th last box .s "2" below italic small
text at 5th last box .s "1" below italic small
text at 4th last box .s "8" below italic small
text at 3rd last box .s "4" below italic small
text at 2nd last box .s "2" below italic small
text at 1st last box .s "1" below italic small

text at 0.3 s of 6th last box.sw "11: 0xB" big big
text at 0.3 s of 2nd last box.sw "9: 0x9" big big

box same as 1st box at (4th box.x, last text.s.y - 0.5) "B"
box same "9"

arrow from 2nd last text.s to 0.07 n of 2nd last box.nw
arrow from last text.s to 0.07 n of last box.ne
```

So `0b10111001 == 0xB9 == 185`. But it's a _whole_ lot easer to
convert `B` (`1011`) and `9` (`1001`) to `10111001` than it is to
convert `185`. Each group of 4 bits[^1] corresponds to one hex digit.

[^1]: A group of four bits is also called a
    “[nibble](https://en.wikipedia.org/wiki/Nibble)” --- half a “bite”!

Now we understand the hexadecimal part of the output of `man
ascii`. If you look up “#”, you'll see that it's 23 in hexadecimal (we
can also write 23<sub>16</sub>). If you type a “#” into a web form,
you'll often see it show up as `%23` in the URL, as in
[google.com/search?q=%23](https://google.com/search?q=%23). (You can
say the octothorpe is “percent encoded” as `%23`.)

_We'll get deeper into binary and working with bits in the [next part](../2c/)._
