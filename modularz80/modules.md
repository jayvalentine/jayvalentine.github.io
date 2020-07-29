---
layout: modularz80
title: Jay Valentine - Modular Z80 Computer - Hardware Modules
---

## Hardware Modules

This page describes the various hardware modules that I've built for the modular Z80 computer.

### Processor Module

The heart of the system is the Z80 processor module. This is little more than a breakout board for the Z80's DIP-40 package, although it does include
pull-up resistors for some of the signals (`BUSREQ`, `INT`, `NMI`, `WAIT`) to ensure that their default state is high (inactive), while ensuring that they can be
pulled low by external modules.

### 8K Boot ROM

This module provides 8K of ROM, starting at address `0x0000`, which is the Z80's reset vector.
A 74LS32 is used to `OR` the `MREQ` and `RD` signals together, producing a signal which is only low (active) when the Z80 is attempting to read from memory
(as opposed to reading from I/O). This is wired to the `OE` (output-enable) of the ROM. Address lines `A15`, `A14` and `A13` are `OR`-ed together to produce
the ROM's `CE` (chip-enable) signal, mapping it to the lower 8K of the Z80's address space.

### 32K High RAM

This module provides 32K of RAM located at the higher end of the Z80's address space (`0x8000` to `0xFFFF`). Like the ROM module, the `MREQ` and `RD` signals
are used to interface to the RAM, along with the `WR` signal for write operations. The `A15` address line is inverted and used as the RAM's `CE` signal,
mapping it to the upper 32K of the address space.

### Serial I/O

This module uses a Motorola 6850 ACIA to provide a single serial port which can be connected to using an FTDI USB-to-TTL-serial adapter.
Four pins are provided for connecting to the FTDI adapter - power, ground, `Tx` and `Rx`. This provides the possibility of powering the computer from the FTDI
adapter.

The 6850 uses the system clock of 3.6864MHz for baud-rate generation, which when divided by 64 gives a baud-rate of 57600 - plenty fast for most applications.
