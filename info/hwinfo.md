# HWInfo

HWInfo or HWSettings is a blob which contains settings for a specific hardware. The goal of this blob is to setup the CPU and its modules.

A text based format also exists, each line contains an instruction then data.

Most of the addresses and registers are unknown, but some of them can be found in the sources published by the BananaPi folks:

[https://github.com/BPI-SINOVOIP/BPI-W2-bsp](https://github.com/BPI-SINOVOIP/BPI-W2-bsp)

An example text file ("`z9s_hwsetting.config`"):

```
w 0xfffffff0,0x00000000
m 0x98007030,0xfffffff3,0x00000008
w 0xfffffff4,0x00000000
w 0xfffffff8,0x00000000
w 0x9800768c,0x08000000	# WDG enable
w 0x980076c0,0x08000000
w 0x98007680,0x80000000
w 0xfffffffc,0x00000000

#*******************************************************************************
# enable SB2 & RBUS clock gating
#*******************************************************************************
w 0x9801a308,0xffffffff
w 0x9801a30c,0x0000ffff
w 0x9801a310,0x0000000e
w 0x9801a020,0x00000000
m 0x9801d000,0xffffffff,0xff000000
m 0x9801d100,0xffffffff,0x80000000
w 0x9801a020,0x00000000
w 0x980151e0,0x047053ff	# set CP clock gating

# Clock Source & Divider Setting
#-------------------------------------------------------------------------------
w 0x98000018,0x00000007	# select clock sources for clk_scpu/clk_sys/clk_sysh
w 0x98000030,0x00000100	# select PLL divider for SCPU/ACPU/BUS PLL
w 0x9800004c,0x0000001c	# select clock sources for clk_ve1/clk_ve2/clk_ve3/clk_ve2_bpu

#*******************************************************************************
# CRT Configuration
#*******************************************************************************
# SCPU PLL Setting
#-------------------------------------------------------------------------------
w 0x98000500,0x0000000c	# turn off OC_EN_SCPU & RSTB
w 0x98000504,0x0001c213	# 800MHz, after 1.6GHz/2
w 0x98000500,0x0000000d	# turn on OC_EN_SCPU & release RSTB
w 0x98000508,0x001e1fe4
w 0x98000104,0x00000005
w 0x98000104,0x00000007

# BUS PLL Setting
#-------------------------------------------------------------------------------
w 0x98000520,0x0000000c	# turn off OC_EN_BUS & RSTB
w 0x98000524,0x00003400	# 256MHz
w 0x98000520,0x0000000d	# turn on OC_EN_BUS & release RSTB

# BUS_H PLL Setting
#-------------------------------------------------------------------------------
w 0x98000540,0x0000000c	# turn off OC_EN_BUS_H & RSTB
w 0x98000544,0x00007000	# 459MHz(Default)
w 0x98000540,0x0000000d	# turn on OC_EN_BUS_H & release RSTB

# ACPU PLL Setting
#-------------------------------------------------------------------------------
w 0x980005c4,0x00008af6	# 550MHz(Default)
w 0x98000110,0x00000007	# power on ACPU PLL & release RSTB
w 0x980005c0,0x0000000d	# turn on OC_EN_ACPU & release RSTB

# DDSA PLL Setting
#-------------------------------------------------------------------------------
w 0x98000564,0x00006800	# 432MHz(Default)
w 0x98000128,0x00000007	# power on DDSA PLL & release RSTB
w 0x98000560,0x0000000d	# turn on OC_EN_DDSA & release RSTB

# DDSB PLL Setting
#-------------------------------------------------------------------------------
w 0x98000584,0x00006800	# 432MHz(Default)
w 0x98000178,0x00000007	# power on DDSB PLL & release RSTB
w 0x98000580,0x0000000d	# turn on OC_EN_DDSB & release RSTB

# PSAUD1A/PSAUD2A PLL Setting
#-------------------------------------------------------------------------------
w 0x98000130,0x000006bd	# 49.152MHz(Default) & enable PS_EN
w 0x98000134,0x0000000f	# release RSTB

# PSAUD1B/PSAUD2B PLL Setting
#-------------------------------------------------------------------------------
w 0x98000188,0x000006bd	# 49.152MHz(Default) & enable PS_EN
w 0x9800018c,0x0000000f	# release RSTB

# PLL Output Enable Setting
#-------------------------------------------------------------------------------
n 20000
w 0x98000104,0x00000003	# enable SCPU PLL OEB
w 0x98000110,0x00000003	# enable ACPU PLL OEB
w 0x98000128,0x00000003	# enable DDSA PLL OEB
w 0x98000178,0x00000003	# enable DDSB PLL OEB
w 0x98000134,0x00000005	# enable PSAUD1A/PSAUD2A PLL OEB
w 0x9800018c,0x00000005	# enable PSAUD1B/PSAUD2B PLL OEB
w 0x98000018,0x00000000	# select clock sources for clk_scpu/clk_sys/clk_sysh

# Reset & Clock Enable Setting
#-------------------------------------------------------------------------------
w 0xfffffff8,0x00000000	# for cold boot
w 0x9800000c,0x13fc0061	# enable CLOCK_ENABLE1
w 0x98000010,0xd801e406	# enable CLOCK_ENABLE2
w 0x98000014,0x00000000	# enable GROUP_CK_EN
w 0x98000000,0xaf800001	# release SOFT_RESET1
w 0x98000004,0x1f840e01	# release SOFT_RESET2
w 0x98000050,0x00000000	# release SOFT_RESET4
w 0xfffffffc,0x00000000

w 0xfffffff0,0x00000000	# enable ae/acpu when resume
w 0x9800000c,0x13fc0071	# enable CLOCK_ENABLE1
w 0x98000010,0xd801e416	# enable CLOCK_ENABLE2
w 0x98000014,0x00000000	# enable GROUP_CK_EN
w 0x98000000,0xbf800355	# release SOFT_RESET1
w 0x98000004,0x1f840e3d	# release SOFT_RESET2
w 0x98000050,0x00000000	# release SOFT_RESET4
w 0xfffffff4,0x00000000

#*******************************************************************************
# ISO Configuration
#*******************************************************************************
w 0x980076c4,0x00000001	# turn on watchdog reset OE
w 0x98007078,0x00000080	# ROM code jump to AARCH64 (jumper)
w 0x98007100,0x00000002	# set type c power gpio output
w 0x98007104,0x00000000	# set type c power gpio low

#*******************************************************************************
# SB2 Configuration 
#*******************************************************************************
w 0x9801a808,0x00000013	# NOR flash access

#*******************************************************************************
# SBX Configuration 
#*******************************************************************************
w 0x9801c608,0x00000001	# change CR priority

#*******************************************************************************
# DC_SYS Configuration 
#*******************************************************************************
w 0x98008004,0x01c00f89
w 0x98008020,0x3a1fbb30
w 0x98008740,0x00000008

#*******************************************************************************
# DPI_CRT Configuration
#*******************************************************************************
m 0x98000050,0xfffffff6,0x00000009
w 0x9800e018,0x05505575
w 0x9800e00c,0x0000000f
w 0x9800e024,0x00000000
w 0x9800e07c,0x00000002
w 0x9800f018,0x05505575
w 0x9800f00c,0x0000000f
w 0x9800f024,0x00000000
w 0x9800f07c,0x00000002
m 0x98000050,0xffffffe0,0x0000001f
w 0x9800e028,0x04001f1f
w 0x9800e028,0x00001f1f
w 0x9800e01c,0x00000008
w 0x9800f028,0x04001f1f
w 0x9800f028,0x00001f1f
w 0x9800f01c,0x00000008
n 20000
w 0x9800e09c,0x000017ff
w 0x9800e010,0x00000000
w 0x9800e014,0x00080000
w 0x9800e098,0x00080808
w 0x9800f09c,0x000017ff
w 0x9800f010,0x00000000
w 0x9800f014,0x00080000
w 0x9800f098,0x00080808
w 0x9800e030,0x13052807
w 0x9800e034,0x13052807
w 0x9800e038,0x13052807
w 0x9800e03c,0x13052807
w 0x9800e040,0x13052807
w 0x9800e044,0x13052807
w 0x9800e048,0x13052807
w 0x9800e04c,0x13052807
w 0x9800e050,0x13052807
w 0x9800e054,0x13052807
w 0x9800e058,0x13052807
w 0x9800e0a4,0x13052807
w 0x9800f030,0x13052807
w 0x9800f034,0x13052807
w 0x9800f038,0x13052807
w 0x9800f03c,0x13052807
w 0x9800f040,0x13052807
w 0x9800f044,0x13052807
w 0x9800f048,0x13052807
w 0x9800f04c,0x13052807
w 0x9800f050,0x13052807
w 0x9800f054,0x13052807
w 0x9800f058,0x13052807
w 0x9800f0a4,0x13052807
w 0x9800e000,0x80000031
w 0x9800e080,0x33333333
w 0x9800f000,0x80000031
w 0x9800f080,0x33333333
w 0x9800e004,0x0fff0fff
w 0x9800e008,0x00070000
w 0x9800f004,0x0fff0fff
w 0x9800f008,0x00070000

#*******************************************************************************
# DPI_DLL Configuration
#*******************************************************************************
w 0x9800e4c0,0x0000003e
w 0x9800e4c4,0x0000003e
w 0x9800e4c8,0x0000003e
w 0x9800e4cc,0x0000003e
w 0x9800e52c,0x00000000
w 0x9800e530,0x00000000
w 0x9800e534,0x00000000
w 0x9800e538,0x00000000
w 0x9800e254,0x00000403
w 0x9800e244,0x00000002
w 0x9800e248,0x00000002
w 0x9800e24c,0x00000002
w 0x9800e250,0x00000002
w 0x9800e318,0x080f2046
w 0x9800e31c,0x0000000c
w 0x9800e318,0x080f2044
w 0x9800f4c0,0x0000003e
w 0x9800f4c4,0x0000003e
w 0x9800f4c8,0x0000003e
w 0x9800f4cc,0x0000003e
w 0x9800f52c,0x00000000
w 0x9800f530,0x00000000
w 0x9800f534,0x00000000
w 0x9800f538,0x00000000
w 0x9800f254,0x00000403
w 0x9800f244,0x00000002
w 0x9800f248,0x00000002
w 0x9800f24c,0x00000002
w 0x9800f250,0x00000002
w 0x9800f318,0x080f2046
w 0x9800f31c,0x0000000c
w 0x9800f318,0x080f2044

#*******************************************************************************
# RXI310 Configuration
#*******************************************************************************
w 0x9800ea24,0x80000064
w 0x9800e804,0x00000213
w 0x980081f0,0x00075213
w 0x9800e808,0x00403214
w 0x9800e810,0x010dac01
w 0x9800e810,0x010dac7a
w 0x9800e814,0x00008f07
w 0x9800e818,0x11689d73
w 0x9800e81c,0x178ea1e2
w 0x9800e830,0x000000e5
w 0x9800e834,0x00000e14
w 0x9800e838,0x00000006
w 0x9800e83c,0x00000020
w 0x9800e80c,0x00000700
w 0x9800e800,0x00000100
w 0x9800e82c,0x00000003
w 0xfffffff8,0x00000000
m 0x98007f08,0xfffffffa,0x00000005
w 0x9800e800,0x00000001
p 0x9800e800,0x0000000f,0x0000000d
w 0xfffffffc,0x00000000
w 0xfffffff0,0x00000000
w 0x9800e800,0x80000000
m 0x98007f08,0xfffffffa,0x00000005
w 0x9800e824,0x03e00000
w 0x9800e828,0x03e00000
w 0x9800ea24,0x80000064
w 0x9800e800,0x00000008
w 0xfffffff4,0x00000000
w 0x9800e80c,0x00000600
w 0x9800fa24,0x80000064
w 0x9800f804,0x00000213
w 0x980081f4,0x00075213
w 0x9800f808,0x00403214
w 0x9800f810,0x010dac01
w 0x9800f810,0x010dac7a
w 0x9800f814,0x00008f07
w 0x9800f818,0x11689d73
w 0x9800f81c,0x178ea1e2
w 0x9800f830,0x000000e5
w 0x9800f834,0x00000e14
w 0x9800f838,0x00000006
w 0x9800f83c,0x00000020
w 0x9800f80c,0x00000700
w 0x9800f800,0x00000100
w 0x9800f82c,0x00000003
w 0xfffffff8,0x00000000
m 0x98007f08,0xfffffff5,0x0000000a
w 0x9800f800,0x00000001
p 0x9800f800,0x0000000f,0x0000000d
w 0xfffffffc,0x00000000
w 0xfffffff0,0x00000000
w 0x9800f800,0x80000000
m 0x98007f08,0xfffffff5,0x0000000a
w 0x9800f824,0x03e00000
w 0x9800f828,0x03e00000
w 0x9800fa24,0x80000064
w 0x9800f800,0x00000008
w 0xfffffff4,0x00000000
w 0x9800f80c,0x00000600

#*******************************************************************************
# DZQ Configuration
#*******************************************************************************
w 0x9800e21c,0x0c080000
w 0x9800e2fc,0x59580000
m 0x9800e318,0xfffffffc,0x00000002
m 0x9800e318,0xfffffffc,0x00000000
w 0x9800e31c,0x00000003
w 0x9800e2cc,0x00286c36
w 0x9800e21c,0x0d080000
p 0x9800e220,0x80000000,0x80000000
w 0x9800e21c,0x0c080000
w 0x9800e21c,0x1c080000
w 0x9800e2fc,0xd9580000
m 0x9800e318,0xfffffffc,0x00000002
m 0x9800e318,0xfffffffc,0x00000000
w 0x9800e31c,0x00000003
w 0x9800e2cc,0x0020763b
w 0x9800e21c,0x1d080000
p 0x9800e220,0x80000000,0x80000000
w 0x9800e21c,0x1c080000
n 1000
w 0x9800f21c,0x0c080000
w 0x9800f2fc,0x59580000
m 0x9800f318,0xfffffffc,0x00000002
m 0x9800f318,0xfffffffc,0x00000000
w 0x9800f31c,0x00000003
w 0x9800f2cc,0x00286c36
w 0x9800f21c,0x0d080000
p 0x9800f220,0x80000000,0x80000000
w 0x9800f21c,0x0c080000
w 0x9800f21c,0x1c080000
w 0x9800f2fc,0xd9580000
m 0x9800f318,0xfffffffc,0x00000002
m 0x9800f318,0xfffffffc,0x00000000
w 0x9800f31c,0x00000003
w 0x9800f2cc,0x0020763b
w 0x9800f21c,0x1d080000
p 0x9800f220,0x80000000,0x80000000
w 0x9800f21c,0x1c080000
n 1000
w 0x9800e264,0x00000000
w 0x9800e2d4,0x00000000
w 0x9800e2d8,0x00110011
w 0x9800e2dc,0x11001100
w 0x9800e2f0,0x00000000
w 0x9800e2f4,0x00000000
w 0x9800e2f8,0x11111111
w 0x9800f264,0x00000000
w 0x9800f2d4,0x00000000
w 0x9800f2d8,0x00110011
w 0x9800f2dc,0x11001100
w 0x9800f2f0,0x00000000
w 0x9800f2f4,0x00000000
w 0x9800f2f8,0x11111111
#*******************************************************************************
# other setting
#*******************************************************************************
m 0x98007078,0xffffffef,0x00000010
w 0xff018000,0x00000001	# Turn on CPU timer
w 0xfffffff8,0x00000000
w 0x98007680,0x000000a5	# WDG disable
w 0xfffffffc,0x00000000
```