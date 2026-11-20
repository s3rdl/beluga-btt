# Beluga Battery – Temperature Monitoring

ESPHome configuration for eight independent NTC measurement points on an ESP32-C3 DevKitM-1. See the [wiring diagram](../assets/battery-temperature-wiring.svg).

## Wiring and channel mapping

- Two ADS1115 devices share the I²C bus: SDA on GPIO5 and SCL on GPIO6.
- The first ADS1115 uses address `0x48` (ADDR to GND); the second uses `0x49` (ADDR to 3.3 V).
- Power both ADS1115 devices from 3.3 V. The ESP32, ADCs, and all eight divider branches share GND.
- Each independent branch is `3.3 V → 10 kΩ fixed resistor → ADC node → NTC → GND`. Do not connect the NTCs in parallel.

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

## ESPHome configuration

[`beluga_battery.yaml`](beluga_battery.yaml) reads each divider voltage, calculates NTC resistance, and converts it to temperature. `gain: 4.096` covers the 3.3 V divider range; `configuration: DOWNSTREAM` matches an NTC between the ADC node and GND.

Before validating or installing, provide these unresolved values in a local `software/secrets.yaml` (ignored by Git):

```yaml
wifi_ssid: "<your Wi-Fi SSID>"
wifi_password: "<your Wi-Fi password>"
ntc_beta_k: <beta value in kelvin from the NTC datasheet>
```

The placeholders are not working values. **The YAML assumes an NTC reference resistance of 10 kΩ at 25 °C; this is not confirmed.** Check the actual NTC datasheet for the reference resistance, reference temperature, and beta value before relying on temperature readings. Adjust the calibration in the YAML if the reference differs. Keep the real `secrets.yaml` private.

The ADC inputs must remain within the ADC supply voltage. Electrically isolate probes when attaching them to a battery; the ESP32 and ADCs do not replace battery protection or charging circuitry.

ESPHome references: [ADS1115](https://esphome.io/components/sensor/ads1115/), [Resistance Sensor](https://esphome.io/components/sensor/resistance/), and [NTC Sensor](https://esphome.io/components/sensor/ntc/).
