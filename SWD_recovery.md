# SWD connection recovery (clone ST-Link V2 + STM32F103C8T6 Blue Pill)

Written from our own debugging, 14 to 16 Oct 2026.

## What happened

| Date | Symptom | Meaning |
|---|---|---|
| 14 Oct | CubeIDE: "Target no device found". CubeProgrammer: "Unable to get core ID", "No STM32 target found" | The ST-Link dongle worked, but the chip did not answer on SWD |
| 15 to 16 Oct | Same error on a fresh project. Board powered (power LED on), link still dead | Not a CubeIDE-only problem |
| 16 Oct | After changing connection settings, a full chip erase, and checking the BOOT jumpers, flashing and blinking worked | Fixed |
| 16 Oct | CubeIDE: "Failed to start GDB server" while CubeProgrammer was still connected | One program at a time can own the ST-Link |

## Working recipe

| Item | Setting |
|---|---|
| Port and mode | SWD, **Under reset** |
| Reset mode | **Hardware reset** |
| Frequency | 100 kHz or lower, Reliable |
| BOOT0 / BOOT1 jumpers | 0 and 0 for normal running |
| Wires | SWDIO, SWCLK, GND, 3.3V, and **RST to the R pin** |
| CubeMX | SYS -> Debug = **Serial Wire** |

Note: we could not isolate which single change was decisive, so we keep the whole recipe.

## Recovery order if it fails again

1. Check the four wires against the pin names printed on the dongle, and press them in.
2. Open CubeProgrammer with the recipe settings and click Connect.
3. If that fails: set BOOT0 = 1, press RESET, connect, and do a **full chip erase**.
4. Put BOOT0 back to 0, confirm SYS -> Debug = Serial Wire, rebuild and reflash.

## One-owner rule

The ST-Link can be used by only one program at a time.
Disconnect CubeProgrammer before flashing from CubeIDE. End any CubeIDE debug session
before using CubeProgrammer.

## Messages that look scary but are not the problem

- "Error 138 ... join a drive to a directory": noise from a failed connection.
- "ST-LINK firmware update available": we deliberately do not update, because the dongle is a clone.
- "Voltage 3.31 V": can come from the dongle's own pin and does not prove the board is powered.
