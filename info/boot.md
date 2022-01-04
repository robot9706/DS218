# Boot

Boot process:

1. The CPU boots up after Power-on-reset. Reads the BOOT_SEL, GPIO57 and GPIO58 pins to decide the booting process. In case of the DS218 it's an InternalRom NOR boot.
2. The internal rom reads the HWInfo from 0x00020800 in the flash.
3. The internal rom proceeds to read the rest of the first partition.
4. An SHA256 hash is calculated for the loaded data, if it matches a predefined (OTP, eFuse, somwhere in flash ?) value it jumps to the loaded first stage bootloader which takes over the boot process.
5. The FSBL loads a second bootloader (UBoot) which loads the Linux kernel.

The boot process can be interupted by sending CTRL+Q, see [Internal ROM](rom.md).

## HWInfo in flash

The info binary has a 96 byte header, the first 4 bytes is the length.

At the end of the HWInfo binary there's probably a CRC, but the true ending of the binary is a 0xFFFFFFFF sequence.