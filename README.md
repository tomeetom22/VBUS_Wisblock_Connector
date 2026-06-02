# SPI_breakout KiCad Projects

This repository contains KiCad PCB projects for VBUS board work. The active
project is `SPI_breakout/`; `SensorBoard_v1.5/` is kept in the repo as a
working reference design.

## Projects

| Directory | Purpose | Current state |
| --- | --- | --- |
| `SPI_breakout/` | Breakout board for bringing SPI signals from a 40-pin connector to an Adafruit microSD breakout footprint. | Schematic and local custom library are started; PCB file is present but effectively empty. |
| `SensorBoard_v1.5/` | Existing sensor board design used as a layout/manufacturing reference. | Completed KiCad project with routed PCB and generated Gerbers. |

## SPI_breakout Overview

`SPI_breakout` is a KiCad 10 project using local project libraries under
`SPI_breakout/MyParts.pretty/`.

Main schematic parts:

| Ref | Symbol | Footprint |
| --- | --- | --- |
| `U1` | `MyParts:M40S1003K6M` | `MyParts:TXGA_FBB04004-F40S1003K6M` |
| `U2` | `MyParts:Adafruit_MicroSD` | `MyParts:Adafruit4682` |

Relevant 40-pin connector signals from the custom symbol:

| Connector pin | Signal |
| --- | --- |
| 25 | `SPI_CS` |
| 26 | `SPI_CLK` |
| 27 | `SPI_MISO` |
| 28 | `SPI_MOSI` |
| 3, 4, 39, 40 | `GND` |
| 5, 17, 18 | `3v3` |
| 6 | `3v3_S` |

Adafruit microSD breakout pins in the custom symbol:

| Pin | Signal |
| --- | --- |
| 1 | `3v3` |
| 2 | `GND` |
| 3 | `CLK` |
| 4 | `D0_SO` |
| 5 | `D1_CMD` |
| 6 | `CS` |
| 7 | `DAT1` |
| 8 | `DAT2` |
| 9 | `DET` |

Before fabrication, confirm the schematic wiring for `CS`, `CLK`, `MISO`, and
`MOSI` against the target module pinout, then update the PCB from the schematic,
place footprints, route the board, and run DRC/ERC.

## Reference Design

Use `SensorBoard_v1.5/` as the reference for:

- Basic KiCad project organization.
- Board outline and mechanical-layout style.
- Connector labeling conventions on silkscreen.
- Gerber/drill output structure under `SensorBoard_v1.5/SensorBoard_v1.4_gerber/`.
- Manufacturer-ready output packaged as `SensorBoard_v1.5/SensorBoard_v1.5_gerber.zip`.

The reference board appears to use JST SH headers, Phoenix terminal blocks, M2
mounting holes, two copper layers, silkscreen pin labels, and generated Gerber
outputs.

## Opening the Projects

1. Open the desired `.kicad_pro` file in KiCad:
   - `SPI_breakout/SPI_breakout.kicad_pro`
   - `SensorBoard_v1.5/SensorBoard_v1.5.kicad_pro`
2. For `SPI_breakout`, keep the project-relative library path intact:
   - `SPI_breakout/fp-lib-table`
   - `SPI_breakout/MyParts.pretty/`
3. After schematic changes, run **Update PCB from Schematic** in KiCad before
   placing or routing the board.

## Notes and Caveats

- `SPI_breakout/SPI_breakout.kicad_pcb` currently only contains the KiCad PCB
  file header, so there is no routed SPI breakout layout yet.
- `SPI_breakout/fp-lib-table` contains multiple `MyParts` entries. If KiCad
  reports duplicate library names, clean this up before doing layout work.
- The TXGA footprint references a 3D model with an absolute local path:
  `/home/user/Downloads/M40S1003K6M.stp`. Move the model into the repo or update
  the footprint if a portable 3D view is needed.
- Generated fabrication files exist for `SensorBoard_v1.5`, but not yet for
  `SPI_breakout`.

## Suggested SPI_breakout Next Steps

1. Verify the schematic pin mapping between `U1` and `U2`.
2. Run ERC and resolve unconnected or mismatched pins.
3. Update the PCB from the schematic.
4. Place the 40-pin connector and microSD breakout footprint.
5. Add board outline, mounting holes, and silkscreen labels following the
   `SensorBoard_v1.5` style.
6. Route SPI, power, and ground.
7. Run DRC.
8. Generate Gerbers and drill files when the board is ready for fabrication.
