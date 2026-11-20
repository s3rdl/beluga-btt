# Beluga BTT – Temperature Monitoring

ESPHome configuration for eight independent NTC measurement points using the Olimex ESP32-POE-ISO-EA. See the [wiring diagram](../assets/battery-temperature-wiring.svg).

## Hardware and network

- Olimex ESP32-POE-ISO-EA with ESP32-WROOM-32UE, 4 MB flash, and external antenna.
- Wired 100 Mb Ethernet using IEEE 802.3af PoE.
- ESPHome Ethernet interface: LAN8720 PHY, configured for the WROOM variant.
- Ethernet pins: MDC GPIO23, MDIO GPIO18, clock GPIO17 (CLK_OUT), PHY address 0, power GPIO12.
- ADS1115 I²C bus on Olimex UEXT: SDA GPIO13, SCL GPIO16.
- The board and both ADS1115 devices use 3.3 V logic and a shared ground for the measurement branches.

The ESPHome board target is `esp32-poe-iso`. The `-EA` variant uses the WROOM-32UE module with an external antenna; it uses the WROOM Ethernet clock pin assignment.

## NTC wiring and channel mapping

Each independent branch is `3.3 V → 10 kΩ fixed resistor → ADC node → NTC → GND`. Do not connect NTCs in parallel.

| Measurement point | ADS1115 address | Input |
|---|---:|---|
| NTC 1 | `0x48` | AIN0 |
| NTC 2 | `0x48` | AIN1 |
| NTC 3 | `0x48` | AIN2 |
| NTC 4 | `0x48` | AIN3 |
| NTC 5 | `0x49` | AIN0 |
| NTC 6 | `0x49` | AIN1 |
| NTC 7 | `0x49` | AIN2 |
| NTC 8 | `0x49` | AIN3 |

Set the first ADS1115 ADDR pin to GND for `0x48`; set the second to 3.3 V for `0x49`.

## ESPHome configuration

[`beluga_battery.yaml`](beluga_battery.yaml) reads each divider voltage, calculates NTC resistance, and converts it to temperature. The ADS1115 gain is `4.096`; the resistance sensors use `DOWNSTREAM` because the NTC is between the ADC node and GND.

The only unresolved secret is the NTC beta value. Add it to a local `software/secrets.yaml` file (ignored by Git):

```yaml
ntc_beta_k: <beta value in kelvin from the NTC datasheet>
```

This placeholder is not a working value. **The YAML assumes an NTC reference resistance of 10 kΩ at 25 °C; this is not confirmed.** Check the actual NTC datasheet for reference resistance, reference temperature, and beta value before relying on temperature readings. Update the calibration if the datasheet differs.

The ESP32 and ADC inputs operate at 3.3 V. Electrically isolate NTC probes when attaching them to a battery; this monitor does not replace battery protection or charging circuitry.

Before hardware arrives, the wiring diagram and GPIO plan are based on Olimex documentation for the ESP32-POE-ISO-EA/WROOM variant. Verify the board revision and pin labels on the physical board before wiring.

ESPHome references: [ADS1115](https://esphome.io/components/sensor/ads1115/), [Resistance Sensor](https://esphome.io/components/sensor/resistance/), and [NTC Sensor](https://esphome.io/components/sensor/ntc/).
