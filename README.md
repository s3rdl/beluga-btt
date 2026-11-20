# Beluga Battery Temperature Monitoring

ESPHome configuration and wiring diagram for eight independent NTC temperature-measurement channels. An ESP32-C3 DevKitM-1 reads two ADS1115 ADCs on I²C (SDA GPIO5, SCL GPIO6). The ADCs use addresses `0x48` and `0x49`, with all four inputs on each device assigned to an NTC divider.

Each divider is wired `3.3 V → 10 kΩ fixed resistor → ADC input → NTC → GND`.

- [Software setup, channel mapping, and parameters to verify](software/README.md)
- [Eight-channel wiring diagram](assets/battery-temperature-wiring.svg)

Wi-Fi credentials and the NTC beta value must be supplied locally. The configuration assumes an NTC reference of **10 kΩ at 25 °C**; verify this against the actual NTC datasheet before using temperature readings.
