# VBUS WisBlock Connector KiCad Projects

This repository contains the KiCad PCB project for the VBUS WisBlock SPI breakout board.

## Projects

| Directory | Purpose | Current state |
| --- | --- | --- |
| `SPI_breakout/` | WisBlock SPI breakout for an Adafruit 4682 MicroSD reader, two AlphaSense OPC-N3 particle counters, and two gas/I2C-style terminal ports. | Schematic and routed PCB are present, with local custom symbols, footprints, 3D models, and updated docs. |

## SPI_breakout Overview

`SPI_breakout` is a KiCad 10 project using local project libraries under `SPI_breakout/MyParts.pretty/`.

The board connects the WisBlock 40-pin IO connector to:

- one Adafruit 4682 MicroSD breakout on the shared SPI bus
- two AlphaSense OPC-N3 screw-terminal ports on the shared SPI bus
- two 4-position gas/I2C-style screw terminals
- a separate external 5 V input for OPC-N3 sensor power

The project-level files live in `SPI_breakout/`:

- `SPI_breakout.kicad_pro` - KiCad project file
- `SPI_breakout.kicad_sch` - schematic
- `SPI_breakout.kicad_pcb` - routed PCB
- `fp-lib-table` and `sym-lib-table` - project library tables
- `MyParts.pretty/` - local symbols and footprints
- `1935161.stp`, `1984633.stp`, `1984659.stp`, `4682 Micro SD Breakout.step` - local 3D models
- `PARTS.md` - current BOM and connector pinout summary

## Parts

See [`SPI_breakout/PARTS.md`](SPI_breakout/PARTS.md) for the current parts list, connector pinouts, and assembly notes.

| Qty | Ref | Function | Footprint |
| ---: | --- | --- | --- |
| 1 | `U1` | WisBlock 40-pin IO board connector, TXGA FBB04004-F40S1003K6M | `MyParts:TXGA_FBB04004-F40S1003K6M` |
| 1 | `U2` | Adafruit 4682 MicroSD breakout | `MyParts:Adafruit4682` |
| 1 | `U2 header` | 1x9, 2.54 mm, 90 degree male-to-male pin header set for Adafruit 4682 | fits `MyParts:Adafruit4682` |
| 2 | `J1`, `J2` | OPC-N3 6-position 3.81 mm pluggable PCB headers, Phoenix Contact 1984659 | `MyParts:CONN_1984659_PXC` |
| 1 | `J3` | External OPC 5 V input terminal, Phoenix Contact 1935161 | `MyParts:CONN_1935161_PXC` |
| 2 | `GAS1`, `GAS2` | 4-position 3.81 mm pluggable PCB headers, Phoenix Contact 1984633 | `MyParts:CONN_1984633_PXC` |

## Main Connections

| Function | WisBlock IO pin | Net |
| --- | ---: | --- |
| MicroSD chip select | 25 | `SD_CS` |
| SPI clock | 26 | `SPI_CLK` |
| SPI MISO | 27 | `SPI_MISO` |
| SPI MOSI | 28 | `SPI_MOSI` |
| OPC-N3 A chip select | 29 | `OPC1_CS` |
| OPC-N3 B chip select | 31 | `OPC2_CS` |
| MicroSD 3.3 V | 18 | `Net-(U2-3v3)` |
| GAS1 3.3 V / SDA / SCL | 17 / 19 / 20 | `Net-(GAS1-VCC)` / `Net-(GAS1-SDA)` / `Net-(GAS1-SCL)` |
| GAS2 3.3 V / SDA / SCL | 5 / 35 / 36 | `Net-(GAS2-VCC)` / `Net-(GAS2-SDA)` / `Net-(GAS2-SCL)` |
| Ground | 40 plus shared returns | `GND` |

## OPC and Power Notes

OPC-N3 sensor 5 V is supplied externally through `J3`; it is not tied to the WisBlock 3.3 V rail. `J3` pin 1 feeds `OPC_5V_EXT` for `J1`/`J2` pin 2, and `J3` pin 2 ties the OPC supply return to WisBlock ground.

The SPI nets are 3.3 V logic. The schematic notes do not include a level shifter, so the WisBlock IO connector, MicroSD breakout, and OPC SPI adapter are expected to be 3.3 V logic compatible before applying external 5 V OPC power.

OPC terminal pin order from the schematic:

| Terminal pin | OPC wire | Signal |
| ---: | --- | --- |
| 1 | Black | `GND` |
| 2 | Red | `OPC_5V_EXT` |
| 3 | Green | SS / chip select |
| 4 | Blue | SDI / `SPI_MOSI` |
| 5 | White | SDO / `SPI_MISO` |
| 6 | Yellow | SCK / `SPI_CLK` |

## Opening the Project

1. Open `SPI_breakout/SPI_breakout.kicad_pro` in KiCad.
2. For `SPI_breakout`, keep the project-relative library paths intact:
   - `SPI_breakout/fp-lib-table`
   - `SPI_breakout/sym-lib-table`
   - `SPI_breakout/MyParts.pretty/`
3. After schematic changes, run **Update PCB from Schematic** in KiCad before adjusting placement or routing.
4. Press `B` in PCB Editor to refill copper zones if the ground pour only shows as a dashed outline.

## Before Fabrication

- Verify physical fit against the RAK base board and any enclosure.
- Confirm terminal access direction and wire service clearance.
- Run KiCad GUI ERC/DRC.
- Refill zones and inspect `GND` pour clearances.
- Generate fresh Gerbers and drill files from the final board.
