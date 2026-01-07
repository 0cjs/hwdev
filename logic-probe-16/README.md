logic-probe-16: 16-channel logic probe
======================================

Test Design
-----------

Todo:
- Lay out PCB for 2 channels.
- Add a way to replace the LED current limiting resistors. (test points?)
  - Add though-hole pads in parallel; cut trace to disable SMD resistor.
- Add reference voltage generator. (SMD switch from [parts
  library][jlc-parts]?)
- Connector for power supply.
- [BOM format][jlc-pcba-bom]


Reference Voltages
------------------

The low/high reference voltages are provided by one of four pairs of
voltage dividers selected via a 2P4T slide switch. The preset
configurations are below, but the fourth set may be replaced by cutting the
traces from the SMD resistors and inserting through-hole resistors into the
pads that parallel them.


    Sw│ Logic    │VRlow VRhigh│
    ──┼──────────┼────────────┼──────────────────────────────────────────────
    1 │ TTL-IN   │ 0.8   2.0  │
    2 │ CMOS-IN  │ 1.5   3.5  │
    3 │ TTL-OUT  │ 0.4   2.4  │
    4 │ CMOS-OUT │ 0.5   4.44 │


Parts
-----

### LM339 Comparitors

From the following options, we've chosen LM339LVDR (SOIC-14):

    LM339LVDR     SOIC-14     qty  191     ¥21
    LM339LVPWR    TSSOP-14    qty 2392     ¥45
    LMV339IPWR    TSSOP-14    qty 1941     ¥73
    LMV339IDR     SOIC-14     qty 2497    ¥114

### LEDs

1206 LEDs (KiCad footprint `LED_1206_3216Metric`).

- Green: [XL-3216UGC-FB]
  - Vf = 2.8 V @ 10 mA; use 220R.
- Red: [XL-3216SURC-FB]
  - Vf = 1.9 V @ 10 mA; use 330R


<!-------------------------------------------------------------------->
[jlc-pcba-bom]: https://jlcpcb.com/help/article/bill-of-materials-for-pcb-assembly
[jlc-parts]: https://jlcpcb.com/parts/all-electronic-components

[XL-3216SURC-FB]: https://jlcpcb.com/api/file/downloadByFileSystemAccessId/8589949748750299136
[XL-3216UGC-FB]: https://jlcpcb.com/api/file/downloadByFileSystemAccessId/8589836680988557312
