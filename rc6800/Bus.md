RC6800 Bus
==========

The RC6800 bus is a variant of the [RC6502 bus], which in turn is based on
the minimal version of the [RC2014 bus].

There was some discussion about this in the 6502.org forums under the topic
[A(nother) proposed RC2014-style bus pinout][f6 6730].


### Physical Specification

The boards use a 39-pin single row of 0.05" square header pins on 0.1"
centers; this fits within a length of 99.106 mm, to stay under a 10 cm
limit. The backplane uses the corresponding female connectors, generally
spaced at about 1–1.5 cm intervals. Typically backplanes use a standard
40-pin vertical connector and the boards a right-angle connector to stand
vertically in the backplane; both these connectors are very cheap.

Optionally, the board and/or backplane may use the full 40-pin connector;
this will require soldering the connection from the 40th pin to the other
backplane connectors on the backplane and to wherever it needs to go on
a board.

The dimensions are the same as the [RC2014 dimensions][14dim] for
single-row cards and backplanes, and the backplanes are physically
compatible. However, the signals, though similar, are not compatible.

### Signal Specification

The signals are quite similar to the RC6502 bus, and boards for that bus
should be usable with little modification. For easy comparision, signals
different on the RC2014 and RC6502 bus are included at the left of the
table below, and signals differing from RC6502 to RC6800 are marked with a
`•`.

    RC2014 RC6502│ RC6800  Pin CPU Periph Notes
    ─────────────┼────────────────────────────────────────────────────────────
                 │    A15   1  out in
                 │      …
                 │     A0  16  out in
                 │    GND  17  in  in
                 │    Vcc  18  in  in
    /M1    Φ2out │•    Φ2  19  in  in   System clock; usu. gen by CPU board
                 │ /RESET  20  IO  in
    CLK     Φ0in │• Φ2aux  21 (IO  in)  Compatibility copy of Φ2
    /INT    /IRQ │   /IRQ  22  in  in   Pull-up req'd; usu. by CPU board
    /MREQ  Φ1out │•     ×  23  ×   ×    Unused
    /WR      R/W̅ │    R/W̅  24  out in
    /RD      RDY │    RDY  25  in  out
    /IORQ   SYNC │   SYNC  26  out in
                 │     D0  27  ↔   ↔
                 │      …
                 │     D7  34  ↔   ↔
    TX,TX2    TX │• /SEL0  35  ×   ×    "Small" select 0: typ. 256b-4K @$C000
    RX,TX2    RX │• /SEL1  36  out in   "Small" select 1: typ. 256b-4K @$D000
    USER1   /NMI │   /NMI  37  in  out  Pull-up req'd; usu. by CPU board
    USER2      × │• /SEL2  38  out in   "Medium" select: typ. 8k @$E000
    USER3      × │• /SEL3  39  out in   "Large" select, typ. 16k @$8000
    USER4,IEO  × │• /SEL4  40  out in   Optional

The primary differences from RC6502 are:
- 19-`Φ2out` is renamed to `Φ2` and becomes the sole clock on the
  backplane. All boards but one use this as clock input; one board
  (typically the CPU board) generates it.
- 21-`Φ0in`/`CLK` is replaced with 21-`Φ2aux`. It's normally unused in
  RC6800 systems, but may be jumpered to 19-`Φ2out` for compatibility.
- 23 `Φ1out` is removed and becomes unused; it does not appear to be used
  by any RC6502 boards except the SBC, for which it's only an output.
- 35-`TX` and 36-`RX` lines are removed from the bus. Serial communciation
  across the backplane is rare, many systems don't do serial at all, and
  systems that do can put the serial connector(s) on the appropriate cards.
- 35-`TX` ⇒ `×` unused and available for re-allocation.
- 36-`RX` ⇒ `/SEL0`, the "small" select (see below).
- 38-`×` ⇒ `SEL2` (incompatible with RC6502 TIA, RIOT).
- 39-`×` ⇒ `SEL3` (incompatible with RC6502 TIA, RIOT, ROM).

To convert an RC6502 "Apple 1" SBC to RC6800 bus:
- Minimal changes:
  - Cut 35-`TX` and 36-`RX` traces to the bus connector.
  - Connect '138 `Y̅4` pin 11 to 35-`SEL0` ($C000-$CFFF external peripherals).
  - Connect `CS_PIA` ('138 `Y̅5` pin 10) to 36-`/SEL1`
    (on-board disable to use $D000-$DFFF for external peripherals)
- Optional changes:
  - Connect `CS_ROM` to 38-`/SEL2`
    (on-board disable to use external ROM at $E000-$FFFF).
  - Connect '138 `Y̅0`–`Y̅3` pins 15-12 to quad-input AND, output to 39-`/SEL3`
    (for external memory in $8000-$BFFF range).

### RC2014 Incompatibility Notes

There are some unfortunate assignments in RC6502 that we maintain to
decrease the cost of tweaking RC2014 boards to be compatible, though it
makes boards less RC2014 compatible. These include:

- 19-`Φ2out`: Equivalant of 21-`CLK` on RC2014, though the optional `Φ2aux`
  duplication of `Φ2` mitigates this.
- 26-`SYNC`: Should really be on the equivalant pin for RC2014, 19-`/M1`.
  This would allow for `/IORQ` to be an external select, logically the same
  function.
- 37-`/NMI` on RC6502 `USER1` is why we don't have a `SEL1`.



<!-------------------------------------------------------------------->
[14dim]: https://smallcomputercentral.com/documentation/circuit-board-dimensions-rc2014-standard/
[RC2014 bus]: https://smallcomputercentral.wordpress.com/documentation/specification-rc2014-bus/
[RC6502 bus]: https://github.com/tebl/RC6502-Apple-1-Replica/blob/master/Bus.md
[f6 6730]: http://forum.6502.org/viewtopic.php?f=4&t=6730
