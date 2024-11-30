TK-85 FT245 USB FIFO
====================

Color codes:

    b0  Black       y4  Yellow      g8  Grey
    b1  Brown       g5  Green       w9  White
    r2  Red         b6  Blue
    o3  Orange      v7  Violet

### 74LS138 3 to 8 line decoder/demux

The output of this select must still be qualified with `/RD` or `/WR`;
the selection signal itself 'overflows' into the next cycle.

      o3 IOa3  A14•AB11 │ A    •‖  Vcc │
      y4 IOa4  A13•AB12 │ B     ‖   Y0 │
      g5 IOa5  A12•AB13 │ C     ‖   Y1 │
      v7 IOa7  A10•AB15 │ /G2A  ‖   Y2 │
      b6 IOa6  A11•AB14 │ /G2B  ‖   Y3 │
               B9=IO/!M │ G1    ‖   Y4 │
                        │ Y7    ‖   Y5 │
                        │ GND   ‖   Y6 │

### 74LS573 Octal D-type Transparent Latches 3-state:

- LE: 0=Qs latched; 1=Q follows D

                        /OE •‖  Vcc
                         1D  ‖  1Q
                         2D  ‖  2Q
                         3D  ‖  3Q
                         4D  ‖  4Q
                         5D  ‖  5Q
                         6D  ‖  6Q
                         7D  ‖  7Q
                         8D  ‖  8Q
                        GND  ‖  LE

### System Bus

See [[re/doc/hw]].

                         GND │ B1   A1      (B=bottom A=Top)
                             │ B2   A2
                         +5V │ B3   A3
                             ⋯
          ??             /RD │      A8
          o3 b6   IO/!M  /WR │ B9   A9
          __ v7     AB7 AB15 │ B10 A10
          __ b6     AB6 AB14 │ B11 A11
          __ g5     AB5 AB13 │ B12 A12
          __ y4     AB4 AB12 │ B13 A13
          __ o3     AB3 AB11 │ B14 A14
                    AB2 AB10 │ B15 A15
                    AB1  AB9 │ B16 A16
                    AB0  AB8 │ B17 A17
                             ⋯
                       /MEMR │ B20
                       /MEMW │ B21
                       READY |     A22
                             ⋯
                 /HLDA /HOLD │ B25 A25
                         DB7 │ B26
                         DB6 │ B27
                         DB5 │ B28
                         DB4 │ B29
                         DB3 │ B30
                         DB2 │ B31
                         DB1 │ B32
                         DB0 │ B33
                             ⋯
                             │ A50 B50

TK-85 Test Programs
-------------------

Set $8009 to port to read.

        8000 00         nop
        8001 …
        8007 00         nop
        8008 DB F9      in   $F9            ; port B data (pins A30-A37)
        800A CD 77 06   call nybbles
        800D 22 CF 83   ld (char8),hl
        8010 CD 6C 05   call dispchars
        8013 C3 08 80   jp   $8008


Current Notes
-------------

Ports $08-$0F seem to activate Y0, not Y1. $10 activates Y2.



<!-------------------------------------------------------------------->
[re/doc/hw]: https://gitlab.com/retroabandon/tk80-re/-/blob/main/doc/hardware.md
