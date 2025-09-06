KP25TK: 25-key Hex Keyboard
===========================

For ease of reference, this repository includes a `render/` subdirectory
containing some files generated from the `kicad/` files. __Bump version
numbers and regenerate these if you change the KiCad source.__
- [`kp25tk-sch.pdf`]: Schematic, generated via `File » Plot…` to PDF output.
- [`kp25tk-pcb.pdf`]: PCB, generated via File » Print…, enabling all layers,
  setting to portrait format and then printing to a file.
- [`kp25tk-render.png`]: PCB 3D render.
- [`kp25tk-silkscreen.png`]:
- [`kp25tk-tracks.png`]:

Contents:
- [Description](#description)
  - [Connections](#connections)
- [Components](#components)
  - [Key Swiches](#key-switches)
- [Theory of Operation](#theory-of-operation)
  - [Power Inputs](#power-inputs)
  - [Keypad Matrix](#keypad-matrix)
  - [NMI, Reset and Slide Switch Outputs](#nmi-reset-and-slide-switch-outputs)


Description
-----------

This 25-key 5×5 keyboard is similar to that on the [NEC TK-85].
with the following features.

* Power: +5V Vcc and GND from host system
* Matrix:
  - 3 row select input: ground one row to select
  - 8 row column output: rows pulled up to Vcc and pulled down by switch
  - 4×4 layout for hex digits `0` to `F`
  - 4 keys at the top and 4 keys at the right for commands
* 3 independent debounced switches:
  - Debounce time can be configured by changing a resistor value (see below).
  - All 3 provide positive and negative logic outputs.
  - 1 key at upper right hand corner of switch array for NMI interrupt
  - 1 tactile switch for RESET
  - 1 slide switch for a user-selected function

### Connections

The board is connected to the scanning device via a 2 row × 13 column
.1"-pitch male header ("Host"), usually used with a ribbon cable and IDC
female connector. The header is unshrouded to allow the use of smaller than
13 column connectors when the full 26 pins are not needed. (Though we
suggest _always_ using a 26P connector on the keypad side and simply using
a narrower ribbon cable that does not take up the full width of the
connector.)

The connector pin layout is configured via wire-wrapping or jumpering (with
"dupont" female-female jumper wires) individual 1-row header pins for each
function group to a second 2×13 header ("Config") that parallels the first.
This allows setting the ribbon cable connector pinout to match a wide
variety of scanning devices. The function groups are:

* Power input: 2:GND, 1:+5V
* Row select input: 3:row 2, 2:row 1, 1:row 0
* Column output: 8:column 7, ..., 1:column 0
* NMI key output: 2:/NMI (negative logic), 1:NMI (positive logic)
* RESET button output: 2:/RESET (negative logic), 1:RESET (positive logic)
* Slide switch output:
  -   UP: 2=low, 1=high
  - DOWN: 2=high, 1=low

### Design Goals

- Be fully configurable so it can work with almost any system.
- Do configuration in a way that it can be changed later.
- Provide a full hex keypad with additional keys for functions and some
  debounced switches for direct (non-scanned) functions.
- Have a physical layout that parallels the logical layout in order to make
  tracing the operation (whether for understanding or debugging) as easy as
  possible, even for novices to electronics.
- Use only through-hole parts to make soldering and modification as easy as
  possible.
- Be as compact as reasonably possible while meeting the other goals above.


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

Component Summary:
- C[123], R[123], D[123], U1: Debounce and inversion circuitry for the
  `NMI` key, `RESET` button, and slide `SWITCH`, respectively.
- R[45], D[45]: Current-limiting resistors and LEDs for `PWR` (Vcc/GND
  present) and `SWLED` (slide switch ON) indicators, respectively.

### Host Connector and Configuration Headers

The 2×13 pin __Host__ header is connected to the host that will scan the
keypad. This is typically done with a ribbon cable with IDC connectors on
each end. Not all 26 pins need be used, but to help avoid pin interference
and misconnections it's best to use a 26-pin IDC connector on the keypad
end of the cable, even if a ribbon cable with fewer conductors is used.

The 2×13 __Config__ header pins are connected to the same-numbered Host
header pins; the builder needs to connect these to the function group
pins below. Wire-wrap is the most reliable way of doing this, but jumper
wires with "dupont"-style female connectors may also be used.

### Power Inputs

GND and +5V must be supplied from the host. (This is normally supplied
on the Host header, and then wired from the Config header to the `VCC_IN`
and `GND_IN` header pins.)

+5V is used to power the Schmitt inverters that debounce and invert the NMI
key, RESET button and slide switch. If only the keypad (less NMI key) is
used, +5V is not necessary.

A current-limiting resistor R4 (1K5) and power LED D4 indicate that the
board is powered. The current-limiting resistor is relatively high value so
that the LED isn't distractingly bright.

### Keypad Matrix

Each of the three row select inputs, `ROWSEL0`, `ROWSEL1`, `ROWSEL2`
connects to the corresponding row line for the keypad. When scanning, each
row in turn is driven low by the host, which then scans that row using the
column outputs.

Each column line is pulled high by its pull-up resistor when no key in that
column is pressed; this will read as `1` on the corresponding column output.
When a key in a _non-scanned_ row (row high or floating) is pressed, this
does not change. When a key in the _scanned_ row is pressed, the low row
sinks current through the diode and the closed keyswitch, bringing the
column low.

#### Keypad Pull-up Resistor Values

The keypad relies on the ability of the host driving the row select inputs
to to pull them down to 0 V, and do so reasonably quickly. Host drivers'
ability to do this varies, and the additional capacitance introduced by the
wiring connecting the keypad to the host, and the keys themselves, can
degrade this.

The 22K values chosen for the resistors give a fairly weak pull-up that's
still higher than other designs without a cable between the keyboard row
drivers and column inputs of the host. The resistor pack is in a socket to
allow changing the value.

If you are getting intermittent or no response from pressing keys, the
pull-up is too strong and you should try a _higher_ resistance, such as 33K
or 47K.

If you are getting lengthened or repeated key presses, the pull-up is too
weak and you should try a _lower_ resistance, such as 10K or 4K7.

#### Keypad Matrix Diodes

The diodes in the matrix serve two purposes.

First, they prevent shorts between the row inputs that would likely damage
the device driving them. Without the diodes, two switches held down on the
same column when one row is low short the low row to the high row.
(Consider, e.g., low row 0 → closed `KEY00` → pulls column 0 low → closed
`KEY08` pulls row 1 low. This is blocked by the diodes between `KEY08` and
row 1.) In keyboards without a diode on each key in the matrix, a diode on
each row line (cathode toward the row select input) would be required
instead.

Second, They prevent "ghost keys" when two keys in one column and one key
in the same row as either are pressed. Without the diodes, If `KEY00`,
`KEY01` and `KEY08` are pressed and `ROWSEL1` is low:
1. Low row 1 connects to column 0 via closed `KEY08`, pulling column 0 low.
2. Low column 0 connects to row 0 via closed `KEY00`, pulling row 0 low.
3. Low row 0 connects to column 1 via closed `KEY01`, pulling column 1 low.
4. The system scanning row 1 sees column 1 low, though `KEY09` is not
   pressed.
The diode between `KEY00` and row 0 prevents the connection in point 2.

Schottky diodes are used instead of silicon diodes to reduce the forward
voltage drop. These _must_ be used for reliability, as the maximum ouput
voltage for a 5V TTL low signal is 0.5 V, and a silicon diode would exceed
this, not bringing the column lines low enough to meet that specification.

This is still out of spec for 5V CMOS output voltage, but well within the
1.5 V maximum that CMOS parts must accept at input as a low voltage.

### NMI, Reset and Slide Switch Outputs

#### Debounce

The upper-right key on the keypad is the "NMI" button. Above the keypad and
to the right is a momentary-contact button, "Reset." Above the keypad and
to the left is a slide switch, "switch" (which may also be called "single
step"). These all provide debounced positive and negative signals separate
from the key matrix. They may be used for any purpose; the names merely
represent typical uses.

Each is debounced via a resistor-capacitor pair, with a diode for
overvoltage protection. R1/C1/D1 are used for NMI; R2/C2/D2 for Reset, and
R3/C3/D3 for the switch. All three operate in exactly the same way, though
you may vary the resistor values for each to give different debounce time
constants if needed. (This will normally not be necessary.)

The debounce works as follows.

When the switch is open (not pressed), the input to the 74x14 Schmitt
trigger gate is raised to +5 V through resistor R1. However, reaching +5 V
will be delayed because C2 must be charged first. (The rate of charge is
[standard for an RC circuit][rc].) Once the charge on R1 has reached the
Vt+ level of the '14 (typically ~1.6 V), the positive logic output of the
switch will go low and the negative logic output of the switch will go
high.

When the switch is pressed, it will short the '14 input to ground, quickly
making it fall below the Vt- level of the '14 (typically ~0.8 V) and the
positive logic output of the switch will switch to high and the negative
logic output will switch to low. This state will remain until the switch is
released, at which point the capacitor will start charging through the RC
circuit as above, eventually reaching the point where the positive logic
output goes low and the negative logic output goes high.

The time constant of R1×C1 determines how long the switch will take to be
released; with the specified 10 kΩ resistor and 0.1 μF capacitor, this will
be about 1 millisecond. The resistor can be changed to a higher value (e.g.
100 kΩ = 10 ms) or a lower value (e.g., 1 kΩ = 100 μs) if necessary.

The Schottky diode (typically 1N5819 or 1N5817) from the gate input to the
+5 V source ensures that the voltage at the gate input never exceeds the
power supply Vcc level plus the forward voltage drop of 0.3 V.

#### Switch LED

The switch additionally has an LED that is illuminated when it's active (in
the "up" position) and extinguished when it's inactive (in the "down"
position).

A current-limiting resistor R5 (1K5) and power LED D5 indicate when the
slide switch is active (up). The current-limiting resistor is relatively
high value so that the LED isn't distractingly bright and to minimise
load on the U1F buffer output.

We avoid placing the LED on the switch side of U1F to avoid any
interference with the debounce logic. That leaves the options of the
postive-logic U1F output and the negative-logic U1E output: we choose the
latter because LS logic is much better at sinking current than sourcing it.



<!-------------------------------------------------------------------->
[`kp25tk-pcb.pdf`]: ./render/kp25tk-pcb.pdf
[`kp25tk-render.png`]: ./render/kp25tk-render.png
[`kp25tk-sch.pdf`]: ./render/kp25tk-sch.pdf
[`kp25tk-silkscreen.png`]: ./render/kp25tk-silkscreen.png
[`kp25tk-tracks.png`]: ./render/kp25tk-tracks.png

[NEC TK-85]: https://gitlab.com/retroabandon/tk80-re
[kailh]: https://www.adafruit.com/product/4958
[keysw]: https://www.aliexpress.com/item/1005004285423123.html

[rc]: https://en.wikipedia.org/wiki/Rc_circuit
