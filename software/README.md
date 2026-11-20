# Beluga Battery – Temperaturmessung

ESPHome-Unterlagen für acht getrennte NTC-Messstellen an einem ESP32-C3.

## Aufbau

- ESP32-C3 DevKitM-1
- Zwei ADS1115 am gemeinsamen I²C-Bus: SDA an GPIO 5, SCL an GPIO 6
- ADS1115 Nr. 1 an Adresse `0x48`, ADS1115 Nr. 2 an Adresse `0x49`
- Je ADS1115 werden alle vier Eingänge AIN0 bis AIN3 verwendet
- Acht voneinander unabhängige Spannungsteiler, je einer pro NTC
- Pro Spannungsteiler ein 10-kΩ-Festwiderstand und ein 10-kΩ-NTC
- Schaltung je Zweig: `3,3 V → 10 kΩ Festwiderstand → Messknoten → NTC → GND`
- Gemeinsame Masse für ESP32, beide ADS1115 und alle Messzweige

| Messstelle | ADS1115-Adresse | Eingang |
|---|---:|---|
| NTC 1 | `0x48` | AIN0 |
| NTC 2 | `0x48` | AIN1 |
| NTC 3 | `0x48` | AIN2 |
| NTC 4 | `0x48` | AIN3 |
| NTC 5 | `0x49` | AIN0 |
| NTC 6 | `0x49` | AIN1 |
| NTC 7 | `0x49` | AIN2 |
| NTC 8 | `0x49` | AIN3 |

## Konfiguration

Die Datei `beluga_battery.yaml` enthält die acht Spannungssensoren sowie die Umrechnung in Widerstand und Temperatur. Sie verweist absichtlich auf nicht mitgelieferte Secrets:

- `wifi_ssid`
- `wifi_password`
- `ntc_beta_k`

Diese Werte müssen vor dem Installieren in einer lokalen ESPHome-`secrets.yaml` ergänzt werden. Es sind hier keine Zugangsdaten oder Beta-Werte eingetragen oder angenommen. Der Temperaturzweig verwendet als Referenz `25 °C bei 10 kΩ` entsprechend der üblichen NTC-Nennwertangabe; bitte anhand des konkreten NTC-Datenblatts bestätigen. Falls der NTC eine andere Referenz hat, muss auch diese Kalibrierung angepasst werden.

Beispiel für die benötigten Einträge, ausdrücklich ohne konkrete Werte:

```yaml
wifi_ssid: "<WLAN-SSID>"
wifi_password: "<WLAN-Passwort>"
ntc_beta_k: <Beta-Wert-in-Kelvin>
```

`<...>` sind Platzhalter und keine gültigen Werte für den produktiven Betrieb. Die echte `secrets.yaml` nicht veröffentlichen oder versionieren.

## Hinweise zum Aufbau

- ADS1115 mit `0x48`: ADDR an GND. ADS1115 mit `0x49`: ADDR an 3,3 V.
- Beide ADS1115 mit 3,3 V versorgen. Die Eingangsspannung darf die ADC-Versorgung nicht überschreiten.
- `gain: 4.096` bietet ausreichenden Messbereich für die 3,3-V-Teiler.
- Der ESPHome-Widerstandssensor ist auf `DOWNSTREAM` gesetzt, weil der NTC im gezeichneten Aufbau zwischen Messknoten und GND liegt.
- Die Temperaturberechnung ist erst mit dem Beta-Wert aus dem NTC-Datenblatt aussagekräftig.
- Die Temperaturfühler elektrisch isoliert anbringen. ESP32/ADCs sind keine Schutz- oder Ladeelektronik für den Akku.

## Referenz

Die Konfiguration orientiert sich an der ESPHome-Dokumentation für [ADS1115](https://esphome.io/components/sensor/ads1115/), [Resistance Sensor](https://esphome.io/components/sensor/resistance/) und [NTC Sensor](https://esphome.io/components/sensor/ntc/).
