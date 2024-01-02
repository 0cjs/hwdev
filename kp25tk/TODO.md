KP25TK To-do List
=================

Schematic:
- Switch to Schottky diodes (1N519) in matrix to reduce voltage drop.
- Change discrete column pull-up resistors to 9-pin array; this results
  in a fairly large savings in board space. Use a single-row machine socket
  so that the pull-up resistors can be replaced with a different value if
  we see problems in operation w/different hosts, cable lengths, etc.
- Add diode to each key on matrix. Can they be optional?

Board Layout:
- Add 6 holes for M3 screws for stand-offs: 4 at corners of keypad, 2 at top.
