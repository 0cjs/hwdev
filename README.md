hwdev: cjs's Hardware Development Repo
======================================

General Files:
- `kicad-lib`: Contains the `0cjs-hwdev` KiCad symbol library, mainly
  vintage parts.

Projects:
- [`drom`]: Ideas for a diode-ROM board.
- [`kp25tk`]: Configurable 25-key parallel keypad, with additional reset
  button and slide switch. The primary use is as a hex keypad interface to
  trainer boards, but it can serve a wide variety of purposes.
- [`rc6800`]: Variant of the RC6502 bus design, which is in turn a variant of
  the RC2014 bus.
- [`tk85`]: Projects for the NEC TK-85 trainer board.

### Revision Numbers

Project use revisions int the format _S.B_ where _S_ is the schematic
revision and _B_ is the board revision within that scheamtic. Either number
being suffixed with `dev` or `devN` indicates that it's a development
version heading towards that next revision, e.g., `2dev3` is development
towards revision `2`. (Revision `0` is always considered a devleopment
revision towards revision `1`.)

The version number is set and needs to be updated separately (in _File »
Page Settings…_) for the schematic and PCB; if you change the schematic
from `2` to `3` you will then manually need to change the PCB to from `2.3`
to `3.1`.

Release versions are tagged in Git with the project name and release, e.g.
`kp25tk-2.1`.



<!-------------------------------------------------------------------->
[`drom`]: ./drom/
[`kp25tk`]: ./kp25tk/
[`rc6800`]: ./rc6800/
[`tk85`]: ./tk85/
