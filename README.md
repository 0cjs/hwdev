hwdev: cjs's Hardware Development Repo
======================================

__NOTE:__ Please run the top-level `Test` script before committing to
ensure that all automatically-generated renders are up to date. (Some
projects may still have manually-generated renders you need to generate
yourself.)

General Files:
- `kicad-version`: The version of KiCad used in the commit.
- `kicad-lib`: Contains the `0cjs-hwdev` KiCad symbol library, mainly
  vintage parts.

Projects:
- [`drom`]: Ideas for a diode-ROM board.
- [`kp25tk`]: Configurable 25-key parallel keypad, with additional reset
  button and slide switch. The primary use is as a hex keypad interface to
  trainer boards, but it can serve a wide variety of purposes.
- [`rc6800`]: Variant of the RC6502 bus design, which is in turn a variant of
  the RC2014 bus.
- [`romext-2364`]: A 2764 EPROM in this board outside the system is
  connected via a cable as a 2364 ROM in a socket in the system.
- [`tk85`]: Projects for the NEC TK-85 trainer board.

### Project Files and Subdirectories

Projects generally include the following files and subdirectories:
- `REAMDE.md`: A description of the project.
- `vis/`: Original diagrams, photos and other illustrations and
  visualisations of the project.
- `kicad/`: The KiCad design files (`.kicad_pro`, `.kicad_sch`, etc.).
- `render/`: Renderings from the KiCad design files, e.g., a PDF version of
  the schematic. These are either generated by the top-level `Test` script
  or created manually.

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
[`romext-2364`]: ./romext-2364/
[`tk85`]: ./tk85/
