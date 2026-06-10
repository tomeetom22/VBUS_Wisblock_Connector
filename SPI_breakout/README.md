# SPI_breakout

KiCad project for a WisBlock IO SPI breakout board. The board brings the WisBlock 40-pin IO connector out to:

- two AlphaSense OPC-N3 screw-terminal ports on the shared SPI bus
- one Adafruit 4682 MicroSD breakout footprint on the same SPI bus
- two 4-position gas/I2C-style screw terminals
- a separate 5 V input terminal for OPC-N3 sensor power

## Project Files

- `SPI_breakout.kicad_pro` - KiCad project file
- `SPI_breakout.kicad_sch` - schematic
- `SPI_breakout.kicad_pcb` - routed PCB
- `MyParts.pretty/` - local symbols and footprints
- `1935161.stp`, `1984633.stp`, `1984659.stp`, `4682 Micro SD Breakout.step` - local 3D models
- `SPI_breakout.zip` - existing board output archive
- `PARTS.md` - current parts and connector pinout summary

## Electrical Notes

OPC-N3 sensor 5 V is supplied externally through `J3`; it is not tied to the WisBlock 3.3 V rail. `J3` pin 1 feeds `OPC_5V_EXT` for `J1`/`J2` pin 2, and `J3` pin 2 ties the OPC supply return to WisBlock ground.

The SPI nets are 3.3 V logic. The schematic notes do not include a level shifter, so the WisBlock IO connector, MicroSD breakout, and OPC SPI adapter are expected to be 3.3 V logic compatible before applying external 5 V OPC power.

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
| Ground | 40 | `GND` |

See `PARTS.md` for connector part numbers, quantities, and pinouts.
