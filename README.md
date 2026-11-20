# Beluga Battery Temperature Tracking (BBTT)

ESPHome-based monitor for eight independent NTC temperature channels.

## Hardware

- Olimex **ESP32-POE-ISO-EA** (ESP32-WROOM-32UE, 4 MB flash, external antenna)
- Ethernet with IEEE 802.3af PoE; ESPHome is configured for wired Ethernet
- Two ADS1115 ADCs at I²C addresses `0x48` and `0x49`
- I²C on the Olimex UEXT pins: SDA GPIO13, SCL GPIO16
- Eight independent dividers: `3.3 V → 10 kΩ fixed resistor → ADC node → NTC → GND`

See the [software guide](software/README.md) and [wiring diagram](assets/battery-temperature-wiring.svg).

The NTC calibration currently assumes **10 kΩ at 25 °C**. Verify that reference and the beta value against the actual thermistor datasheet before relying on temperature readings. The exact board variant is selected; physical board revision and pin labels will be checked when it arrives.
