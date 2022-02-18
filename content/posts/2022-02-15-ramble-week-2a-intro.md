---
title: "CS Ramble — Week 2a - questions"
slug: "cs-ramble/2a"
date: 2022-02-15T20:15:00-05:00
tags: ['ramble', 'week2']
---

_These are the questions for week 2 of [A Ramble Around CS](../)._

# Questions for week 2

- If humans had eight fingers, and only used the digits 0–7 to count,
  how would they write what we call “ten”? What would they mean when
  they wrote “100”? What would a “round number” mean to them? (To us, 10, 100, 1000,
  etc. are “round numbers”.)
- What if you only had the digits 0 and 1? How would you write what we
  call “ten”? What would it mean when you wrote “100”? What would a
  “round number” look like?
- What does `print(1<<8)` print in your favorite programming language?
  Why? What’s going on?
- What does `print(65 | 32)` print in your favorite programming
  language? Why? What’s going on? How about `print(90 | 32)`?
- What does it mean that ‘A’ is 65, and ‘a’ is 65|32, ‘Z’ is 90, and
  ‘z’ is 90|32?
  - Ruby: `('A'.ord | 32).chr` → `"a"`
  - Python: `chr(ord('a') | 32)` → `'a'`
  - Javascript: `String.fromCharCode('A'.charCodeAt(0) | 32)` → `"a"`
  - Hint: what kind of number is 32 in binary?
- What if humans had 16 fingers and had 16 different digits? (Let’s
  say, 0–9 for the first ten, and A–F for the next 6.) How would they
  write what we call “ten”? What would they mean when they wrote
  “100”?

_We'll start discussing numeric bases in the [next part](../2b/)._
