---
layout: default
title: Jay Valentine - Modular Z80 Computer
---

# Modular Z80 Computer

Here you can find details of my modular Z80 computer. This page will be expanded over time as I upgrade the computer with new capabilities.
This system was heavily inspired by [RC2014](https://rc2014.co.uk/), a similar modular Z80 system. Because of this, I'm not going to sell
kits for this project. If you want to buy a ready-made kit, please take a look at the RC2014!

## System Architecture

The system is designed around a simple bus, comprised of the Z80's control signals, address and data lines, along with power, clock and reset.
All modules in the system interface with this bus. The decision to simply break out the Z80 control signals, rather than do any kind of decoding
between the processor and the bus, makes the bus flexible, as different modules can interpret the signals in different ways as required.
For example, a bankable RAM module might have a memory interface for the RAM, and an I/O port for the bank select register. Such a module would
not be possible if the memory and I/O ports were separated into separate busses.

However, with this design comes a certain level of duplication. For example, each module has to handle its own address and control signal decoding,
rather than assigning ports to address ranges. Ultimately, I felt that this design would be easier to design and construct modules for, and gave the
highest level of flexibility.

## Processor Module

The heart of the system is the Z80 processor module. This is little more than a breakout board for the Z80's DIP-40 package, although it does include
pull-up resistors for some of the signals (`BUSREQ`, `INT`, `NMI`, `WAIT`) to ensure that their default state is high (inactive), while ensuring that they can be
pulled low by external modules.
