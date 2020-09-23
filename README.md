### mdevx tool

The tool called mdevx was developed in Year 2008 for Coldfire 7475 silicon.

https://www.rockbox.org/wiki/pub/Main/DataSheets/CFPRM.pdf

#### Purpose

mdevx is a simple command-line program used to read/write to hardware locations in a 32 bit Armv7-A architecture:
ARM Cortex-A5, ARM Cortex-A7, ARM Cortex-A8, ARM Cortex-A9, ARM Cortex-A12, ARM Cortex-A15, ARM Cortex-A17

It is a standalnoe tool.

It it reccommended for the 32 bit MMU class of silicon: armv7, ColdFire and similar
architectures.

It does similar functionality/job as the Open Source tool: devmem2

https://github.com/hackndev/tools/blob/master/devmem2.c

#### INTEL tool for similar purpose

I strongly reccomment for x86 and x86_64 architecture the following tool:

https://github.com/Intel-ADSC/samtool

#### Compiling and linking for armv7 architectures (on target platforms)

	gcc mdevx.c -o mdevx

#### Usage

Usage is very simple - help built-in/included within the tool:

	# mdevx

	--------------------------------------------------------------------------------
	 Mdevx Read/Write Utility
	 Copyright (c) 2008 - 2020 Zoran Stojsavljevic. All rights reserved.
	--------------------------------------------------------------------------------
	 Usage: ./mdevx <addr> <data>       (for writing tool expects 2, 4 or 8 nibbles exact)
	 Usage: ./mdevx <addr> <format> <#> (for # (default 1) reading, format is b/h/w)
	 Usage: ./mdevx <addr>              (interactive mode, 8 nibbles exact for <addr>)
	 Interactive mode: <offset> <data/format> (description for data/format above)
	 Interactive mode: <cr>          (repeat previous format by entering <cr>)
	--------------------------------------------------------------------------------

#### Examples

I reccommend to test the tool browsing BBB memory (from kernel source code, the file:
<kernel_root_directory>/arch/arm/boot/dts/am335x-bone-common.dtsi

	line 13:	memory@80000000 {
	line 14:		device_type = "memory";
	line 15:		reg = <0x80000000 0x10000000>; /* 256 MB */
	line 16:	}

Read command:

	./mdevx 0x80000000 w
	32 bit data read [0x80000000] = 0xbe0a1eeb

Interactive mode examples:

	debian@arm:~/mdevx$ sudo su
	root@arm:/home/debian/mdevx# ./mdevx 0x82000000
	[base address 0x82000000], <offset> <data|b/h/w>? 0 b <<== offset 0, byte size (8 bit)
	 8 bit data read [0x82000000] = 0x4d
	[base address 0x82000000], <offset> <data|b/h/w>? 
	 8 bit data read [0x82000001] = 0x5a
	[base address 0x82000000], <offset> <data|b/h/w>? 
	 8 bit data read [0x82000002] = 0x10
	[base address 0x82000000], <offset> <data|b/h/w>? 
	 8 bit data read [0x82000003] = 0x13
	[base address 0x82000000], <offset> <data|b/h/w>?

	root@arm:/home/debian/mdevx# ./mdevx 0x82000000
	[base address 0x82000000], <offset> <data|b/h/w>? 0 w <<== offset 0, word size (32 bit)
	32 bit data read [0x82000000] = 0x13105a4d
	[base address 0x82000000], <offset> <data|b/h/w>? 
	32 bit data read [0x82000004] = 0x13105a4d
	[base address 0x82000000], <offset> <data|b/h/w>? 
	32 bit data read [0x82000008] = 0x13105a4d
	[base address 0x82000000], <offset> <data|b/h/w>? 
	32 bit data read [0x8200000c] = 0x13105a4d
	[base address 0x82000000], <offset> <data|b/h/w>?

Memory dump example:

	root@arm:/home/debian/mdevx# ./mdevx 0x82000000 b 256

	0x82000000  0x4d 0x5a 0x10 0x13 0x4d 0x5a 0x10 0x13 - 0x4d 0x5a 0x10 0x13 0x4d 0x5a 0x10 0x13 MZ..MZ..MZ..MZ..
	0x82000010  0x4d 0x5a 0x10 0x13 0x4d 0x5a 0x10 0x13 - 0x4d 0x5a 0x10 0x13 0x03 0xf0 0x4f 0xe2 MZ..MZ..MZ....O.
	0x82000020  0x00 0xf0 0xee 0xbf 0x18 0x28 0x6f 0x01 - 0x00 0x00 0x00 0x00 0x00 0x32 0x86 0x00 .....(o......2..
	0x82000030  0x01 0x02 0x03 0x04 0x45 0x45 0x45 0x45 - 0x1c 0x68 0x00 0x00 0x40 0x00 0x00 0x00 ....EEEE.h..@...
	0x82000040  0x50 0x45 0x00 0x00 0xc2 0x01 0x02 0x00 - 0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x00 PE..............
	0x82000050  0x00 0x00 0x00 0x00 0x90 0x00 0x06 0x03 - 0x0b 0x01 0x02 0x14 0x00 0x20 0x86 0x00 ............. ..
	0x82000060  0x00 0x14 0x00 0x00 0x00 0x00 0x00 0x00 - 0x51 0x25 0x00 0x00 0x00 0x10 0x00 0x00 ........Q%......
	0x82000070  0x00 0x30 0x86 0x00 0x00 0x00 0x00 0x00 - 0x00 0x10 0x00 0x00 0x00 0x02 0x00 0x00 .0..............
	0x82000080  0x00 0x00 0x00 0x00 0x01 0x00 0x00 0x00 - 0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x00 ................
	0x82000090  0x00 0x44 0x86 0x00 0x00 0x10 0x00 0x00 - 0x00 0x00 0x00 0x00 0x0a 0x00 0x00 0x00 .D..............
	0x820000a0  0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x00 - 0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x00 ................
	0x820000b0  0x00 0x00 0x00 0x00 0x06 0x00 0x00 0x00 - 0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x00 ................
	0x820000c0  0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x00 - 0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x00 ................
	0x820000d0  0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x00 - 0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x00 ................
	0x820000e0  0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x00 - 0x2e 0x74 0x65 0x78 0x74 0x00 0x00 0x00 .........text...
	0x820000f0  0x00 0x20 0x86 0x00 0x00 0x10 0x00 0x00 - 0x00 0x20 0x86 0x00 0x00 0x10 0x00 0x00 . ....... ......
