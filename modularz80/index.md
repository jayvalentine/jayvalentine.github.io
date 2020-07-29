---
layout: modularz80
title: Jay Valentine - Modular Z80 Computer
---

## Introduction

Here you can find details of my modular Z80 computer. These pages will be expanded over time as I upgrade the computer with new capabilities.
This system was heavily inspired by [RC2014](https://rc2014.co.uk/), a similar modular Z80 system. If you're not bothered about designing your own system,
and just want something ready-made to play around with, take a look there!

## System Architecture

The system is designed around a simple bus, comprised of the Z80's control signals, address and data lines, along with power, clock and reset.
All modules in the system interface with this bus. The decision to simply break out the Z80 control signals, rather than do any kind of decoding
between the processor and the bus, makes the bus flexible, as different modules can interpret the signals in different ways as required.
For example, a bankable RAM module might have a memory interface for the RAM, and an I/O port for the bank select register. Such a module would
not be possible if the memory and I/O ports were separated into separate busses.

However, with this design comes a certain level of duplication. For example, each module has to handle its own address and control signal decoding,
rather than assigning ports to address ranges. Ultimately, I felt that this design would be easier to design and construct modules for, and gave the
highest level of flexibility.

The bus is composed of 40 signals, which allows the use of 40-pin headers on the main-board. While 36 pins would be sufficient for a simple computer,
it would not be wide enough to carry all the signals of the Z80, and 40-pin headers seem to be more widely available as well. With the current bus width,
2 lines remain unused, although on the current main-board design these pins are still connected to the bus, so they could be used for something in the future.
