
## Summary

This project is an SOC (System on a Chip) coded in VHDL and implemented for the Lattice iCE40-hx8k dev board. The SOC contains the following components: 9995 CPU + UART + Timer + I/O Ports

## Required Hardware

* Lattice iCE40-hx8k dev board (can be ordered online at www.latticesemi.com)
* USB-to-Serial 3.3V adapter (can be ordered from eBay)
* misc USB cables and wires for connecting the USB-to-Serial adapter

NOTE: Make sure the USB-to-serial adapter is a 3.3V version. Some adapters have 5V interface signals which could damage your iCE40-hx8k dev board.

## Tools

* IceCube2 (from Lattice Semiconductor) was used for synthesis and FPGA Routing.
* Icestorm (https:/github.com/cliffordwolf/icestorm) was used for programming.


## Build Flow

I used the Lattice IceCube2 software to generate the SOC_bitmap.bin programming file and then I used this command line "iceprog SOC_bitmap.bin" the program the iCE40-hx8k dev board over the USB cable (iceprog is part of the icestorm tool suite).

## Console Interface

I used the minicom program (on Ubuntu Linux) as a console to communicate with the 9995 SOC over the USB-to-Serial connection. Configure minicom using the command line "minicom -s" to configure the serial port for ttyUSB0 and turn of the hardware handshaking. There are probably other alternatives to minicom. Any ANSI terminal-emulator program should work for this application.

## Pinout

The iCE40 pins are defined as follows:
```
UART_RXD   G1 -pullup yes
UART_TXD   G2
PORTA[0]   B5
PORTA[1]   B4
PORTA[2]   A2
PORTA[3]   A1
PORTA[4]   C5
PORTA[5]   C4
PORTA[6]   B3
PORTA[7]   C3
RESET      N3 -pullup yes
CLK        J3 -pullup yes
```

## Memory Map

The memory map of this SOC is as follows:
```
0000 -> 1fff   8k bytes of RAM
f000 -> f004   UART  registers
f008 -> f00a   Timer registers
f00c           Output register
f00e           Interrupt mask register
f010 -> f012   Random number generator
f014           Interrupt source register
```

## 9995 CPU Background Info

The TMS 9995 was a second generation of the TMS 9900. The TMS 9995 was code-compatible with the older TMS 9900 and it included 4 new instructions: LWP, LST, MPYS, and DIVS. Like the TMS 9900, the TMS 9995 was derived from the TI 990 16-bit minicomputer. It had a 32k word (64k byte) address space and two internal 16-bit registers (PC and WP). The 16x16-bit workspace registers resided in memory and the workspace register pointed to the base address of the workspace registers in RAM. There was no direct hardware support for saving state on a stack for context switching. Instead, when a subroutine is called or an interrupt is processed, context is switched by updating the workspace register. This made context switches relatively efficient.


## Contributors

* Scott L Baker - SOC design

## License

See the **LICENSE** file in this repository
