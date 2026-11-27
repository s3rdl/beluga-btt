# Beluga BTT – Temperature Monitoring

ESPHome configuration for twelve independent NTC measurement points, split across three Olimex ESP32-POE-ISO-EA boards. See the [wiring diagram](../assets/battery-temperature-wiring.svg).

## Hardware and network

- Three Olimex ESP32-POE-ISO-EA boards, each with its own wired Ethernet/PoE connection.
- Each board hosts one ADS1115 module on its local I²C bus. All three ADS1115 modules use address `0x48`; the buses are separate.
- On each Olimex board, SDA is GPIO13 and SCL is GPIO16 (UEXT).
- All three boards use the LAN8720 Ethernet PHY: MDC GPIO23, MDIO GPIO18, clock GPIO17 (`CLK_OUT`), PHY address 0, power GPIO12.
- ESPHome board target: `esp32-poe-iso`, variant `esp32` (WROOM-32UE / EA).

The first board has been powered over PoE and is reporting its measurements to Home Assistant. Confirm the pin labels on each board before wiring.

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
| NTC 9 | Board 3 | `0x48` | AIN0 |
| NTC 10 | Board 3 | `0x48` | AIN1 |
| NTC 11 | Board 3 | `0x48` | AIN2 |
| NTC 12 | Board 3 | `0x48` | AIN3 |

The repeated address is valid because each ADS1115 is connected to a different ESP32 I²C bus. Set each ADS1115 ADDR pin to GND for `0x48`.

## ESPHome configurations

- [`beluga_battery.yaml`](beluga_battery.yaml) is Olimex node 1 and reads NTC 1–4.
- [`beluga_battery_2.yaml`](beluga_battery_2.yaml) is **Beluga Betty 2**, reads NTC 5–8, and also runs the ESPHome Bluetooth proxy package.
- [`beluga_battery_3.yaml`](beluga_battery_3.yaml) is Olimex node 3, reads NTC 9–12, and also runs the ESPHome Bluetooth proxy package.

All three files use Ethernet/PoE and the same `ntc_beta_k` secret in a local `software/secrets.yaml` file (ignored by Git):

```yaml
ntc_beta_k: 3435
```

The second and third nodes need their generated, unique API encryption keys in the local `secrets.yaml` as `beluga_betty_2_api_key` and `beluga_battery_3_api_key`. Do not commit those keys.

For the ordered Semitec 103JT-100 NTC, use `ntc_beta_k: 3435`. Its datasheet specifies 10 kΩ at 25 °C and beta 3435 K. Keep `secrets.yaml` local and out of Git. If the installed NTC model changes, update the calibration based on its datasheet.

The ESP32 and ADC inputs operate at 3.3 V. Electrically isolate NTC probes when attaching them to a battery; this monitor does not replace battery protection or charging circuitry.

ESPHome references: [ADS1115](https://esphome.io/components/sensor/ads1115/), [Resistance Sensor](https://esphome.io/components/sensor/resistance/), and [NTC Sensor](https://esphome.io/components/sensor/ntc/).
