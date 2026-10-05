# CMOS Inverter — TSMC 16nm FinFET (Cadence Virtuoso)

Custom layout of a CMOS inverter in TSMC N16 (16nm FinFET, ADFP), drawn in Cadence Virtuoso and verified with Siemens Calibre DRC and LVS.

The `inv_` folder is a Cadence library with two cells:

| Cell | Description | LVS | DRC |
|------|-------------|-----|-----|
| `INVTAP` | Inverter with well/substrate taps (final version) | ✅ CORRECT | 31 results, mostly implant (PP/NP), latch-up and density checks |
| `inv` | First inverter, without taps | ❌ INCORRECT (net count / connectivity) | 19 results |

## Schematic and layout

| Schematic | Layout |
|-----------|--------|
| ![INVTAP schematic](docs/images/INVTAP_schematic.png) | ![INVTAP layout](docs/images/INVTAP_layout.png) |

## Circuit

| Device | Type | L | Fins |
|--------|------|---|------|
| MM0 (pull-down) | `nch_svt_mac` | 16 nm | 4 |
| MM1 (pull-up) | `pch_svt_mac` | 16 nm | 4 |

Pins: `IN`, `OUT`, `VDD`, `GND`. The netlists are in [`netlists/`](netlists).

## Repository layout

```
.
├── inv_/                 Cadence library (open in Virtuoso)
│   ├── cdsinfo.tag
│   ├── data.dm
│   ├── inv/              schematic + layout (first version)
│   └── INVTAP/           schematic + layout (with taps, LVS clean)
├── netlists/             CDL netlists exported from the schematics
└── docs/images/          schematic and layout snapshots
```

## Opening the library

1. Set up the TSMC N16 ADFP PDK in your Virtuoso environment. The PDK is **not** included in this repository.
2. Add the library to your `cds.lib`:
   ```
   DEFINE inv_ /path/to/this/repo/inv_
   ```
3. In the Library Manager, attach `inv_` to the `tsmcN16` technology library if prompted.
4. Open `INVTAP → layout` or `INVTAP → schematic`.

## Running DRC and LVS

The Calibre run folders (`drc/`, `lvs/`, `svdb/`, `*.density`, reports) are excluded through `.gitignore` because Calibre regenerates them. To re-run:

- **DRC:** Calibre → Run nmDRC, using the N16 ADFP DRC deck.
- **LVS:** Calibre → Run nmLVS, using the N16 ADFP LVS deck with the schematic as the source.

## Tools

- Cadence Virtuoso ICADVM20.1
- Siemens Calibre nmDRC / nmLVS
- TSMC N16 ADFP PDK (academic access)
