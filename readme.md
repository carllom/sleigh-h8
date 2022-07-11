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

[https://trenchant.io/expanding-the-dragon-adding-an-isa-to-ghidra/](An easier introduction to SLEIGH definitions)

[https://swarm.ptsecurity.com/creating-a-ghidra-processor-module-in-sleigh-using-v8-bytecode-as-an-example/](Step-by-step walkthrough of implementing a model (so-so))

## TODOs

Check half-carry in ccr macro - what if it is word (bit11) or longword (bit??) op? How to distinguish size of input variables? Or different macros for different sizes?

carry (C) or scarry/overflow(V) uses built in functions - is OK?

~! match 0 bits, sometimes they do matter (band, biand) - go through all instructions and fill in 0-bits~

Use more `@define`s? Use300H, UseAdvanced

## C interface

### Renesas C compiler

[https://www.renesas.com/us/en/document/mat/h8s-h8300-series-cc-compiler-package-ver700-users-manual?language=en&r=1169476](Renesas c compiler manual page 279)

Default 2 registers reserved for parameters?

* ER0-ER1 caller-save (H8/300: R0-R1) (also E/R2 if 3 parameter registers defined)
  These registers can be overwritten by function at will
* ER2-ER6 calee-save (H8/300: R2-R6) (or E/R3.. if 3 parameter registers defined)
  These registers are restored before returning

@253D0 Register restore helper function (sort of a stdcall helper)
@253F2 Register save helper function

@3ff misread A8 (should be 28?)
