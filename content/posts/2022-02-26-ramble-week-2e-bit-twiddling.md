---
title: "CS Ramble — Week 2e - bit twiddling"
slug: "cs-ramble/2e"
date: 2022-02-26T20:21:00-05:00
tags: ['ramble', 'week2']
---

_This is post is part of [week 2](../2a/) of [A Ramble Around
CS](../)._

In this post, we're going to look at how to get, set, and otherwise
manipulate individual bits.

> Bit twiddling and bit bashing are often used interchangeably with
> bit manipulation, but sometimes exclusively refer to clever or
> non-obvious ways or uses of bit manipulation, or tedious or
> challenging low-level device control data manipulation tasks.
>
> The term bit twiddling dates from early computing hardware, where
> computer operators would make adjustments by tweaking or twiddling
> computer controls. As computer programming languages evolved,
> programmers adopted the term to mean any handling of data that
> involved bit-level computation.
>
> ---[Wikipedia: Bit manipulation](https://en.wikipedia.org/wiki/Bit_manipulation#Terminology)

## Masks

We often refer to clearing or setting all but a certain set of bits in
a number as “masking“. [This Stackoverflow
answer](https://stackoverflow.com/a/53722721/23582) gives a good
visual explanation of where the term comes from.

Let's say we want to check whether a number is even or odd. In
decimal, since the base itself is even, nothing can make the number
odd except the last digit, so we check whether the last digit is even
(0, 2, 4, 6, 8) or odd (1, 3, 5, 7, 9).

In binary, again, the base (2) is even, so only the last digit can
make a number odd. So we check whether the last digit is even (0) or
odd (1). Well, that's pretty simple!

So how do we check the last digit only? The answer is to throw away,
or “mask out” all the other digits. In the [last part](../2d/), we
showed that you can `and`, `or`, or `xor` whole numbers against each
other at once, and it performs a bitwise `and`, `or`, or `xor` on
corresponding pairs of bits. We can use that to clear out everything
but the last bit:

#### Even example:

| | &nbsp; | |
|--|--|--|
|**a** | | `01010110` |
| **mask** | | `00000001` |
| **a** _and_ **mask** | | `00000000` |

#### Odd example:

| | &nbsp; | |
|--|--|--|
|**a** | | `01010111` |
| **mask** | | `00000001` |
| **a** _and_ **mask** | | `00000001` |

You can see that when `and`ing a number with a mask, any digit that is
`0` in the mask will be `0` in the result, and any digit that is `1`
in the mask will be unchanged by the mask. So we can clear all the
digits except the lowest (0ᵗʰ) by `and`ing with `00000001`, or just
`1`.

So by checking whether `n&1 == 0`, we can tell if `n` is even! In
Ruby, that would look like this[^1]:

```ruby
# Ruby

def is_even(n)
  return n&1 == 0
end

puts is_even 0    # true
puts is_even 1    # false
puts is_even 2    # true
puts is_even 3    # false
puts is_even 99   # false
puts is_even 100  # true
```

[^1]: Actually, many programming languages have a convention that `0`
    is `false` and anything else is `true`, so in Python, you could
    write this:

	```python
	# Python

	def is_even(n):
	  return not n&1

	def is_odd(n):
	  return n&1
	```

    You could do more or less the same thing in C. In fact, `n&1` is so
    terse and---once you get used to it---clear, that you'd seldom create
    an `is_even` or `is_odd` function in any language.

### _Or_-ing

As `and` can be used to clear bits or leave them alone, `or` can be
used to _set_ bits or leave them alone. For instance, to set the 5ᵗʰ
bit from lowest (counting from 0, of course), we can `or` with
`0b100000` == `32` == `1 << 5`:

| | &nbsp; | |
|--|--|--|
|**a** | | `01001101` |
| **mask** | | `00100000` |
| **a** _or_ **mask** | | `01101101` |

In fact, in ASCII, the uppercase and lowercase English letters are
laid out so that they're exactly the same, except for the 5th bit!

```shell
$ python3
Python 3.9.10 (main, Jan 15 2022, 11:48:04)
Type "help", "copyright", "credits" or "license" for more information.
>>> chr(0b01001101)
'M'
>>> chr(0b01001101 | 1<<5)
'm'
>>>
```

As you can see in this partial excerpt from the ASCII table, the
uppercase and lowercase numbers differ by exactly `32` ==
`1<<5`. Neat, huh?

| | | | | | | | | | | | | | | | | | |
|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|
|**64:**| |@|A|B|C|D|E|F|G|H|I|J|K|L|<mark>**M**</mark>|N|O|
|**80:**| |P|Q|R|S|T|U|V|W|X|Y|Z|[|\ |]|^|_|
|**96:**| |`|a|b|c|d|e|f|g|h|i|j|k|l|<mark>**m**</mark>|n|o|
|**112:**| |p|q|r|s|t|u|v|w|x|y|z|{|&#124;|}|~|del|

### Masking more than one bit at a time: hex digits

As you may recall from the [“Why hexadecimal?” section of part
b](../2b/#why-hexadecimal), hexadecimal is often used because it maps
nicely to binary: one hex digit to four bits:

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

$width = 1st box.w.x - 4th box.e.x
$brace(4th box.se.x, 4th box.se.y-0.05, $width, -0.1)
text at last arc.start "B" below
$brace(last box.se.x, 4th box.se.y-0.05, $width, -0.1)
text at last arc.start "9" below
```

We can mask with `0b1111` to get just the low four bits, which is the
same as masking with `0xF` to get just the low hex digit. To get the
high digit, we can shift right, _then_ mask:

```javascript
// Javascript

console.log(0xB9 & 0xF)
// 9
console.log((0xB9 >> 4) & 0xF)
// 11

console.log(((0xB9 >> 4) & 0xF).toString(16).toUpperCase())
// B
```

&nbsp;

_I think that's enough for now, eh? Until next time!_

&nbsp;

&nbsp;
