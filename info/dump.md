# Other random info

## SPI address

The SPI flash is mapped to `0x88100000`.

## GPIO

GPIOs can be read by reading the GPIO registers. By default every IO is configured as INPUT.

To read GPIO57 and GPIO58 boot flags execute the following:

```
p 9801b124
```

This outputs (GPIO57 HIGH, GPIO58 LOW):

```
0x9801b124 = 0xFB800000
```

## UART0

UART0 is located at `0x98007800`

    Writing to this address outputs the byte on the debug serial.
```
s
98007800
00000045
```

## DDR controller config

```
980081F0 = 0x00075214

DC_INFO_DRAM_TYPE_MASK (0x0000000f)
0x04 = TYPE_DDR4


DC_INFO_DQ_MODE_MASK (0x000000f0)
0x01 = FULL_DQ


DC_INFO_DRAM_USED_MASK (0x00000700)
0x02 = 4GB?


DC_INFO_DRAM_MODE_MASK (0x00003000)
0x01 = DRAM_MODE_x16


DC_INFO_DRAM_CELL_MASK (0x0000c000)
0x01 = DRAM_CELL_1 


DC_INFO_DRAM_SIZE_MASK (0x000f0000)
0x07 = 7GB?
```

[https://github.com/BPI-SINOVOIP/BPI-W2-bsp/blob/master/u-boot-rtk/arch/arm/include/asm/arch-rtd1295/ddr.h](https://github.com/BPI-SINOVOIP/BPI-W2-bsp/blob/master/u-boot-rtk/arch/arm/include/asm/arch-rtd1295/ddr.h)

## Execute assembly

It's possible to execute assembly code by putting into memory:
* 0x01500000 does not work - probably because of bad RAM
* 0x80000000 seems to work
* 0x00021000 seems to work

Change load address:
```
s
98007058
00021000
```

Change CPU mode if required:

```
s
98007078
00000080
```

Execute:
```
g
```

## UBoot boot commands:

```
rtkspi read spi_start_address ddr_start_address data_size
lzmadec srcaddr dstaddr [dstsize]
```

Actual usage found in the flash:
```
rtkspi read 0x00100000 0x0b000000 0x002f0000;
lzmadec 0x0b000000 0x03000000 0x002f0000;

rtkspi read 0x000c0000 0x0b000000 0x00040000;
lzmadec 0x0b000000 0x01b00000 0x00040000;

rtkspi read 0x00000000 0x01f00000 0x00010000;
rtkspi read 0x003f0000 0x02200000 0x003ff000
```

## Other addresses found in source codes:

* * [https://github.com/BPI-SINOVOIP/BPI-W2-bsp/blob/master/u-boot-rtk/include/configs/rtd1295_common.h](https://github.com/BPI-SINOVOIP/BPI-W2-bsp/blob/master/u-boot-rtk/include/configs/rtd1295_common.h)
* * [https://github.com/BPI-SINOVOIP/BPI-W2-bsp/blob/master/u-boot-rtk/arch/arm/include/asm/arch-rtd1295/rbus/iso_reg.h](https://github.com/BPI-SINOVOIP/BPI-W2-bsp/blob/master/u-boot-rtk/arch/arm/include/asm/arch-rtd1295/rbus/iso_reg.h)
* * [https://github.com/BPI-SINOVOIP/BPI-W2-bsp/blob/master/u-boot-rtk/arch/arm/cpu/armv8/rtd1295](https://github.com/BPI-SINOVOIP/BPI-W2-bsp/blob/master/u-boot-rtk/arch/arm/cpu/armv8/rtd1295)

```
#define CONFIG_KERNEL_LOADADDR	0x03000000
#define CONFIG_ROOTFS_LOADADDR	0x02200000
#define CONFIG_RESCUE_ROOTFS_LOADADDR 	0x02200000
#define CONFIG_LOGO_LOADADDR	0x02002000      //reserved ~2M
#define CONFIG_FDT_LOADADDR	0x02100000      //reserved 64K
#define CONFIG_BLUE_LOGO_LOADADDR 0x30000000

#define CONFIG_FW_LOADADDR	0x0f900000


#define CONFIG_NR_DRAM_BANKS		1
#define CONFIG_SYS_SDRAM_BASE		0x00000000
#define CONFIG_SYS_RAM_DCU1_SIZE	0x20000000		//512MB
```