---
layout: default
title: Jay Valentine - Personal Projects
---

# Projects

On this page you can read a bit about the things I've done in my spare time.
These are all ongoing projects that I revisit whenever I find the time.

----

## Modular Z80 Computer

This is the first significant electronics project that I have attemped, and I didn't start small!
I'm aiming to build a modular Z80 computer, which will start out simple, just running a serial bootloader.
However, being a modular system I intend to upgrade it over time with components like video display, sound,
and even a CompactFlash hard disk! Eventually the system will be able to run a full-fledged OS, which I also intend
to write myself.

Read more about this project [here]({% link modularz80/index.md %}).

----

## ZFORTH

ZFORTH is a FORTH implementation targeting the Z80 8-bit microprocessor.
The core of the language is written entirely in Z80 assembler.
I'm hoping that it can eventually serve as the shell scripting language for
a Z80 multi-tasking operating system.

FORTH is a programming paradigm in which a lightweight interpreter executes
'words'. A small set of primitive 'words' are written directly in assembly language,
but the capability of the environment can be extended, as FORTH allows the ability
to define new words, and even redefine existing ones.

----

## Zemu

Zemu is a Ruby gem (library) which wraps a third-party Z80 CPU emulator, written in C.
It provides code generation for other components of a system (e.g. memory and IO),
and the ability to compile the system into a library. This library is then wrapped in a Ruby
object with which the user can programmatically control the execution of the emulated machine, for example
by setting breakpoints or accessing memory values.

An 'interactive' mode is also provided which allows a user to control the execution of the emulator
through a command-line interface, as well as exposing a virtual serial port to which a terminal
emulator can be attached.

Zemu has proved valuable during the development of ZFORTH, and I hope that I can use it as a platform
for prototyping custom Z80 computers that I later intend to build.

----

## EvoSim

EvoSim (short for Evolutionary Simulator) is a simulation of the evolution of a population of creatures
whose behaviours are controlled by simple neural network 'brains'. The 'brains' evolve through an RT-NEAT
mechanism, providing real-time changes in the population's traits.

The simulator is written in C++, with SDL2 used to provide the graphical capabilities.

----

## Monolith

Monolith is a web game inspired by epic science-fiction like 2001: A Space Odyssey.
It is a procedurally-generated text-based game in which you take on the role of a
spacecraft sent to monitor primitive alien civilizations.

The actions you take guide the progression of the tribes through time, but events are not
entirely under your control, and your decisions may have unexpected consequences.

Monolith is written in TypeScript, and can be played [here]({% link monolith/index.html %})!
