---
layout: page
title: 'Projects'
---

A list of side projects, in no particular order.

## gopikchr

![pikchr example code and drawing](/img/projects/pikchr.png)

[gopikchr](https://github.com/gopikchr/gopikchr) is a direct port of
[pikchr](https://pikchr.org/) to Go. It's used to generate the
diagrams on my blog
([simpler example](https://zellyn.com/2022/02/cs-ramble/2c/),
[completely silly example](https://zellyn.com/video/gears/worm/#tooth-profile)).

_I'm working on a Javascript port with the hopes of getting pikchr
support onto Github by way of [Mermaid](https://mermaid.js.org)._

## goapple2 / go6502

![goapple2 image](/img/projects/goapple2.png)

[goapple2](https://github.com/zellyn/goapple2) is a simple Apple II
emulator written in Go. The [6502
emulation](https://github.com/zellyn/go6502) is cycle-accurate, and
includes a Go port of the [Visual 6502](http://www.visual6502.org)
transistor-level simulation.

Also includes a multi-dialect assembler capable of assembling most of
the source listings of the Apple II ROMs as well as the [S-C
Documentor](https://www.txbobsc.com/scsc/scdocumentor/) recreation of
Applesoft BASIC.

## weblight (in-progress)

![esp32 with wires and LEDs on a breadboard](/img/projects/weblight.jpg)

[weblight](https://github.com/zellyn/weblight) is an in-process
project to drive LEDs with an ESP32-C3, while documenting the process
and choices enough for it to serve as an introduction to ESP32
programming and working with LEDs.

## bedrock pruner (in-progress)

![overhead view of editing a mincraft map](/img/projects/bedrockprune.jpg)

[bedrockprune](https://github.com/zellyn/bedrockprune) is an
in-process project to view and prune Bedrock minecraft world
databases, written in Go.

## a2audit & diskii

![Seagull Sisters](/img/projects/seagull-srs.png)

[a2audit](https://github.com/zellyn/a2audit) is a test for Apple II
emulator correctness, focusing (for now) on memory expansion
cards/schemes. Most people who build an Apple II emulator end up using
it at least a little bit.

[diskii](https://github.com/zellyn/diskii) is a utility for
manipulating Apple II disk images.

## apple2shader

![Apple II shader](/img/projects/apple2shader.png)

[apple2shader](https://zellyn.com/apple2shader/) is a WebGL port of
Marc S. Ressl's amazing NTSC emulation from
[OpenEmulator](https://openemulator.github.io).

## gocool

![cool homepage image](/img/projects/cool.png)

[gocoool](https://github.com/zellyn/gocool) is a Go language
implementation of the [Cool programming
language](https://theory.stanford.edu/~aiken/software/cool/cool.html)
done for the Coursera . Includes a hand-written recursive-descent
parser in addition to a standard yacc-based one.

---

For potential projects and improvements, see [Project TODOs](../todo).
