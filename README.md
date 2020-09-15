### mdevx tool

The tool called mdevx was developed in Year 2008 for Coldfire 7475 silicon.

https://www.rockbox.org/wiki/pub/Main/DataSheets/CFPRM.pdf

#### Purpose

mdevx is a simple command-line program used to read/write to hardware locations in a 32 bit
armv7 A5, A8, A9, A12, A15 MMU based silicons. It is a standalnoe tool.

It it reccommended for the 32 bit MMU class of silicon: armv7, ColdFire and similar
architectures.

It does similar functionality/job as the Open Source tool: devmem2

https://github.com/hackndev/tools/blob/master/devmem2.c

#### INTEL tool for similar purpose

I strongly reccomment for x86 and x86_64 architecture the following tool:

https://github.com/Intel-ADSC/samtool

#### Compiling and linking for armv7 architectures (target platforms):

	gcc mdevx.c -o mdevx

#### Usage

Usage is very simple - help included in the tool

	# mdevx

	--------------------------------------------------------------------------------
	 Mdevx Read/Write Utility
	 Copyright (c) 2008 - 2020 Zoran Stojsavljevic. All rights reserved.
	--------------------------------------------------------------------------------
	 Usage: ./mdevx <addr> <data>       (for writing tool expects 2, 4 or 8 nibbles exact)
	 Usage: ./mdevx <addr> <format> <#> (for # (default 1) reading, format is b/h/w)
	 Usage: ./mdevx <addr>              (interractive mode, 8 nibbles exact for <addr>)
	 Interractive mode: <offset> <data/format> (description for data/format above)
	 Interractive mode: <cr>          (repeat previous format by entering <cr>)
	--------------------------------------------------------------------------------

#### Example

I reccommend to test the tool browsing the BBB memory:

	line 13:	memory@80000000 {
	line 14:		device_type = "memory";
	line 15:		reg = <0x80000000 0x10000000>; /* 256 MB */
	line 16:	}

Issuing the command:

	./mdevx 0x80000000 w
	32 bit data read [0x80000000] = 0xbe0a1eeb
