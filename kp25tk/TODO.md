KP25TK To-do List
=================

Schematic:
- Remove diodes on COL outputs; they're not necessary
- Move row input diodes up under row inputs so that it's more clear
  what they're protecting.
- Change Host and WW connectors to 26-pin.
- Add 6 holes for M3 screws for stand-offs: 4 at corners of keypad, 2 at top.
- Add LED for power indicator. Try with 1K5 resistor so it's not too bright.
- Add diode to each key on matrix. Can they be optional?

Board Layout:
- Shrink board width so that keypad takes up entire width of board.
  - Slide switch and reset button move to right side above keypad.
  - Single-row headers for power, /ROWSEL, etc. have about 1.5 cm of
    ground fill (no components) with wire-wrap connector centred above.
    (This is to leave a comfortable space for routing the wire-wrapping.)
- Generate export of board layout image to `kp25tk-pcb.pdf`
