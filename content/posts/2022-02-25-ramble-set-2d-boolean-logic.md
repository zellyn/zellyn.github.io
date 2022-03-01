---
title: "CS Ramble — Set 2d - boolean logic"
slug: "cs-ramble/2d"
date: 2022-02-25T20:10:00-05:00
tags: ['ramble', 'set2']
---

_This is post is part of [set 2](../2a/) of [A Ramble Around
CS](../)._

In this post, we're going to become more familiar with messing around
with individual bits: “bit twiddling”.

## And, Or, Not, etc.

When dealing with bits---or with “boolean” values (`True` or
`False`)[^1]---we talk about value a **_and_** value b, value a
**_or_** value b, etc. They have more precise meanings than their
normal English usage:

[^1]: Named after [George Boole](https://www.irishphilosophy.com/2014/12/08/ones-and-zeros/)

In normal usage, you might say, “I'll wash the dishes if you take the
trash out **_and_** feed the dog.“ The first part (washing the dishes)
will be true if both (taking the trash out) **_and_** (feeding the
dog) are true. This actually corresponds pretty much exactly to
boolean values:

| a | b | &nbsp; | a _and_ b |
|---|---|--------|:---------:|
|False|False| |False|
|False|**True**| |False|
|**True**|False| |False|
|**True**|**True**| |**True**|

Using `1` to represent `True`, and `0` for `False`, we could also
write that like so:

| a | b | &nbsp; | a _and_ b |
|---|---|--------|:---------:|
|0|0| |0|
|0|**1**| |0|
|**1**|0| |0|
|**1**|**1**| |**1**|

In English, though, when we say, “I'm going to take the trash out
**_or_** feed the dog,” we usually mean we'll do one or the other, but
not both.[^2]

[^2]: Although, of course, if I said, “I'll wash the dishes if you
take the trash out **_or_** feed the dog,“ and you did both, I'd still
wash the dishes!

With boolean logic, “or” doesn't imply that both won't be true. Here's
the truth table:

| a | b | &nbsp; | a _or_ b |
|---|---|--------|:--------:|
|False|False| |False|
|False|**True**| |**True**|
|**True**|False| |**True**|
|**True**|**True**| |**True**|

And again with `0` and `1` (which we'll stick to from now on):

| a | b | &nbsp; | a _or_ b |
|---|---|--------|:--------:|
|0|0| |0|
|0|**1**| |**1**|
|**1**|0| |**1**|
|**1**|**1**| |**1**|

The word for “a or b, _but not both_” is called “exclusive or”
(“xor”), and means what it sounds like:

| a | b | &nbsp; | a _xor_ b |
|---|---|--------|:---------:|
|False|False| |False|
|False|**True**| |**True**|
|**True**|False| |**True**|
|**True**|**True**| |<mark>False</mark>|

There's one more basic boolean operation: **_not_**:

| a | &nbsp; | _not_ a |
|---|--------|:-------:|
| 0 | | **1** |
| **1** | | 0 |

### An aside: circuits and gates

In an electrical circuit, we call the components that perform these
operations “gates”, and use the following symbols:

```pikchr

AND: [
	right; line 0.4; arc
	up; arc
	left; line 0.4
	down; line 0.5
] at (0,0)
text at AND - (0.02,0) "AND" bold

line from AND.e right 0.25
text at last line.e " a and b" ljust
line from AND.w + (0,0.1) left 0.25
text at last line.w "a " rjust
line from AND.w - (0,0.1) left 0.25
text at last line.w "b " rjust

OR: [
	line from (0, 0.25) then right 0.4 then down 0.25 right 0.25 radius 0.25
	line from (0, -0.25) then right 0.4 then up 0.25 right 0.25 radius 0.25
	line from (0, 0.25) to (0.1,0.1) to (0.1, -0.1) to (0, -0.25) radius 0.25
] at (0,-1)
text at OR - (0.01,0)  "OR" bold

line from OR.e right 0.25
text at last line.e " a or b" ljust
line from OR.w + (0.09,0.1) left 0.34
text at last line.w "a " rjust
line from OR.w + (0.09,-0.1) left 0.34
text at last line.w "b " rjust

XOR: [
	line from (0, 0.25) then right 0.4 then down 0.25 right 0.25 radius 0.25
	line from (0, -0.25) then right 0.4 then up 0.25 right 0.25 radius 0.25
	line from (0, 0.25) to (0.1,0.1) to (0.1, -0.1) to (0, -0.25) radius 0.25
	line from (-0.07, 0.25) to (0.03,0.1) to (0.03, -0.1) to (-0.07, -0.25) radius 0.25
] at (0,-2)
text at XOR + (0.04,0) "XOR" bold

line from XOR.e right 0.25
text at last line.e " a xor b" ljust
line from XOR.w + (0.09,0.1) left 0.3
text at last line.w "a " rjust
line from XOR.w + (0.09,-0.1) left 0.3
text at last line.w "b " rjust

NOT: [
	line from (0, 0) then up 0.5 then right 0.5 down 0.25 close
	circle radius 0.05 with w at last line.e
] at (0,-3)
text at NOT - (0.105,0) "NOT" bold

line from NOT.e right 0.3
text at last line.e " a not b" ljust
line from NOT.w + (0,0.1) left 0.27
text at last line.w "a " rjust
line from NOT.w + (0,-0.1) left 0.27
text at last line.w "b " rjust
```

A guick search yielded a [decent YouTube
video](https://youtu.be/kbXwFWMl6WA) on simple gates. To see a more
complex example of how to combine several gates to do something
useful, go to [this site](https://sciencedemos.org.uk/logic_gates.php)
and choose the “4-bit addition” preset.

## “ANDing” and “ORing” bits

Just as we can `and` together two bits, we can `and` together two
numbers each made up of many bits: we just perform the `and` bit by
bit:

| | | |
|:-:|-|-|
| **a** | |`0 1 0 1 0 1 1 0` |
| **b** | |`1 0 1 1 0 0 1 0` |
| **a _and_ b** | |`0 0 0 1 0 0 1 0 ` |

Same with `or:`

| | | |
|:-:|-|-|
| **a** | |`0 1 0 1 0 1 1 0` |
| **b** | |`1 0 1 1 0 0 1 0` |
| **a _or_ b** | |`1 1 1 1 0 1 1 0` |

`xor:`

| | | |
|:-:|-|-|
| **a** | |`0 1 0 1 0 1 1 0` |
| **b** | |`1 0 1 1 0 0 1 0` |
| **a _or_ b** | |`1 1 1 0 0 1 0 0` |

`not:`

| | | |
|:-:|-|-|
| **a** | |`0 1 0 1 0 1 1 0` |
| **_not_ a** | |`1 0 1 0 1 0 0 1` |

In Go code, these operations would use `&` for `and`, `|` for `or`,
`^` for `xor`, and a prefixed `^` for `not` (most languages use a
prefixed `~`):

```go
var a uint8 = 0b01010110
var b uint8 = 0b10110010

fmt.Printf("a:       %08b\n", a)
fmt.Printf("b:       %08b\n", b)
fmt.Printf("a and b: %08b\n", a&b)
fmt.Printf("a or b:  %08b\n", a|b)
fmt.Printf("a xor b: %08b\n", a^b)
fmt.Printf("not a:   %08b\n", ^a)

/*
a:       01010110
b:       10110010
a and b: 00010010
a or b:  11110110
a xor b: 11100100
not a:   10101001
*/
```
<div align="center"><i>(playground version <a href="https://go.dev/play/p/9IbjCDynXWf">here</a>)</i></div>

In Python, it would look very similar:

```python
a = 0b01010110
b = 0b10110010

print(f'a:       {a:08b}')
# a:       01010110

print(f'b:       {a:08b}')
# b:       01010110

print(f'a and b: {a&b:08b}')
# a and b: 00010010

print(f'a or b:  {a|b:08b}')
# a or b:  11110110

print(f'a xor b: {a^b:08b}')
# a xor b: 11100100

# For not, we have to force the result back into the range [0,255].
# (If the highest bit in a number is set, the number is
# interpreted as a negative number.)
print(f'not a:   {~a % 256:08b}')
# not a:   10101001
```

_In the [next part](../2e/), we'll learn how to read and change the
values of individual bits._
