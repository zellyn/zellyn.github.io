---
title: "CS Ramble — Week 2c - Binary and Bits"
slug: "cs-ramble/2c"
date: 2022-02-17T21:30:00-05:00
tags: ['ramble', 'week2']
---

_This is post is part of [week 2](../2a/) of [A Ramble Around
CS](../)._

In this post, we're going to become more familiar with thinking in
binary, and messing around with individual bits: “bit twiddling”.

## Basics

Let's use our example byte from last time to show the anatomy of a byte: `0b10111001 == 0xB9 == 185`


```pikchr
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

text at 0.3 n of 1st box.n "“high bit”" italic
arrow from last text.s to first box.n

text at 0.3 n of last box.n "“low bit”" italic
arrow from last text.s to last box.n

$width = 1st box.w.x - 4th box.e.x
$brace(4th box.se.x, 4th box.se.y-0.15, $width, -0.1)
text at last arc.start "B" below "“high nibble”    " below italic
$brace(last box.se.x, 4th box.se.y-0.15, $width, -0.1)
text at last arc.start "9" below "    “low nibble”" below italic
```

> **Note:** the work “nibble” is less common these days than it used
> to be, when computers had much less memory, and making maximum use
> of every little bit was more important. Don't be surprised if people
> haven't heard of it: you can name-drop “high nibble” to increase
> your nerd cred!

## Bit shifting

In base 10, if we shift digits to the left or right, we're multiplying
or dividing by 10:

```pikchr
box "1" fit width 0.2 fill lightblue
box same "8"
box same "5"

box same at 1st box + (-1st box.width, -0.7) "1"
box same "8"
box same "5"
box same "0"

line from 1st box.sw to 4th box.nw dashed thin
line same from 2nd box.sw to 5th box.nw
line same from 3rd box.sw to 6th box.nw
line same from 3rd box.se to 7th box.nw

text at ((2nd box.x + 5th box.x)/2, (2nd box.s.y + 5th box.n.y)/2) "shift ←"
box at last text width last text.width height last text.height color white fill white
text at last "shift ←" italic

RIGHT: box same as 1st box at 1.4 s of 1st box "1"
box same "8"
box same "5"

box same at RIGHT + (1st box.width, -0.7) "1"
box same "8"

line from 5th last box.sw to 2nd last box.nw dashed thin
line same from 4th last box.sw to last box.nw
line same from 3rd last box.sw to last box.ne

text at ((4th last box.w.x + last box.w.x)/2, (4th last box.s.y + last box.n.y)/2) "shift →"
box at last text width last text.width height last text.height color white fill white
text at last "shift →" italic

box with nw at 3rd box.ne + (0.02,0.1) height (1st box.n.y - 3rd last box.s.y + 0.2) \
  width 0.02 color 0xEEEEEE fill 0xEEEEEE
```

In base 2, if we shift digits to the left or right, we're multiplying
or dividing by 2:

```pikchr
box "1" fit width 0.2 fill lightblue
	box same "1"
box same "1"

box same at 1st box + (-1st box.width, -0.7) "1"
box same "1"
box same "1"
box same "0"

line from 1st box.sw to 4th box.nw dashed thin
line same from 2nd box.sw to 5th box.nw
line same from 3rd box.sw to 6th box.nw
line same from 3rd box.se to 7th box.nw

text at ((2nd box.x + 5th box.x)/2, (2nd box.s.y + 5th box.n.y)/2) "shift ←"
box at last text width last text.width height last text.height color white fill white
text at last "shift ←" italic

RIGHT: box same as 1st box at 1.4 s of 1st box "1"
box same "1"
box same "1"

box same at RIGHT + (1st box.width, -0.7) "1"
box same "1"

line from 5th last box.sw to 2nd last box.nw dashed thin
line same from 4th last box.sw to last box.nw
line same from 3rd last box.sw to last box.ne

text at ((4th last box.w.x + last box.w.x)/2, (4th last box.s.y + last box.n.y)/2) "shift →"
box at last text width last text.width height last text.height color white fill white
text at last "shift →" italic

box with nw at 3rd box.ne + (0.02,0.1) height (1st box.n.y - 3rd last box.s.y + 0.2) \
  width 0.02 color 0xEEEEEE fill 0xEEEEEE
```

In code, we use `<<` to shift left, and `>>` to shift right:

```python
# python:
print(bin(0b111 << 1))  # 0b1110
print(bin(0b111 >> 1))  # 0b11
```

We can shift by a lot if we have the space for it. In python, there's
no real limit on how big numbers can be, so we can shift as far as we
like:

```python
print(bin(1 << 10))     # 0b10000000000
print(1 << 10)          # 1024
print(1<<130)           # 1361129467683753853853498429727072845824
```

In a language like Go, where the bit widths are fixed (because numbers
occupy a specific number of bytes), we can shift right off the end!

```go
var a uint8 = 1
fmt.Println(a<<7)  # 128
fmt.Println(a<<8)  # 0
```
_(Longer example with several different widths you can play around with [here](https://go.dev/play/p/UF31Z4jEBig).)_

As you may have noticed, you can (within the bounds of the bit-width
of your variable), replace `2ⁿ ` with `1 << n`, and you can likewise
replace `x × 2ⁿ` with `x << n`. You can also replace `⌊ˣ⁄₂⌋` with
`x >> 1` and `⌊x/2ⁿ⌋` with `x >> n`[^1]. In fact, in compiled languages,
your compiler will make these substitutions in the compiled code
whenever it can, because bit-shifting is faster than multiplication.

[^1]: If you're a bit rusty, `⌊a⌋` means the next integer (whole
    number) equal to or less than `a`. It's called the “floor” of `a`:
    `⌊2⌋` ＝ `2`, `⌊1.9⌋` ＝ `1`, and `⌊−1.9⌋` ＝ `−2`. Similarly,
    `⌈a⌉` is the “ceiling” of `a`: `⌈2⌉` ＝ `2`, `⌈1.9⌉` ＝ `2`, and
    `⌈−1.9⌉` ＝ `−1`.

Although that may take a few minutes to sink in, you're already
familiar with it in good old base 10: how do you multiply by `1000`?
Just shift left three places (add three zeros). Since you multiply by
`10` each time you shift left once, you can multiply by `10³` by
shifting left `3` places.

### Fractions

We're going to get into it deeply, but you can of course write
fractions in other bases. In base `10`, we divide by ten each time we
move to the right, so the digit to the right of the “decimal” point is
of course the ⅒ column. To represent ½, we use ⁵⁄₁₀, and write `0.5`.

In octal, the column to the right of the point is the ⅛ column, so
we'd use ⁴⁄₈, and write `0.4₈`.

In binary, it's the ½ column already, so we'd use ½, and write
`0.1₂`. ¼ would be `0.01₂`, and ¾ would be `0.11₂`. Weird, huh? But it
all makes sense:

```pikchr
box "0" big fit fill lightblue
box same "0" big
box same "0" big
box same "0" big
box same "." big big bold
box same "1" big
box same "1" big
box same "0" big
box same "0" big

text at 1st box .s "8" below italic
text at 2nd box .s "4" below italic
text at 3rd box .s "2" below italic
text at 4th box .s "1" below italic
text at 6th box .s "½" below italic
text at 7th box .s "¼" below italic
text at 8th box .s "⅛" below italic
text at 9th box .s "1⁄16" below italic

text at 0.5 s of 5th box "¾ ＝ ½ + ¼" big
```

_We'll explore how to work with the values of individual bits in the
next part._
