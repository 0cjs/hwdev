hwdev: cjs's Hardware Development Repo
======================================

__Note for developers:__ Please run `Test` before committing. But also:
- In order to avoid a lot of spurious changes on dev branches, `Test` will
  update renders only if you are commiting to the `main` branch. On dev
  branches, if you want to see/test renders, just use the `render` script
  directly. Ensure you bring commits to `main` and run `Test` there before
  pushing them and test them on `main`.
- If you see massive changes to renders in all projects when you run
  `Test`, you are probably running a different version of KiCad than
  normally used with this repo. Check the `kicad-version` file for changes
  and, if lower, upgrade, or if higher, discuss with the repo owner.

#### General Files and Directores

- `Test`: Top-level test script.
- `bin/`: Support programs and scripts.
  - `bin/render`: Given paths to `.kicad_sch` and `.kicad_pcb` files,
    renders PDF and Gerber versions of these to a `render/` dir at the same
    level as the directory containing the sch/pcb files. Run `bin/render -h`
    for more details.
  - `bin/ghdl-url`: Given a path to a file in a repo, return the fully
    resolved (i.e., not HTTP 302 redirects) GitHub download URL for it.
    (See the header comments in the file for more detail.)
- `kicad-version`: The version of KiCad used in the commit.
- `kicad-lib`: Contains the `0cjs-hwdev` KiCad symbol library, mainly
  vintage parts.

#### Project Directories

- [`drom`]: Ideas for a diode-ROM board.
- [`floppy-head-protector`]: PCB-only project to build a template to cut
  out cardboard head travel protectors for 5.25" floppy diskette drives.
- [`kp25tk`]: Configurable 25-key parallel keypad, with additional reset
  button and slide switch. The primary use is as a hex keypad interface to
  trainer boards, but it can serve a wide variety of purposes.
- [`rc6800`]: Variant of the RC6502 bus design, which is in turn a variant of
  the RC2014 bus.
- [`romread-adapter`]: Interface to allow reading of ROMs via the 8265
  PPI GPIO interface on a SuperAKI-80 SBC (or other similar system).
- [`romext-2364`]: A 2764 EPROM in this board outside the system is
  connected via a cable as a 2364 ROM in a socket in the system.
- [`tk85`]: Projects for the NEC TK-85 trainer board.


Project Files and Subdirectories
--------------------------------

Projects generally include the following files and subdirectories:
- `REAMDE.md`: A description of the project.
- `vis/`: Original diagrams, photos and other illustrations and
  visualisations of the project.
- `kicad/`: The KiCad design files (`.kicad_pro`, `.kicad_sch`, etc.).
- `render/`: Renderings from the KiCad design files, including a PDF of the
  schematic and Gerbers of the board design. These are usually generated
  via the top-level `render` script but may be created manually; see above.
  (Also see below about viewing board designs.)

### Revision Numbers

Project use revisions int the format _S-B_ where _S_ is the schematic
revision and _B_ is the board revision within that scheamtic. Either number
being suffixed with `dev` indicates that it's a development revision
heading towards that next revision, e.g., `2dev-3` is development towards
revision `2` of the schematic. (Revision `0` is always considered a
development revision towards revision `1`.)

The revision number is separately set and updated (in _File » Page
Settings…_) for the schematic and PCB. If you change the schematic from `2`
to `3` you will then manually need to change the PCB to from `2-3` to
`3-4dev`.

Release versions are tagged in Git with the project name and release,
separated by an underscore, e.g. `kp25tk_2-1`.

### Viewing Board Designs

The `$PROJECT/render/$PROJECT-jlcpcb.zip` contains a subset of Gerbers
designed for upload to [JLCPCB]. This file also serves as a way to view the
board design without having to load up KiCad: you can upload this file to
any of several web-based Gerber viewers. These include:

* __[PCBWay viewer]:__ Press the '+' button and upload the ZIP. Provides
  Gerber layers and 2D top and bottom board views. Saves the uploads so you
  need not upload again if you come back later.

* __[JLCPCB viewer]:__ Requires sign-in. Click "Add gerber file" at the
  middle left of the top page (or at the top of the order page) and then
  click "Gerber Viewer" at the lower right just below the small renders.
  provides Gerber layers and both 2D top/bottom 3D board renders.

* __[NextPCB viewer]:__ Click "Upload Files" and upload the ZIP. Provides
  Gerber layer views, error checking, and a PDF report download that
  includes 2D top/ bottom board views.

* __[Tracespace viewer]:__ An [open source][tracespace] viewer that can
  accept URLs to ZIP files of Gerbers. (This has been re-used by several
  PCB house websites and others.) Unfortunately, it does not follow HTTP
  302 redirects, so it doesn't work with "raw download" links from GitHub;
  you need to convert them to the `githubusercontent.com` link with
  the `bin/ghdl-url` program. Also, unfortunately, there seems to be no way
  to specify the download URL in a query string to have it automatically
  load the data by clicking on a link.

Viewers confirmed not to work with the ZIP file include the __[Altium 365
viewer].__ (These may work by uploading the individual files under the
`render/$PROJECT-gerber/` directory.)



<!-------------------------------------------------------------------->
[`drom`]: ./drom/
[`floppy-head-protector`]: ./floppy-head-protector/
[`kp25tk`]: ./kp25tk/
[`rc6800`]: ./rc6800/
[`romread-adapter`]: ./romread-adapter/
[`romext-2364`]: ./romext-2364/
[`tk85`]: ./tk85/

[Altium 365 viewer]: https://www.altium.com/viewer/
[JLCPCB viewer]: https://cart.jlcpcb.com/quote
[JLCPCB]: https://jlcpcb.com/
[NextPCB viewer]: https://www.nextpcb.com/free-online-gerber-viewer.html
[PCBWay viewer]: https://www.pcbway.com/project/OnlineGerberViewer.html
[Tracespace viewer]: https://tracespace.io/view/
[tracespace]: https://github.com/tracespace/tracespace
