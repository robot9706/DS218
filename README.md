# Hardware reversing of a DS218 NAS

## Purpose

This repo was made to document information about the Synology DS218 NAS and **mainly** the RealTek RTD1296 ARM CPU after a failed attempt of trying to fix one.

It is for educational purposes only.

There was no attempt of reverse engineering the Synology firmware. It's irrelevant for the purpouse of this project.

## Story

A DS218 randomly decided to not work anymore.

The debug serial shows the following:

```
C1:80000000
C2
?
C3h
hwsetting size: 00000B94
C4
f
5-5verify fsbl fail
0000003BC7
C1:80000000
C2
?uu3-1
```

"`verify fsbl fail`" might mean "First Stage Bootloader verify fail", so at first I decided to reflash the bootloader in the SPI flash, however it did not seem to be corrupted. Looking at the communication between the CPU and the SPI flash the bootloader partition is loaded and then the boot fails. After looking into many things and learning a lot about the CPU my conclusion is that one (or more) RAM chip(s) died making this an interesting journey.

Testing the RAM using the peek and set commands (using the InternalROM) it seems like random bytes in memory flip, or they remain 0x00.

## DS218 specs

* CPU: RTD1296
* * ARM® Cortex-A53 quad-core CPU
* * ARM® Mali-T820 MP3 GPU
* * USB 2.0 and 3.0
* * HDMI RX/TX
* * Gigabit ethernet
* * PCIe
* * SATA 3.0
* * Other media features (encoding - decoding)
* RAM: 4GB - DDR4
* Storage: 2x SATA HDD
* Gigabit ethernet
* 2x USB3.0
* 1x USB2.0

## Pictures

Pictures of the DS218 can be found in the "[pictures](/pictures/README.md)" folder.

## Gathered info

This is the information I gathered while trying to fix the NAS:

* [Flash layout](/info/flash.md)
* [Boot process](/info/boot.md)
* [Internal ROM](/info/rom.md)
* [HWInfo blob](/info/hwinfo.md)

## Documentation

Documentation gathered during the research process can be found in the "[docs](/docs)" folder.

## Links

[DS218](https://www.synology.com/products/DS218)

[RTD1296](https://www.realtek.com/en/products/communications-network-ics/item/rtd1295) 