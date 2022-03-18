---
title: "Accurate timing on a Raspberry Pi"
slug: "raspberry-pi-accurate-timing"
date: 2022-02-21T14:30:00-05:00
tags: ['rpi']
draft: true
---

{{< toc >}}

## The problem

I've been trying to open my garage door and gate using a [Raspberry Pi
Zero W](https://www.raspberrypi.com/products/raspberry-pi-zero-w/) and
a [315MHz FS1000A](https://www.google.com/search?q=315mhz+fs1000a)
transmitter. At first, I tried directly toggline the GPIO (“General
Purpose Input-Output”) pins on and off in code.

Unfortunately, the timing in Linux just isn't that accurate. I ended
up using several tricks:

* `sudo chrt -f -r 99` to run the command with as much [realtime
  priority](https://man7.org/linux/man-pages/man1/chrt.1.html) as
  possible
* Busy-waiting rather than sleeping (which was _horribly_ imprecise):
  ```go
  for target.After(time.Now()) {}
  ```
* Setting target time based on offsets from the _original_ start time,
  rather than the current time, so that errors don't accumulate

Even so, it's still not accurate enough. Here's a pretty good run,
which still includes an obvious lag in turning off the signal at one
point:

![timing image with lag pointed to by a pink arrow](/img/2022/capture-with-lag.png)
_Image captured with
[`rtl_sdr`](https://osmocom.org/projects/rtl-sdr/wiki/Rtl-sdr#Usage)
and displayed using [`inspectrum`](https://github.com/miek/inspectrum)_

## Quick disclaimer and request for correction

I am _not_ a Raspberry Pi expert. I found the information and
understanding I needed was scattered far and wide, across different
forum questions and responses, datasheets, web pages, software
documentation, and even source code. The answers also changed over the
years. This post is as much an effort to organize my own thoughts and
provide a reference for _me_ to come back to later as anything. It's
also an effort to provoke correction: if you notice errors or
omissions, please let me know, and I'll make updates.

## Sources of information

The first Raspberry Pis (and the curretn Zero and Zero W) used the
Broadcom 2835 System-On-a-Chip (SOC), and the [BCM2835 Peripherals
instruction
manual](https://www.raspberrypi.org/app/uploads/2012/02/BCM2835-ARM-Peripherals.pdf)
is very useful for low-level understanding. (The later models seem to
have kept compatibility with the 2835, so the information remains
useful.)

[Chris Hager](https://www.metachris.com/about/)'s Python `RPIO.PWM`
module [documentation](https://pythonhosted.org/RPIO/pwm_py.html) and
[source
code](https://github.com/metachris/RPIO/blob/master/source/c_pwm/pwm.c)
are useful because the C source code is clear and well-documented.

[Jeremy Garff](https://github.com/jgarff)'s
[rpi_ws281x](https://github.com/jgarff/rpi_ws281x) project for
controlling [WS281X LEDs](https://learn.watterott.com/kb/ws281x/) is a
great resource because it can use all three of DMA, PWM, and SPI,
because it is pushing out custom data streams of bits, because the
main body of code in
[ws2811.c](https://github.com/jgarff/rpi_ws281x/blob/master/ws2811.c)
is clear and well-commented, and because the README discusses the
trade-offs and limitations of the three approaches.

## Viable approaches

As mentioned above, there are three techniques that can be used for
accurate timing on the Raspberry Pi, each with pros and cons:

### DMA

DMA, or “[Direct Memory
Access](https://en.wikipedia.org/wiki/Direct_memory_access)”, is a
technique where you set up a series of instructions in memory, then
activate the DMA controller, and it runs more or less on its own,
directly accessing memory to retrieve the instructions. A typical use
of DMA is to transfer chunks of data between regions of memory, or to
video, networking, or audio devices.

On the BCM2835, DMA instructions can also be used to turn GPIO pins on
or off. By setting up just the right set of instructions, a custom
sequence of data can be created on the GPIO pins.

### PWM

PWM, or “[Pulse Width
Modulation](https://en.wikipedia.org/wiki/Pulse-width_modulation)”, is
typically used to refer to changing (_modulating_) the _width_ of the
_pulses_ in a square wave sent to some device, like an LED or speaker,
in order to change the average brightness or volume, or timbre of the
sound. On the BCM2835, the PWM machinery can also be used to shunt
custom waveforms to GPIO pins.

### SPI

SPI “[Serial Peripheral
Interface](https://en.wikipedia.org/wiki/Serial_Peripheral_Interface)”
is a specification for short-distance communication with peripherals,
either two- or just one-way. Although intended for marshaling and
communication of data, the BCM2835 also allows SPI to be used to
simply push out a custom waveform.

## Methods of using the approaches

There are also different ways of using DMA, PWM, and SPI. The most
low-level but straight-forward approach is to read the
datasheet/manual, get enough privileges to reference the memory-mapped
control addresses, and directly control the hardware.

The less direct (and safer) approach is to use devices and modules
that people have written for the Linux kernel to control the hardware,
for which there are sometimes multiple approaches.

## Timing

The raw speeds of the RPi clocks are far too quick to be useful for
the slower data rate required by these radio devices. But all three
approaches have ways to use different clocks with different
frequencies, and to subdivide the clock frequency.

### <mark>Open questions</mark>
<mark>**TODO**: figure out:</mark>
- <mark>what clocks are available</mark>
- <mark>which can be used with which approach</mark>
- <mark>what their frequencies are</mark>
- <mark>which clock frequencies are dependent on base frequency</mark>
- <mark>which clock frequencies vary with model</mark>
- <mark>which clocks vary their frequencies as the RPi moderates its frequency to remain cool enough</mark>
- <mark>what subdivision ranges and granularity are possible with each approach</mark>
- <mark>how to find out the clock frequencies</mark>

## Summary table

|         | Direct access | Linux method 1 | Linux method 2 |
|---------|---------------|----------------|----------------|
| **DMA** |               |                |                |
| **PWM** |               |                |                |
| **SPI** |               |                |                |
