romext-2364
===========

This board has a 2764 EPROM socket (typically ZIF) into which you plug your
own EPROMs, and a DIP-24W socket that connects via a IDC cable to a 2364
ROM socket inside the machine, effectively "inserting" your 2764 EPROM into
the 2364 ROM socket. (The cable between the two allows you to have the
board outside the system case so that you can easily access it to change
EPROMS.)

#### Datasheets

- [NEC NMOS μPD2364][nec2364] Read Only Memory 8192 words, 8 bits/word.
- [ST Microelectronics M2764A][st2764] NMOS 64 Kbit (8Kb × 8) UV EPROM.

### Design Notes

This is designed to work (but is as yet untested) when connected to the
expansion ROM socket of an original [NEC PC-8001]. There are several
assumptions made that cannot yet be changed in board configuration (though
the board should be updated to be configurable for these).

1. The 2764 `VPP` pin 1 and `P̅G̅M̅` pin 27 should both be held at `Vcc` for
   all non-programming operations.

2. The 2364 `C̅S̅` pin 20 is designed for mask ROMs programmed with negative
   logic enable.




<!-------------------------------------------------------------------->
[NEC PC-8001]: https://github.com/0cjs/sedoc/tree/main/8bit/nec/8001
[st2764]: https://downloads.reactivemicro.com/Electronics/ROM/2764%20EPROM.pdf
[nec2364]: https://archive.org/details/nec-upd2364-datasheet/mode/1up?view=theater
