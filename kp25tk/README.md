KP25TK: 25-key Hex Keyboard
===========================

For ease of reference, this repository includes some files generated from
the `kicad/` files. __Regenerate these if you change the KiCad source.__
- [`kp25tk-sch.pdf`]: Schematic, generated via `File » Plot…` to PDF output.
- [`kp25tk-pcb.pdf`]: PCB, generated via File » Print…, enabling all layers,
  setting to portrait format and then printing to a file.

Contents:
- [Description](#description)
  - [Connections](#connections)
- [Components](#components)
  - [Key Swiches](#key-switches)
- [Theory of Operation](#theory-of-operation)
  - [Power Inputs](#power-inputs)
  - [Row Select Inputs](#row-select-inputs)
  - [Column Outputs](#column-outputs)
  - [NMI, Reset and Slide Switch Outputs](#nmi-reset-and-slide-switch-outputs)


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
  - 1 slide switch for a user-selected function

### Connections

The board is connected to the scanning device via a 2 row × 13 column
.1"-pitch male header ("Host") for a ribbon cable IDC female connector. The
header is unshrouded to allow the use of smaller than 13 column connectors
when the full 26 pins are not needed.

The connector pin layout is configured via wire-wrapping or jumpering
individual 1-row header pins for each function group to a second 2×14
header ("Config") that parallels the first. This allows setting the ribbon
cable connector pinout to match a wide variety of scanning devices. The
function groups are:
- Power input: 2:GND, 1:+5V
- Row select input: 3:row 2, 2:row 1, 1:row 0
- Column output: 8:column 7, ..., 1:column 0
- NMI key output: 2:/NMI (negative logic), 1:NMI (positive logic)
- RESET button output: 2:/RESET (negative logic), 1:RESET (positive logic)
- Slide switch output:
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


Theory of Operation
-------------------

The 2×14 pin __Host__ header is connected via ribbon cable to the host that
will scan the keypad. Not all 26 pins need be used, but to help avoid pin
interference and misconnections it's best to use a 26-pin IDC connector on
the keypad end of the cable, even if a ribbon cable with fewer conductors
is used.

The 2×14 __Config__ header pins are connected to the same-numbered Host
connector pins; the builder needs to connect these to the function group
pins below; wire-wrap is the most reliable way of doing this, but jumper
wires with "dupont"-style female connectors may also be used.

#### Power Inputs

GND and +5V must be supplied from the host. +5V is used to power the
Schmitt inverters that debounce and invert the NMI key, RESET button
and slide switch. If only the keypad (less NMI key) is used, +5V is
not necessary.

#### Row Select Inputs

Each of the three row select inputs connects through the cathode of a
signal diode to the coresponding row line for the keypad. When scanning,
each row in turn is driven low by the host, which then scans that row using
the column outputs (see below). The diodes allow the host to pull a row
low, but prevent the host from driving a row high. The diodes prevent short
circuits between the row select inputs (which are likely to damage the
host) when two keys in a column, one on a grounded row and one on a high
row, are pressed simultaneously.

#### Column Outputs

XXX

#### NMI, Reset and Slide Switch Outputs

XXX



<!-------------------------------------------------------------------->
[`kp25tk-pcb.pdf`]: ./kp25tk-pcb.pdf
[`kp25tk-sch.pdf`]: ./kp25tk-sch.pdf

[NEC TK-85]: https://gitlab.com/retroabandon/tk80-re
[kailh]: https://www.adafruit.com/product/4958
[keysw]: https://www.aliexpress.com/item/1005004285423123.html
