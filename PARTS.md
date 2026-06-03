# SPI_breakout Parts List

This is the working parts list for `SPI_breakout/`.

| Qty | Ref | Part | Notes |
| ---: | --- | --- | --- |
| 1 | `U1` | TXGA `FBB04004-F40S1003K6M` or matching WisBlock 40-pin board-to-board connector | Footprint: `MyParts:TXGA_FBB04004-F40S1003K6M`. Verify exact mating height against the RAK base board. |
| 1 | `U2` | Adafruit 4682 microSD SPI/SDIO breakout | Powered from WisBlock 3.3 V. |
| 2 | `J1`, `J2` | 6-position 3.81 mm pluggable terminal header | Footprint uses Phoenix MC style: `PhoenixContact_MC_1,5_6-G-3.81_1x06_P3.81mm_Horizontal`. Intended for Digi-Key `TJ0631030000G`-style 6-position plug plus compatible PCB header. |
| 1 | `J3` | 2-position 3.81 mm pluggable terminal header | External 5 V input for OPC power. Use compatible plug/header pair. |
| 2 | External | AlphaSense OPC-N3 | Connected through `J1` and `J2`; not mounted directly to PCB. |
| 1 | External | Regulated 5 V supply for OPC-N3 sensors | Connects to `J3`; return ground is common with WisBlock ground. |
| 1 | External | RAK WisBlock base / IO host | Provides 3.3 V logic, SPI bus, and GPIO chip-select lines. |

## Terminal Pinout

`J1` and `J2` use the same pin order:

| Pin | Wire color | Signal |
| ---: | --- | --- |
| 1 | Black | GND |
| 2 | Red | External 5 V |
| 3 | Green | SS / chip select |
| 4 | Blue | SDI / MOSI |
| 5 | White | SDO / MISO |
| 6 | Yellow | SCK |

`J3` pinout:

| Pin | Signal |
| ---: | --- |
| 1 | External 5 V |
| 2 | GND |

## Procurement Notes

- `TJ0631030000G` is a 6-position 3.81 mm pluggable terminal plug. Pair it with
  a compatible 3.81 mm PCB header matching the selected footprint and desired
  wire-entry direction.
- The OPC-N3 is powered from 5 V, but the board assumes SPI logic is safe for
  WisBlock 3.3 V pins. Confirm the OPC wiring/adapter does not return 5 V logic
  before fabrication.
