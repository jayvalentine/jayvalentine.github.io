---
layout: modularz80
title: Jay Valentine - Modular Z80 Computer - Bootloader
---

## ZBoot, a Z80 Bootloader/Monitor

<figure>
    <img src="/modularz80/images/bootloader.png" alt="ZBoot interface"/>
    <figcaption><i>ZBoot interface when loading a HEX file.</i></figcaption>
</figure>

The source code for this program can be seen at the [GitHub repository](https://github.com/jayvalentine/z80-bootloader).

The bootloader is a simple program that allows the loading of other programs over the serial port (or eventually a hard disk)
into RAM to be executed. This means that the computer's EEPROM does not need to be re-programmed every time a new program is to be run.

Programs to be loaded are described in [Intel HEX](https://en.wikipedia.org/wiki/Intel_HEX) format.

### Hardware Interface

One of the most helpful components a piece of system software can offer is abstracted access to the hardware components of the system.
For ZBoot, this comes in the form of a "syscall" interface - although "syscall" is something of a misnomer, considering the Z80 has no concept
of privileged and unprivileged execution.

In a program running on top of ZBoot, a syscall is made by loading an offset into the `A` register, and executing an `RST 48` instruction.
This instruction calls a subroutine at `0x0030` in the system's ROM, which looks up the corresponding syscall routine using the offset in `A`.
Because the offset must be a multiple of 2, this gives a total number of 128 syscalls, numbered 0-127.

The syscall handler looks something like this:

```
    org     $0030               ; Start at 30h
syscall_entry:
    jp      syscall_handler     ; Jump to syscall routine

syscall_handler:
    push    HL                  ; Save HL and DE on stack because they
    push    DE                  ; are used by the syscall routine

    ; Calculate the position in the syscall table for this syscall.
    ; This is a bit of a cheat that relies on having the syscall table
    ; at a 256-byte aligned address, but means we don't have to do any
    ; addition.
    ld      D, ((syscall_table & 0xff00) >> 8)  
    ld      E, A
    
    ; Load the syscall routine address from (DE) into HL.
    ld      A, (DE)
    ld      L, A
    inc     DE
    ld      A, (DE)
    ld      H, A
    
    ; Jump to the syscall routine, now in HL.
    ; The syscall routine will restore HL and DE as they might
    ; contain parameters to the syscall.
    jp      (HL)

    ; Syscall table must be located at a 256 byte aligned address.
    org     $0100
syscall_table:
    addr    syscall_swrite
    addr    syscall_sread
```

and would be called using the following sequence of instructions (presented as a macro to make using the syscalls easier):

```
    macro   zsyscall, number
    ld      A, \number << 1
    rst     48
    endmacro
```

Seeing as there's no concept of privilege levels in the Z80, this syscall interface is not *strictly* necessary.
For example, we could just expose the addresses of important subroutines, such as the serial read and write routines,
in a header file of some kind, which could then be called by the hosted program.

However, this would mean that any code using the hardware interface would have to be recompiled every time the bootloader
code changes. This is a pain, particularly while the bootloader is under active development, and is prone to error.

This mechanism is expensive (the code above requires *78* cycles just to get to the syscall routine, rather than the 17 required for a `call`)
but ultimately the time saved during development is worth it. As an added bonus, were I to write full-fledged OS for this machine -
so long as the OS presented the same interface to the hosted program - no changes would be necessary to a program written for the bootloader!
