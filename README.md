# VBUS WisBlock Connector KiCad Projects

This repository contains KiCad PCB projects for VBUS board work. The active
project is `SPI_breakout/`; `SensorBoard_v1.5/` is kept as a routed reference
design.

## Projects

| Directory | Purpose | Current state |
| --- | --- | --- |
| `SPI_breakout/` | WisBlock SPI breakout for an Adafruit 4682 microSD reader and two AlphaSense OPC-N3 particle counters. | Schematic updated and first-pass compact 2-layer PCB layout routed. |
| `SensorBoard_v1.5/` | Existing sensor board design used as a layout/manufacturing reference. | Completed KiCad project with routed PCB and generated Gerbers. |

## SPI_breakout Overview

`SPI_breakout` is a KiCad 10 project using local project libraries under
`SPI_breakout/MyParts.pretty/`.

The board connects the WisBlock 40-pin IO connector to:

- One Adafruit 4682 microSD breakout on the shared SPI bus.
- Two AlphaSense OPC-N3 sensors on the same SPI bus.
- A separate external 5 V input for the OPC-N3 sensors.

The current PCB is a compact first-pass layout:

- 2 copper layers.
- Board outline: `35 mm x 45 mm`.
- RAK WisBlock IO template connector and mounting-hole locations retained.
- Both OPC 6-position terminal blocks placed inside the original `35 mm x 25 mm`
  RAK template boundary.
- microSD breakout rotated and placed just below the template area.

## Main Parts

See [PARTS.md](PARTS.md) for the current parts list.

| Ref | Function | Symbol | Footprint |
| --- | --- | --- | --- |
| `U1` | WisBlock 40-pin IO connector | `MyParts:M40S1003K6M` | `MyParts:TXGA_FBB04004-F40S1003K6M` |
| `U2` | Adafruit 4682 microSD breakout | `MyParts:Adafruit_MicroSD` | `MyParts:Adafruit4682` |
| `J1` | OPC-N3 A terminal | `MyParts:AlphaSense_OPC_N3` | `Connector_Phoenix_MC:PhoenixContact_MC_1,5_6-G-3.81_1x06_P3.81mm_Horizontal` |
| `J2` | OPC-N3 B terminal | `MyParts:AlphaSense_OPC_N3` | `Connector_Phoenix_MC:PhoenixContact_MC_1,5_6-G-3.81_1x06_P3.81mm_Horizontal` |
| `J3` | External OPC 5 V input | `MyParts:External_5V_Input` | `Connector_Phoenix_MC:PhoenixContact_MC_1,5_2-G-3.81_1x02_P3.81mm_Horizontal` |

## SPI and Power Mapping

| Net | Connected devices |
| --- | --- |
| `/SPI_CLK` | `U1.26`, `U2.3`, `J1.6`, `J2.6` |
| `/SPI_MISO` | `U1.27`, `U2.4`, `J1.5`, `J2.5` |
| `/SPI_MOSI` | `U1.28`, `U2.5`, `J1.4`, `J2.4` |
| `/SD_CS` | `U1.25`, `U2.6` |
| `/OPC1_CS` | `U1.29` / WisBlock `IO1`, `J1.3` |
| `/OPC2_CS` | `U1.31` / WisBlock `IO3`, `J2.3` |
| `Net-(U2-3v3)` | `U1.18`, `U2.1` |
| `/OPC_5V_EXT` | `J3.1`, `J1.2`, `J2.2` |
| `/GND` | WisBlock GND, microSD GND, both OPC GND terminals, external 5 V return |

OPC terminal pin order follows `VBUS_Comms/OPC_Wiring.txt`:

| Terminal pin | OPC wire | Signal |
| --- | --- | --- |
| 1 | Black | GND |
| 2 | Red | External 5 V |
| 3 | Green | SS / chip select |
| 4 | Blue | SDI / MOSI |
| 5 | White | SDO / MISO |
| 6 | Yellow | SCK |

## Voltage Notes

- Adafruit 4682 is powered from WisBlock 3.3 V.
- OPC-N3 sensor power is supplied from the separate 5 V input at `J3`; it is not
  tied to WisBlock 3.3 V.
- The SPI logic is currently routed directly to WisBlock 3.3 V logic. Before
  fabrication, confirm the exact OPC-N3 cable/adapter electrical interface does
  not drive SPI lines at 5 V.

## Validation Notes

The compact PCB layout has been checked with a local geometry audit for:

- Trace-to-pad conflicts.
- Trace-to-trace conflicts.
- Via-to-pad and via-to-trace conflicts.
- Continuity on the routed non-GND nets.

KiCad Gerber export and 3D render both succeed. On this machine,
`kicad-cli pcb drc` exits without producing a useful report, so run DRC inside
the KiCad GUI before manufacturing.

## Opening the Projects

1. Open the desired `.kicad_pro` file in KiCad:
   - `SPI_breakout/SPI_breakout.kicad_pro`
   - `SensorBoard_v1.5/SensorBoard_v1.5.kicad_pro`
2. For `SPI_breakout`, keep the project-relative library paths intact:
   - `SPI_breakout/fp-lib-table`
   - `SPI_breakout/sym-lib-table`
   - `SPI_breakout/MyParts.pretty/`
3. After schematic changes, run **Update PCB from Schematic** in KiCad before
   adjusting placement or routing.
4. Press `B` in PCB Editor to refill copper zones if the ground pour only shows
   as a dashed outline.

## Before Fabrication

- Verify physical fit against the RAK base board and any enclosure.
- Confirm terminal access direction and wire service clearance.
- Run KiCad GUI ERC/DRC.
- Refill zones and inspect `/GND` pour clearances.
- Generate fresh Gerbers and drill files from the final board.
