KP25TK To-do List
=================

Schematic:
- Change 74LS14 for 74HCT14. (Cannot use HC because Schmitt input range is
  where we want it on TTL, and not CMOS?) This makes sure that the NMI etc.
  outputs are CMOS-compatible. (Check input ranges for LS vs HC vs HCT.)
- Note 1N5819 diode type in README. Also note that 1N4148 has lower
  junction capacitance and much lower leakage current?
- Replace SWLED circuit: current one won't work due to weak outputs of 7414
  (much less than 7404!).
  - Resistor + diode from +5V → signal (parallel w/pull-up resistor); on
    when switch is up/closed.
  - Resistor/LED supplies pull-up current, so optionally make the regular
    pull-up resistor slightly larger (weaker pull-up) than the other
    non-LED circuits, giving same overall current when pulling up, and
    thus the same RC constant!
- Consider changing KB matrix decoupling resistor to same as debounce
  for easier BOM. (Decoupling value not terribly important.)
- Peer Review

Board Layout:
- Add 6 holes for M3 screws for stand-offs: 4 at corners of keypad, 2 at top
- Draw traces