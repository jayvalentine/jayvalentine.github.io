---
layout: default
title: Jay Valentine - Modular Z80 Computer
---

# Modular Z80 Computer

Here you can find details of my modular Z80 computer. This page will be expanded over time as I upgrade the computer with new capabilities.
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

## Processor Module

The heart of the system is the Z80 processor module. This is little more than a breakout board for the Z80's DIP-40 package, although it does include
pull-up resistors for some of the signals (`BUSREQ`, `INT`, `NMI`, `WAIT`) to ensure that their default state is high (inactive), while ensuring that they can be
pulled low by external modules.

## ROM

Currently, the system has 8K of ROM, starting at address `0x0000`. This ROM contains the serial bootloader.
A 74LS32 is used to `OR` the `MREQ` and `RD` signals together, producing a signal which is only low (active) when the Z80 is attempting to read from memory
(as opposed to reading from I/O). This is wired to the `OE` (output-enable) of the ROM. Address lines `A15`, `A14` and `A13` are `OR`-ed together to produce
the ROM's `CE` (chip-enable) signal, mapping it to the lower 8K of the Z80's address space.

## RAM

32K of RAM is located at the higher end of the Z80's address space (`0x8000` to `0xFFFF`). This space is currently used for storing the program loaded over serial by
the bootloader, as well as the stack. Like the ROM module, the `MREQ` and `RD` signals are used to interface to the RAM, along with the `WR` signal for write operations.
The `A15` address line is inverted and used as the RAM's `CE` signal, mapping it to the upper 32K of the address space.

## Serial I/O

Currently, the only I/O for the system is a Motorola 6850 ACIA, which provides a single serial port which can be connected to using an FTDI USB-to-TTL-serial adapter.
Four pins are provided for connecting to the FTDI adapter - power, ground, `Tx` and `Rx`. This provides the possibility of powering the computer from the FTDI adapter.

The 6850 uses the system clock of 3.6864MHz for baud-rate generation, which when divided by 64 gives a baud-rate of 57600 - plenty fast for most applications.
