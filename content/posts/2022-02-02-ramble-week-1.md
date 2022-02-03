---
title: "A ramble around CS — intro/week 1"
slug: "a-ramble-around-cs-intro-week-1"
date: 2022-02-02T19:58:00-05:00
tags: ['ramble']
draft: true
---

# Intro

It turns out we have several folks at Square who arrived at
programming through boot-camps, or other non-CS paths, and feel a sort
of lingering insecurity (or get explicitly told by unkind people) that
their path left them without a solid grasp of “the fundamentals”.

I have the pleasure of having a boot-camp grad on our team: she's now
several tech jobs in (and fantastically capable), and has a really
great perspective on what our industry can be like to people coming in
from outside.

We decided to try an experiment for folks in our company interested in
learning CS: I'll play the grizzled veteran, and come up with
provocative questions, and we'll meet for an hour every two weeks and
discuss them. I'm excited to see how it goes!

# Questions for week 1

<br/>
<table style="border:1px solid lightgrey;"><td>

- When you say `a = "Hello, world"` in your favorite language, how is
  the string stored? How about `"Hello, 世界"`?
- When you say `a = "Hello"` and `b = " world"` and then do `c = a +
  b`, what happens? How are `a`, `b`, and `c` stored?
- When you say `a = 42`, and then `b = a`, and then `b = 17`, what are
  `a` and `b`? Why?
- When you say `a = {"foo": 42}`, and then `b = a`, and then `b.foo =
  17`, what are `a` and `b`? Why?
- Why do computer people use “foo” and “bar” all the time? What’s up
  with that? What about 42 and 17?

</table>

# Computer memory

You can imagine all your computer's memory as a series of little boxes,
numbered from 0 (because we're computer people), and going up, and up,
and up. The laptop I'm typing this on has 32MB of memory, or
34,359,738,368 little boxes!


```pikchr {toggle=true}
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

If we want to store letters in the boxes, we have to come up with some
kind of “[Character
Encoding](https://en.wikipedia.org/wiki/Character_encoding)”,
assigning a number to each letter.

_I'd use 1 for “A”, 2 for “B”, ..., except it's probably better to
just jump straight to the actual numbers your computer (usually) uses
(called “[ASCII](https://en.wikipedia.org/wiki/ASCII)”) so we get used
to seeing the real numbers:_

```pikchr {toggle=true}
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

$space = linewid/20
text at 1st box.n + (0,$space) "0" above
text at 2nd box.n + (0,$space) "1" above
text at 3rd box.n + (0,$space) "2" above
text at 4th box.n + (0,$space) "3" above
text at 5th box.n + (0,$space) "4" above
text at 6th box.n + (0,$space) "5" above
text at 7th box.n + (0,$space) "6" above
text at 8th box.n + (0,$space) "7" above
text at 9th box.n + (0,$space) "8" above
text at 10th box.n + (0,$space) "9" above
text at 11th box.n + (0,$space) "10" above
text at 12th box.n + (0,$space) "11" above
```
