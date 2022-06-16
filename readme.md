# H8/300 processor module for Ghidra

## Language definitions (`.ldefs`)

A language defines a specific processor model and its corresponding
processor specification file (`.pspec`) and its _slafile_ (`.sla`)

The file also defines one or many compiler definitions (`.cspec`) for the processor. This is used by the decompiler module.

## Processor specification (`.pspec`)

Defines registers (or just the program counter?)

## Compiler specification (`.cspec`)

Define call ABI?

## SLEIGH file (XML) (`.sla`)

The compiled processor specification. 

## SLEIGH source file (SLEIGH language) (`.slaspec`)

This is the source file in the SLEIGH formal language. When compiled it generates the much more verbose `.sla` processor specification file in XML format.

## Addressing modes

* Register direct - `Rn`
* Register indirect - `@ERn`
* Register indirect w. displacement - `@(d:16, ERn)` or `@(d:24, ERn)` - 16-bit is sign extended
* Register indirect w. post-increment or pre-decrement - `@ERn+` or `@-ERn`
* Absolute address - `@aa:8` or `@aa:16` or `@aa:24` 8 bits maps to highest mem range (FFFFnn). 16 bits maps to low for 0..7FFF and high for 8000+ (00aaaa, FFbbbb)
* Immediate - `#xx:8` or `#xx:16` or `#xx:32`
* PC-relative - `@(d:8, PC)` or `@(d:16, PC)`
* Memory indirect - `@@aa:8` get longword in 000000..0000FF range

## H8/300H compared to H8/300

H8/300H is binary compatible with all instructions for H8/300

H8/300H has added:

* More general registers (8 16-bit registers)
* Extended address space (24 bit compared to 16-bit)
* Addressing have been enhanced to make use of 24 bit addresses
* Data-transfer, arithmentic and logic can use 32-bit data
* Signed mul/div and more instructions.

## Registers

* `ER0`..`ER7` where `ER7` doubles as stack pointer `SP`
  `ERn` (32 bit) contains `En` (16 bit) followed by `Rn` (16 bit) containing `RnH` (8 bit) and `RnL` (8 bit)
* `PC` (24 bit)
* `CCR` I,UI,H,U,N,Z,V,C

## Links

[https://spinsel.dev/assets/2020-06-17-ghidra-brainfuck-processor-1/ghidra_docs/language_spec/html/sleigh_definitions.html](SLEIGH specification)
