# Beluga Battery Temperature Tracking (BBTT)

ESPHome-based monitor for eight independent NTC temperature channels.

## Hardware

- Two Olimex **ESP32-POE-ISO-EA** boards (ESP32-WROOM-32UE, 4 MB flash, external antenna), each with Ethernet/IEEE 802.3af PoE
- One Soldered ADS1115 module per Olimex board, connected to that board's UEXT I²C bus
- Both ADS1115 modules use address `0x48`; the address is reused because the boards have separate I²C buses
- I²C on each Olimex UEXT: SDA GPIO13, SCL GPIO16
- Four NTC channels per board, for eight channels total
- Eight independent dividers: `3.3 V → 10 kΩ fixed resistor → ADC node → NTC → GND`

See the [software guide](software/README.md), [wiring diagram](assets/battery-temperature-wiring.svg), and [KiCad carrier-board project](pcb/ADS1115/README.md). The KiCad design represents one node; use two identical carrier boards for the complete eight-channel system.

The NTC calibration currently assumes **10 kΩ at 25 °C**. Verify that reference and the beta value against the actual thermistor datasheet before relying on temperature readings. The exact board variant is selected; physical board revision and pin labels will be checked when it arrives.
