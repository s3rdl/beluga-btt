# Olimex ESP32-POE-ISO-EA footprint

The project-local footprint is `Beluga_Battery.pretty/Olimex_ESP32-POE-ISO-EA.kicad_mod` and is registered in KiCad as part of the existing `Beluga_Battery` project footprint library.

## Geometry basis

The outline and two 1x10 extension-header hole rows are transcribed from Olimex's official ESP32-POE-ISO hardware repository, Rev.N fabrication data:

- Board outline: 28.000 x 98.150 mm, including the small side notch.
- Two header rows: 10 holes each, 2.54 mm pitch, 25.40 mm row spacing.
- Finished hole nominal: 1.00 mm (based on the official plated drill file).
- Pad numbering is currently provisional and unique for a single footprint: 1–10 on EXT1, 11–20 on EXT2, each read from the board's top end to its bottom end. The schematic symbol must preserve this mapping or explicitly revise it when the board pinout is added.

Source: <https://github.com/OLIMEX/ESP32-POE-ISO/tree/master/HARDWARE/ESP32-PoE-ISO-Rev.N/Gerbers>

## EA variant check

The order is for ESP32-POE-ISO-EA, whose module uses an external antenna connector. The footprint uses the Rev.N board geometry and header drill positions. Before manufacturing a carrier PCB, compare the delivered EA board against the footprint in KiCad's 3D viewer and measure the header and outline dimensions; this project copy is intended for mechanical placement and still needs that physical confirmation.

A simple 3D envelope model is assigned for board-size visualization. It is a plain 28 x 98.15 x 1.6 mm board block, not a component-accurate render of the PoE board.

The matching schematic symbol `Olimex_ESP32-POE-ISO-EA` is in `Beluga_Battery.kicad_sym`. It represents EXT1 and EXT2 as separate ten-pin rows. Signal names were assigned from the official Rev.N schematic and pinout diagram. The footprint pad numbers are 1–10 for EXT1 and 11–20 for EXT2; the EXT2 footprint numbers 11–20 correspond to connector positions 1–10 respectively.


## Extension header pin map

| Header pin | EXT1 signal | Header pin | EXT2 signal |
| ---: | --- | ---: | --- |
| 1 | GND | 1 | GPIO39 / PWR_SENSE |
| 2 | +3V3 | 2 | GPIO36 / U1RXD |
| 3 | +5V | 3 | GPIO35 / BAT_M |
| 4 | ESP_EN | 4 | GPIO34 / BUT1 |
| 5 | GPIO0 | 5 | GPIO33 |
| 6 | GPIO1 / U0TXD | 6 | GPIO32 |
| 7 | GPIO2 / HS2_DATA0 | 7 | GPIO16 / I2C_SCL |
| 8 | GPIO3 / U0RXD | 8 | GPIO15 / HS2_CMD |
| 9 | GPIO4 / U1TXD | 9 | GPIO14 / HS2_CLK |
| 10 | GPIO5 / SPI_CS | 10 | GPIO13 / I2C_SDA |

The I2C signals shown in the pinout are GPIO16 for SCL and GPIO13 for SDA. GPIO39, GPIO35 and GPIO34 also have on-board power-sense, battery-measurement and button connections; check the Olimex documentation before repurposing them. The EA hardware variant and delivered revision still need a visual/pinout check against the physical board.
