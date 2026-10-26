# NIRVA: Intelligence Beneath the Surface

Wearable safety monitor for underground coal-mine workers.
Vishwakarma Awards 2026, Application ID VK26-IITH-SM-0280
(Sustainable Mining: Worker Health, Safety & Environmental Hazard Tracking).

Team: Sinchana R (ECE, 3rd year) and Sanjitha LP (Mining Engineering, 3rd year), NITK Surathkal.

> **Prototype for demonstration only. Not for underground use.**
> A heated MQ gas sensor can be an ignition source in methane. Heart rate and SpO2
> are indicative, not diagnostic. NIRVA is not a medical device.

## What this is

A two-part wearable built around one STM32F103C8T6 (Blue Pill):

- **Wrist module:** MAX30102 (heart rate, SpO2, I2C) and LM35 (skin temperature, analog).
- **Pod (chest or helmet):** MQ-series gas sensors (methane or combustible gas, CO),
  DHT11 (ambient temperature and humidity, supporting context), buzzer and alarm LED.
- One microcontroller reads everything and raises a local audible and visual alarm.
  There is no radio in the wearable. LoRa to a surface gateway is a planned stretch goal.

Target parameters: heart rate, SpO2, skin temperature, methane or combustible gas, carbon monoxide.
Gas detection is **relative** (verified with safe stimuli), not calibrated ppm.

## Status (as of 26 Oct 2026)

| Item | Status | Evidence |
|---|---|---|
| Build, flash, run via ST-Link (SWD) | Verified | `firmware/`, `docs/SWD_recovery.md` |
| LED blink | Verified (toolchain check) | `firmware/` |
| UART output at 115200 via CP2102 | Verified | `firmware/`, `photos/` |
| LM35 skin temperature (ADC) | Verified; comparison with a reference thermometer pending | `firmware/` |
| MQ gas sensors | 48 h burn-in done; ADC readings in progress | not yet |
| Buzzer and alarm LED | Hardware identified (active-low buzzer); alarm logic in progress | not yet |
| MAX30102, DHT11, OLED, Kalman filter | Planned | none |
| LoRa / MQTT gateway | Planned (stretch goal) | none |
| PCB and 3D-printed enclosure | Planned (Phase 2) | none |

Evidence is added to this repo as each item is verified. Nothing is marked Verified without a file here.

## Folders

| Folder | Contents |
|---|---|
| `firmware/` | STM32CubeIDE projects (Blink, UART, LM35) |
| `kicad/` | Module-level wiring schematic (ERC: 0 errors). Chip-level schematic and PCB are Phase 2 |
| `bom/` | Prototype bill of materials (CSV copy of the shared sheet) |
| `photos/` | Bench photos and screenshots (original files) |
| `docs/` | Notes, including the SWD debugging write-up |

## Build

1. Open a project folder in STM32CubeIDE.
2. In the `.ioc`, **SYS -> Debug = Serial Wire**.
3. Build, then flash with an ST-Link V2 (SWD).

If the target does not connect, see `docs/SWD_recovery.md`.

## Video link : https://drive.google.com/drive/folders/1_lXvUChW2pcTBaWPdrS3A_UHqrYorxAb

## Honest limitations

- Not calibrated for ppm; relative detection only.
- LM35 is a general-purpose sensor, so we say "skin temperature", not core temperature.
- Runtime on one 18650 is estimated at about 3 to 4 h and has not been measured yet.
- Commit dates in this repo are real. Some bench files were added after the Stage 2 deck was submitted
  on 26 Oct 2026; original files keep their own timestamps.
