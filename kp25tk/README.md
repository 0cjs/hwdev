KP25TK: 25-key Hex Keyboard
===========================

__NOTE:__ `kp25tk.pdf` is the `File » Plot…` to PDF output of the
schematic; make sure you regenerate this if you modify the schematic.


Description
-----------

This 25-key 5×5 keyboard is similar to that on the [NEC TK-85].
with the following features:

- Power:
  - +5V Vcc, GND
- Matrix:
  - 3 row select input: ground one row to select
  - 8 row column output: rows pulled up to Vcc and pulled down by switch
  - 4×4 layout for hex digits `0` to `F`
  - 4 keys at the top and 4 keys at the right for commands
- 3 independent debounced switches:
  - All 3 provide positive and negative logic outputs
  - 1 key at upper right hand corner of switch array for NMI interrupt
  - 1 tactile switch for RESET
  - 1 slide switch for mode indication

### Connections

The board is connected to the scanning device via a 2 row × 10 column
.1"-pitch male header for a ribbon cable IDC female connector. The header
is unshrouded to allow the use of smaller than 10 column connectors when
the full 20 pins are not needed.

The connector pin layout is configured via wire-wrapping individual 1-row
header pins for each function group to a second 2×10 header that parallels
the first. This allows setting the ribbon cable connector pinout to match
a wide variety of scanning devices. The function groups are:
- Power input: 2:GND, 1:+5V
- Row select input: 3:row 2, 2:row 1, 1:row 0
- Column output: 8:column 7, ..., 1:column 0
- NMI key output: 2:/NMI (negative logic), 1:NMI (positive logic)
- RESET button output: 2:/RESET (negative logic), 1:RESET (positive logic)
- Mode switch output:
  -   UP: 2=low, 1=high
  - DOWN: 2=high, 1=low


Components
----------

### Key Switches

Currently planned are [Outemu "Tactile Brown"][keysw] (AliExpress) key
switches, which are (allegedly) compatible with Cherry MX and "CIY"
sockets.

These [Kailh switch sockets][kailh] (Adafruit) for MX-compatible mechanical
keys may be useful. They still need mechanical support, but the Adafruit
description says that a bit of hot glue or epoxy will do the trick.



<!-------------------------------------------------------------------->
[NEC TK-85]: https://gitlab.com/retroabandon/tk80-re
[keysw]: https://www.aliexpress.com/item/1005004285423123.html
[kailh]: https://www.adafruit.com/product/4958
