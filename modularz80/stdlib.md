---
layout: modularz80
title: Jay Valentine - Modular Z80 - Standard Library
---

## Implementing a Standard Library in Assembler

I've struggled to find a C compiler that can generate efficient code for the Z80, mainly due to the design decisions made in the processor's
architecture. I'm not alone, and so the Z80 C compiler I'm using, [z88dk](https://www.z88dk.org), implements most of the core functionality of the C standard library in assembler.
However, I wanted to try implementing my own for two reasons:

1. Some aspects of the standard library (e.g. acessing files, serial port, etc) will be hardware-specific, and even if I used z88dk's libraries, I'd have to port some of it anyway.
2. It'll be fun!

At the minute I have a minimal library, but over time hopefully this will prove a useful standard library when writing user programs for the modular Z80.

### C Standard Library

Being a C programmer, I'm familiar with a lot of the standard library functions and have definitely missed `printf` and `strcmp` while coding in assembler!
I'm currently building up a set of routines that can be used from both C and assembler code.

#### stdio.h

Currently, `stdio.h` is the only part of the standard library that I've implemented, and even then only at the most basic level.
`gets` and `puts` exist, for reading and writing the serial port (which is my system's current `stdout`/`stdin`).
At some point soon I'm also going to write an assembler implementation of `printf` up and running, which will be useful for more complex user interfaces.

File handling will come after, once I have a CompactFlash card set up as a hard-drive!

### Non-Standard Libraries

In addition to the C standard library, I'll need to implement drivers for some of the peripherals for the computer, for controlling sound, drawing graphics and more.

#### SN76489 MIDI Driver

I'm currently prototyping a board with one (or potentially two) Texas Instruments SN76489 chips for generating sound. Each chip has three square-wave voices and a noise generator.
I figured that a good way to drive this board in a generic manner would be to allow the playing of MIDI notes. The driver allows frequencies to be played on the SN76489 in terms of MIDI
note numbers, abstracting away from the control register format used by the chip.
