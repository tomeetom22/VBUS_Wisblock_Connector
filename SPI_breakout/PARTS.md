# SPI_breakout Parts

Parts summary generated from the current KiCad schematic and PCB files.

## BOM

| Qty | Ref | Part / description | Footprint | Notes |
| ---: | --- | --- | --- | --- |
| 1 | U1 | TXGA FBB04004-F40S1003K6M 40-pin WisBlock IO board connector | `MyParts:TXGA_FBB04004-F40S1003K6M` | SMD connector on bottom side. |
| 1 | U2 | Adafruit 4682 MicroSD breakout | `MyParts:Adafruit4682` | Through-hole 1x9 header footprint for the breakout. |
| 1 | U2 header | 1x9, 2.54 mm, 90 degree male-to-male pin header set | fits `MyParts:Adafruit4682` | Added for mounting the Adafruit 4682 at right angle. Breakaway right-angle male header is OK if cut to 9 pins. |
| 2 | J1, J2 | Phoenix Contact 1984659, 6-position 3.81 mm pluggable PCB header | `MyParts:CONN_1984659_PXC` | OPC-N3 A/B connectors. Matching plug noted on schematic: TJ0631030000G or equivalent. |
| 1 | J3 | Phoenix Contact 1935161, 2-position external OPC 5 V input terminal | `MyParts:CONN_1935161_PXC` | Feeds only the OPC 5 V rail plus common return. |
| 2 | GAS1, GAS2 | Phoenix Contact 1984633, 4-position 3.81 mm pluggable PCB header | `MyParts:CONN_1984633_PXC` | Gas/I2C-style terminal blocks. |
| 3 | H1, H3, H4 | 1.5 mm mounting holes | `RAK_IO_FIX_SMALL` | Board mounting features. |
| 2 | H2, H5 | 2.8 mm mounting holes | `RAK_IO_FIX_LARGE` | Board mounting features. |

## Connector Pinouts

### J1 - OPC-N3 A

| Pin | Signal | Wire color from schematic note |
| ---: | --- | --- |
| 1 | `GND` | black |
| 2 | `OPC_5V_EXT` | red |
| 3 | `OPC1_CS` / SS | green |
| 4 | `SPI_MOSI` / SDI | blue |
| 5 | `SPI_MISO` / SDO | white |
| 6 | `SPI_CLK` / SCK | yellow |

### J2 - OPC-N3 B

| Pin | Signal | Wire color from schematic note |
| ---: | --- | --- |
| 1 | `GND` | black |
| 2 | `OPC_5V_EXT` | red |
| 3 | `OPC2_CS` / SS | green |
| 4 | `SPI_MOSI` / SDI | blue |
| 5 | `SPI_MISO` / SDO | white |
| 6 | `SPI_CLK` / SCK | yellow |

### J3 - External OPC 5 V Input

| Pin | Signal |
| ---: | --- |
| 1 | `OPC_5V_EXT` |
| 2 | `GND` |

### U2 - Adafruit 4682 MicroSD Breakout

| Pin | Signal |
| ---: | --- |
| 1 | `3v3` from WisBlock IO pin 18 |
| 2 | `GND` |
| 3 | `SPI_CLK` |
| 4 | `SPI_MISO` / D0/SO |
| 5 | `SPI_MOSI` / D1/CMD |
| 6 | `SD_CS` |
| 7 | DAT1, unconnected |
| 8 | DAT2, unconnected |
| 9 | DET, unconnected |

### GAS1

| Pin | Signal | WisBlock IO pin |
| ---: | --- | ---: |
| 1 | `VCC` / 3.3 V | 17 |
| 2 | `GND` | common ground |
| 3 | `SCL1` | 20 |
| 4 | `SDA1` | 19 |

### GAS2

| Pin | Signal | WisBlock IO pin |
| ---: | --- | ---: |
| 1 | `VCC` / 3.3 V | 5 |
| 2 | `GND` | common ground |
| 3 | `SCL2` | 36 |
| 4 | `SDA2` | 35 |

## Assembly Notes

- Supply OPC 5 V through `J3`; do not expect the WisBlock 3.3 V rail to power OPC-N3 sensors.
- Verify the OPC SPI interface is 3.3 V logic compatible before applying external 5 V power.
- Use a 1x9 right-angle male header for the Adafruit 4682 so the breakout mates with the `U2` through-hole footprint.
