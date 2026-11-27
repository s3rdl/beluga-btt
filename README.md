# Beluga Battery Temperature Tracking (BBTT)

ESPHome-based monitor for twelve independent NTC temperature channels.

## Hardware

- Three Olimex **ESP32-POE-ISO-EA** boards (ESP32-WROOM-32UE, 4 MB flash, external antenna), each with Ethernet/IEEE 802.3af PoE
- One Soldered ADS1115 module per Olimex board, connected to that board's UEXT I²C bus
- Both ADS1115 modules use address `0x48`; the address is reused because the boards have separate I²C buses
- I²C on each Olimex UEXT: SDA GPIO13, SCL GPIO16
- Four NTC channels per board, for twelve channels total
- Twelve independent dividers: `3.3 V → 10 kΩ fixed resistor → ADC node → NTC → GND`

See the [software guide](software/README.md), [wiring diagram](assets/battery-temperature-wiring.svg), and [KiCad carrier-board project](pcb/ADS1115/README.md). The KiCad design represents one node; use three identical carrier boards for the complete twelve-channel system.

The ordered Semitec **103JT-100** NTCs are specified as **10 kΩ at 25 °C** with a beta value of **3435 K**. The ESPHome configurations read the beta value from a local secret. The first Olimex node has been brought up and its readings are visible in Home Assistant; check the remaining nodes and all probes in the completed assembly.
