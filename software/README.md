# Beluga BTT – Temperature Monitoring

ESPHome configuration for eight independent NTC measurement points, split across two Olimex ESP32-POE-ISO-EA boards. See the [wiring diagram](../assets/battery-temperature-wiring.svg).

## Hardware and network

- Two Olimex ESP32-POE-ISO-EA boards, each with its own wired Ethernet/PoE connection.
- Each board hosts one ADS1115 module on its local I²C bus. Both ADS1115 modules use address `0x48`; the buses are separate.
- On each Olimex board, SDA is GPIO13 and SCL is GPIO16 (UEXT).
- Both boards use the LAN8720 Ethernet PHY: MDC GPIO23, MDIO GPIO18, clock GPIO17 (`CLK_OUT`), PHY address 0, power GPIO12.
- ESPHome board target: `esp32-poe-iso`, variant `esp32` (WROOM-32UE / EA).

The exact physical board revision and pin labels must still be checked against the delivered hardware before wiring.

## NTC wiring and channel mapping

Each independent branch is `3.3 V → 10 kΩ fixed resistor → ADC node → NTC → GND`. Do not connect NTCs in parallel. Each ADS1115 uses AIN0–AIN3.

| Measurement point | Olimex node | ADS1115 address | Input |
|---|---|---:|---|
| NTC 1 | Board 1 | `0x48` | AIN0 |
| NTC 2 | Board 1 | `0x48` | AIN1 |
| NTC 3 | Board 1 | `0x48` | AIN2 |
| NTC 4 | Board 1 | `0x48` | AIN3 |
| NTC 5 | Board 2 | `0x48` | AIN0 |
| NTC 6 | Board 2 | `0x48` | AIN1 |
| NTC 7 | Board 2 | `0x48` | AIN2 |
| NTC 8 | Board 2 | `0x48` | AIN3 |

The repeated address is valid because each ADS1115 is connected to a different ESP32 I²C bus. Set each ADS1115 ADDR pin to GND for `0x48`.

## ESPHome configurations

- [`beluga_battery.yaml`](beluga_battery.yaml) is Olimex node 1 and reads NTC 1–4.
- [`beluga_battery_2.yaml`](beluga_battery_2.yaml) is Olimex node 2 and reads NTC 5–8.

Both files use Ethernet/PoE and the same `ntc_beta_k` secret in a local `software/secrets.yaml` file (ignored by Git):

```yaml
ntc_beta_k: <beta value in kelvin from the NTC datasheet>
```

This placeholder is not a working value. **The YAML assumes an NTC reference resistance of 10 kΩ at 25 °C; this is not confirmed.** Check the actual NTC datasheet for reference resistance, reference temperature, and beta value before relying on temperature readings. Update both configurations if the datasheet differs.

The ESP32 and ADC inputs operate at 3.3 V. Electrically isolate NTC probes when attaching them to a battery; this monitor does not replace battery protection or charging circuitry.

ESPHome references: [ADS1115](https://esphome.io/components/sensor/ads1115/), [Resistance Sensor](https://esphome.io/components/sensor/resistance/), and [NTC Sensor](https://esphome.io/components/sensor/ntc/).
