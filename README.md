# Arcade Mash MIDI Keyboard

A USB MIDI controller built around a bare RP2040 chip, with big, bouncy arcade buttons instead of stiff keyboard switches. One finger plays a full four-note chord, and a physical toggle switch turns on Scale-Lock mode so you can't hit a wrong note.

Made for Hack Life, Week 3.

<img width="546" height="970" alt="image" src="https://github.com/user-attachments/assets/d2c4d8e3-6b59-4d12-b959-a6cfc5f3be0d" />



## Features

- **Arcade buttons:** 6 key buttons plus 1 big bass kick button, wired to the board with 2-pin connectors
- **Chord mode:** custom firmware where 1 finger = 4 notes
- **Scale-Lock toggle:** a hardware switch that restricts playing to one musical scale, so every note sounds right
- **Indicator LED:** shows when Scale-Lock is on
- **USB-C:** plug-and-play USB MIDI, no drivers needed
- **Bare RP2040:** the chip, 16 MB flash, crystal and 3.3 V regulator are all on the board (no dev board)

## Layout

The board is 120 x 70 mm and follows the original sketch:

| Position | Part |
|---|---|
| Top row, left to right | Key 1, Key 2, Key 3, Key 4 |
| Top right | RP2040, flash chip, USB-C on the right edge |
| Bottom left | Scale-Lock toggle with its LED above it |
| Bottom centre | Bass kick (the big button) |
| Bottom right | Key 5, Key 6 |

The arcade buttons mount in the case lid in the same arrangement, with a wire from each button to its connector on the board.

<img width="1274" height="708" alt="Screenshot 2026-10-26 163737" src="https://github.com/user-attachments/assets/a682d48d-43c9-4468-9e5c-af3e95b4692a" />
Layout of components

## Schematic

<img width="848" height="596" alt="Screenshot 2026-10-26 164018" src="https://github.com/user-attachments/assets/df8f78d2-8537-4a96-b255-f42a1893bdef" />

## Pin map

| RP2040 pin | GPIO | Connected to |
|---|---|---|
| 4 | GPIO2 | Key 1 |
| 5 | GPIO3 | Key 2 |
| 6 | GPIO4 | Key 3 |
| 7 | GPIO5 | Key 4 |
| 8 | GPIO6 | Bass kick |
| 9 | GPIO7 | Key 5 |
| 11 | GPIO8 | Key 6 |
| 12 | GPIO9 | Scale-Lock toggle |
| 13 | GPIO10 | LED (through a 330 ohm resistor) |

All buttons and the toggle connect their pin to ground, and the firmware enables the RP2040's internal pull-ups. A press reads as LOW.

## How it works

- Each arcade button closes a circuit between its GPIO pin and ground.
- The firmware reads the buttons and sends USB MIDI messages to the computer.
- In **chord mode**, each button plays four notes at once instead of one.
- When the **Scale-Lock toggle** is on, the firmware snaps every note to the chosen scale, so no combination of buttons can sound out of key.

## Parts list

| Part | Notes |
|---|---|
| RP2040 | QFN-56 chip |
| W25Q128JVS | 16 MB QSPI flash |
| 12 MHz crystal | with two 15 pF capacitors and a 1 k resistor |
| AP2112K-3.3 | 3.3 V regulator |
| USB-C receptacle | 16-pin, USB 2.0 |
| 6 x arcade button | key buttons |
| 1 x large arcade button | bass kick |
| 1 x toggle switch | Scale-Lock |
| LED and 330 ohm resistor | Scale-Lock indicator |
| 2 x push button | reset and BOOTSEL |
| 3-pin header | SWD debug |
| 2-pin headers | one for each button and the toggle |
| Capacitors and resistors | 100 nF, 1 uF, 15 pF, 27 ohm, 5.1 k, 1 k, 330 ohm (0603) |

## Building it

1. Order the PCB. The RP2040 has fine pins (0.4 mm), so ordering with assembly is strongly recommended.
2. Mount the arcade buttons and toggle in your case lid in the same layout as the board.
3. Wire each button and the toggle to its connector on the board (one wire to the signal pin, one to ground).
4. Hold **BOOTSEL** while plugging in the USB cable. The board shows up as a drive.
5. Copy the firmware onto the drive.

## Firmware

Firmware is in progress.

<!-- Add a link to your firmware folder or repo here, and note which language you're using -->

## Design notes

- Decoupling capacitors sit right against each power pin of the RP2040.
- The crystal and its capacitors are placed close to the XIN and XOUT pins.
- A ground plane covers both layers of the board.
- The USB data lines go through 27 ohm series resistors, and the USB-C CC pins have 5.1 k pull-downs to ground.

## Files

- `schematic/` KiCad schematic
- `pcb/` KiCad PCB layout
- `production/` Gerbers and bill of materials
- `firmware/` MIDI firmware

