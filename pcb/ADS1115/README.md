# Soldered 333095 ADS1115 module library

This folder contains a project-local KiCad symbol and footprint for the Soldered 333095 ADS1115 breakout module, hardware revision V1.1.1.

## Library files

- `Beluga_Battery.kicad_sym` — schematic symbol.
- `Beluga_Battery.pretty/Soldered_333095.kicad_mod` — through-hole footprint.
- `3dmodels/Soldered_333095_v1.1.1.step` — manufacturer's STEP model.
- `sym-lib-table` and `fp-lib-table` — project-local KiCad library registration.

The libraries appear in KiCad under the nickname `Beluga_Battery`. They are registered for this project only.

## Module pin mapping

The pin numbers follow the manufacturer's V1.1.1 schematic and PCB CAD. Pin names and numbers are deliberately not numbered in simple left-to-right order.

| Footprint pad | Signal |
| ---: | --- |
| 1 | GND |
| 2 | AIN3 / A3 |
| 3 | AIN2 / A2 |
| 4 | AIN1 / A1 |
| 5 | AIN0 / A0 |
| 6 | GND |
| 7 | VCC |
| 8 | SDA |
| 9 | SCL |
| 10 | ALERT/RDY |

The physical pin rows are 2.54 mm pitch and 17.78 mm apart. Their centerline is 2 mm above the footprint origin; this offset comes from the manufacturer's STEP model and is intentional. The footprint uses 1.0 mm finished drill holes for the module headers. The two mounting holes are modeled as 3.2 mm NPTH, 16 mm apart.

## 3D model status

The manufacturer's STEP model is included and assigned to the footprint. Its position and orientation still need a final visual comparison against the physical module before relying on the 3D view for mechanical clearance. The 2D pad layout follows the manufacturer's product pinout and board image.

## Source and license

The STEP file and source board CAD are from the manufacturer's repository:

<https://github.com/SolderedElectronics/ADC-16-bit-ADS1115-4-channel-with-PGA-breakout-hardware-design>

The source repository's `LICENSE.md` applies. A copy of the source repository is retained under `../lib/` for attribution and reference.
