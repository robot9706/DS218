# RealTek internal ROM

The CPU has a built in ROM which handles booting from multiple sources (SPI, NAND, USB) and even has a built in terminal.

The ROM can be found in [internal_rom.bin](../dump/README.md).

## Built in terminal

When the CPU is powered up a built in terminal can be accessed by sending CTRL+Q using the debug serial port. This halts the boot process and presents a "`d/g/r>`" terminal.

It supports the following commands:

|Command|Description|
|---|-------|
|d|Download a binary using Y-modem, the target address is located in 0x98007058|
|g|Jump to the address in 0x98007058|
|h|Load HWInfo using Y-modem, it is loaded usually around 0x80000000|
|j and r|Unknown command, asks for 4 words then a password.|
|p [memory address]|Peek, read any memory address.|
|s|Set, write any memory address. Requires the address and the data in two separate lines|

Note: None of the commands require the 0x prefix.

```
d/g/r>s
98007058
01000000
```

## Loading HWInfo

Before using almost any command it is advised to load the HWInfo file using the "`h`" command. This will ensure that most of the CPU modules are setup, mainly the DRAM controller. Without this step it is impossible to access RAM.

## Magic memory addresses

Some memory addresses are used by the Internal ROM:

|Address|Description|
|---|---|
|0x98007078|Defines the CPU mode for the "g" Go command. If the 7. bit is set it runs as AArch64 (so 0x00000080), otherwise AArch32 (0x00000000).|
|0x98007058|Contains the address where the download command puts the binary as well as where the "g" Go command will jump.|

## Other info

* The SPI flash is mapped to `0x88100000`.

* The FSBL is loaded to `0x10100000`.

## Dumping the ROM

The internal rom can be dumped using the built in terminal by reading every address from "0x00000000 - 0x08000000" which is the ROM area. The "`internal_rom.bin`" only contains (0x00000000 - 0x000125E7).
