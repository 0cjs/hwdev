romext-2364
===========

This board has a 2764 EPROM socket (typically ZIF) into which you plug your
own EPROMs, and a DIP-24W socket that connects via a IDC cable to a 2364
ROM socket inside the machine, effectively "inserting" your 2764 EPROM into
the 2364 ROM socket. (The cable between the two allows you to have the
board outside the system case so that you can easily access it to change
EPROMS.)

You will need a standard 0.1 μF bypass capacitor across the GND (pin 14)
and Vcc (pin 28) pins of the 2764; an axial capacitor is easily soldered
across the back of the board.

#### Datasheets

- [NEC NMOS μPD2364][nec2364] Read Only Memory 8192 words, 8 bits/word.
- [ST Microelectronics M2764A][st2764] NMOS 64 Kbit (8Kb × 8) UV EPROM.

### Design Notes

This has been tested to work with the expansion ROM socket of an original
[NEC PC-8001]. It may work with other systems as well, but there are some
assumptions made that cannot yet be changed in board configuration (though
the board should be updated to be configurable for these).

1. The 2764 `VPP` pin 1 and `P̅G̅M̅` pin 27 should both be held at `Vcc` for
   all non-programming operations.

2. The 2364 `C̅S̅` pin 20 is designed for mask ROMs programmed with negative
   logic enable.

### Non-project Settings

The following settings are important but not kept as part of the project
by KiCad.

* PCB grid is 2.54 mm for parts and 1.27 mm for traces.



<!-------------------------------------------------------------------->
[NEC PC-8001]: https://github.com/0cjs/sedoc/tree/main/8bit/nec/8001
[st2764]: https://downloads.reactivemicro.com/Electronics/ROM/2764%20EPROM.pdf
[nec2364]: https://archive.org/details/nec-upd2364-datasheet/mode/1up?view=theater
