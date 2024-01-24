Jumperable Diode ROM
====================

An 8×_n_ diode ROM consists of 8 columns for data output, each with a
pull-up. Connected to these are an input for each row, only one of which
is pulled low at any time. (The others are high or left floating.)

When a row is pulled low to read the address each data output column that is
_unconnected_ to that row will give a `1` output. Each data output column
that is _connected_ to a row via a diode with anode on on the column and
cathode on the row will let the row input pull the column low (against the
pull-up), giving a `0` output.

It's probably reasonable to connect the column outputs to inverting HCT
buffers to give the outputs lots of drive capability, including into CMOS.
Using inverting buffers also means that switches or jumpers in series with
the diodes will make represent a `1` bit when on and a `0` bit when off,
which is a bit more intuitive.

### Anode Recovery

Diodes have parasitic capacitance that must be charged after level switch.
An anode recovery system applies current to the the data output lines
connected to the diode anodes in order to ensure that any lines that had
been pulled low through a diode in the previously selected row are brought
back high again before the next memory read.

The retrocomp.com Diode ROM Book does this via a BC557 transistor sourcing
current from Vcc through a 47 ohm resistor to a group of 1N4148 anodes,
each with its cathode connected to a data line. There seems to be a slight
delay (via R/C network) on the signal going to the base of the transistor.

### References

- Wikipedia, [Diode matrix][wp].
- EESE, [Understanding Diode ROM][eese 505304]. Includes sample schematic.
- retrocmp.com, [Diode ROM Book][drbook]. Detailed description of diode ROM
  boards for a PDP-11 (UNIBUS or QBUS interface). It also includes LEDs
  indicating the last address accessed and the data value output.
- VCF Forum, [PDP-11 Diode ROM][vcf 1246504]. Discussion of the above, and
  some other links.
- Triumph Adler TA 10/1 accounting machine "programmable" diode ROM board
  pics (uses SN74154N demuxes): [[ta10-107]], [[ta10-109]]


Address Decoding
----------------

#### Address Blocks

8-address blocks with 74'138 decoders: `A0`…`A2` on `A`…`C`.
- Two blocks with extra enable free for entire ROM.
- Three blocks without extra enable, but no extra gates.
- Four blocks with one inverter, leaving extra enable free.

Layout:

     G1 /G2A /G2B
     --  A4  A3      %xxx00aaa   $0000-$0007  block 0
     A3  A4  --      %xxx01aaa   $0008-$000F  block 1
     A4  A3  --      %xxx10aaa   $0010-$0018  block 2

     G1 /G2A /G2B
    /A3  A4  --      %xxx00aaa   $0000-$0007  block 0
     A3  A4  --      %xxx01aaa   $0008-$000F  block 1
     A4  A3  --      %xxx10aaa   $0010-$0018  block 2
     A4 /A3  --      %xxx11aaa   $0018-$001F  block 3

16-address blocks with 74'154 decoders: `A0`…`A3` on `A`…`D`. One block
leaves 2 negative-logic enables free; 4 blocks can be done with the
addition of two inverters. The latter requires a separate method of ROM
disable, perhaps on the output buffers.

    /G1 /G2
     A4  A5          %xx00aaaa   $0000-$0007  block 0
    /A4  A5          %xx01aaaa   $0008-$000F  block 1
     A4 /A5          %xx10aaaa   $0010-$0018  block 2
    /A4 /A5          %xx11aaaa   $0018-$001F  block 3

#### Full Device Enable/Disable

Perhaps a 74'688 8-bit magnitude comparator. `A8`…`A15` to P input,
pull-ups on `Q` inputs with jumpers/switches to ground them.

This restricts the ROM to a 256-byte space, with the blocks mirrored
through it.

### Datasheets and Inventory Stock

* __'138__ (16) 3→8 decoder/demux, inverting out [SN74LS138], [SN74HCT138]
  - Pins: `A B C G̅2̅A̅ G̅2̅B̅ G1 Y̅7 GND ‥ Y̅6 Y̅5 Y̅4 Y̅3 Y̅2 Y̅1 Y̅0 Vcc`
  - enable: G1, G̅2̅A̅, G̅2̅B̅  (disabled: all outputs high)
  - input: A (LSB), B, C
  - output: one of Y0...Y7 low: ABC=0→Y0, ABC=1→Y1, ..., ABC=7→Y7

* __'154__ (24): 4→16 decoder/demux [SN74154]
  - Pins: `0 1 2 3 4 5 6 7 8 9 10 GND ‥ 11 12 13 14 15 G̅1̅ G̅2̅ D C B A Vcc`
  - BCDA = low → 0 = low, 1-15 high.

* __'159__ (24W): 4→16 decoder/demux, inverting out, open collector [SN74159]
  - Pins: `0 1 2 3 4 5 6 7 8 9 10 GND ‥ 11 12 13 14 15 G̅1 G̅2 D C B A Vcc`

* __'688__ (20): 8-bit comparator `=` w/output enable [SN74SL682]
  - Pins: `G̅ P0 Q0 P1 Q1 P2 Q2 P3 Q3 GND ‥ P4 Q4 P5 Q5 P6 Q6 P7 Q7 P̅=̅Q̅ Vcc`

Inventory (cjs):

     Qt  Item           Desc
    -----------------------------------------------------------------
      7  74LS138        '138 3- to 8-line demux, address latch, inverting
      5  SN74LS154N     '154 4→16 decoder DIP-24N
      9  DM74LS154N     '154 4→16 decoder DIP-24W
     10  SN74HCT138N    '138 3-to-8 line demux, address latch, inverting
     10  SN74LS688N     '688 8-bit magnitude comparators



<!-------------------------------------------------------------------->
[drbook]: https://www.retrocmp.com/projects/diode-rom-book
[eese 505304]: https://electronics.stackexchange.com/q/505304/15390
[ta10-107]: http://www.horniger.de/computer/ta/TA101_107.jpg
[ta10-109]: http://www.horniger.de/computer/ta/TA101_109.jpg
[vcf 1246504]: https://forum.vcfed.org/index.php?threads/pdp-11-diode-rom.1246504/
[wp]: https://en.wikipedia.org/wiki/Diode_matrix

[SN74LS138]: http://www.ti.com/lit/gpn/sn74ls138
[SN74HCT138]: http://www.ti.com/lit/gpn/sn74hct138
[SN74154]: https://web.archive.org/web/20150321045049/http://www.ti.com/lit/ds/symlink/sn74154.pdf
[SN74159]: https://web.archive.org/web/20070102021404/http://focus.ti.com/lit/ds/symlink/sn74159.pdf
[SN74SL682]: https://www.ti.com/lit/ds/symlink/sn74ls682.pdf
