---
layout: page
title: 'Side Project TODOs'
---

```pikchr
box "Get new idea"
arrow
box "Start new" "project"
down
arrow
box "Tell" "everyone"
left
move
box "Finish" "project"
arrow from 3rd box.w go 0.25 w then 0.5 n then left until even with 1st box then to 1st box.s
```

<center><i><a href="https://twitter.com/jmspool/status/1496552435392860161">source</a></i></center>

# Potential side projects

I'm trying to create a structured list of next steps: hopefully
that'll make it easier to pick off tasks to work on, and (less likely)
attract interest from others.

For completed projects, see [the main Projects page](../done).

## 6502 Golf site

[comp.sys.apple2](https://groups.google.com/forum/#!forum/comp.sys.apple2)
folks sometimes like to collaborate/compete on golfing 6502 code. I
thought it would be fun to create a site for 6502 golfing.

Leaderboards would be fun, and being able to track the tree of ideas
(by giving credit or explicitly starting with code and modifying it)
would be instructive.

### Tasks

- [ ] Get my [go6502](https://github.com/zellyn/go6502) code working
      under GopherJS.
- [ ] Set up a basic GoBuffalo site.
- [ ] Figure out Google/Github/Twitter/etc. auth with GoBuffalo site.
- [ ] Fork and modify [goplay.space](https://goplay.space/) to get the
  editor.
- [ ] Split my
  [assembler](https://github.com/zellyn/go6502/tree/master/asm) code
  out of go6502: they're really two different projects: eg. mixing
  Sweet16 opcode definitions with those used for emulation is just a
  mess.
- [ ] Get assemblers hooked up to editor and emulator.
- [ ] Add saving of code, etc.
- [ ] Add challenges/problems with descriptions, solutions,
  leaderboards, etc.

## Retrocomputing technical docs conversion pipeline

- [ ] Classify/divide up page areas: Header, paragraphs, diagrams,
  etc.
- [ ] OCR on paragraphs
- [ ] turking / collaborative editing

## Apple II emulator improvements/tasks

### Update visual 6502

- [ ] Incorporate performance improvements from the [C
      version](https://github.com/mist64/perfect6502/commits/master)
      into [my Go
      port](https://github.com/zellyn/go6502/tree/master/visual).

### Get it running Pitch Dark in a web page

- [ ] Figure out how to compile to webassembly
- [ ] Build a shim to hook it up to my [apple2shader
      port](https://zellyn.github.io/apple2shader/).
- [ ] Keyboard input
- [ ] Implement 65c02 support
- [ ] Implement IIe RAM model
- [ ] Implement IIe Double-res graphics modes
- [ ] Implement fake hard-disk card for .2mg disk images
- [ ] IndexedDB state storage of some kind
