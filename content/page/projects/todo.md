---
layout: page
title: 'Projects: TODO'
---

This is a list of side projects I would _like_ to do.

I'm trying to create a structured list of next steps: hopefully
that'll make it easier to pick off tasks to work on, and (less likely)
attract interest from others.

# 6502 Golf site

[comp.sys.apple2](https://groups.google.com/forum/#!forum/comp.sys.apple2)
folks sometimes like to collaborate/compete on golfing 6502 code. I
thought it would be fun to create a site for 6502 golfing.

Leaderboards would be fun, and being able to track the tree of ideas
(by giving credit or explicitly starting with code and modifying it)
would be instructive.

## Tasks

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

# OpenEmulator Tasks

## Make IIe aux card pluggable

- [ ] Route an3/frctxt through card
- [ ] Figure out how to make AppleIIeVideo not need direct access to VRAM

## Veronica emulation

- [ ] First, go read all the blog posts :-)

## Implement Woz disk format

- [ ] Make it work at 1 cycle (or is it 4?) per bit
- [ ] Convert .woz to angular positions and back again for real spinny-ness

## Implement Mockingboard support

- [ ] Copy Mame code, I guess

## Linux UI, Windows UI

- [ ] Figure out which would get more users
- [ ] Port the basics
- [ ] Port audio
- [ ] Port shaders/OpenGL stuff

# Retrocomputing technical docs conversion pipeline

- [ ] Classify/divide up page areas: Header, paragraphs, diagrams,
  etc.
- [ ] OCR on paragraphs
- [ ] turking / collaborative editing

# Native Go game controller library

- [ ] Figure out where [Chromium MacOS XBox
  code](https://cs.chromium.org/chromium/src/device/gamepad/?q=gamepad&sq=package:chromium&dr)
  gets called from
- [ ] Get proof-of-concept-level cgo code working to enumerate and/or
  find controller
- [ ] Figure out how to register Objective-C callbacks (for connect/disconnect) that
  have the correct lifecycle/memory management, and call back into Go safely.
- [ ] Organize a community effort to port all the Chrome code to Go on all the OSes :-)

# LambdaMoo Go port

- [ ] Write some actual tests
- [ ] Figure out what to do for regexes: they have backrefs :-(
- [ ] Port the code
- [ ] Port WAIF patches
- [ ] Port unicode patches and/or figure out if there are any
  downsides from simply supporting utf8
- [ ] Port the 64-bit ints
- [ ] Figure out how to add more concurrency using goroutines
- [ ] Use something like sqlite?

# Apple II emulator improvements/tasks

## Update visual 6502

- [ ] Incorporate performance improvements from the [C
      version](https://github.com/mist64/perfect6502/commits/master)
      into [my Go
      port](https://github.com/zellyn/go6502/tree/master/visual).

## Get it running Pitch Dark in a web page

- [ ] Figure out how to compile to webassembly
- [ ] Build a shim to hook it up to my [apple2shader
      port](https://zellyn.github.io/apple2shader/).
- [ ] Keyboard input
- [ ] Implement 65c02 support
- [ ] Implement IIe RAM model
- [ ] Implement IIe Double-res graphics modes
- [ ] Implement fake hard-disk card for .2mg disk images
- [ ] IndexedDB state storage of some kind
