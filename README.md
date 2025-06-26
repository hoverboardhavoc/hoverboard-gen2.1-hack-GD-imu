# hoverboard-sideboard-hack-GD

[![License: GPL v3](https://img.shields.io/badge/License-GPLv3-blue.svg)](https://www.gnu.org/licenses/gpl-3.0)

This repository demonstrates the use of the IMU on a split hoverboard board. This is not the same as a sideboard that the project was forked from. The splitboards contain both an IMU and MOSFETs to drive the motor, whereas the sideboards only had the IMU and some switches.

---
## Hardware

![classywalk](https://github.com/user-attachments/assets/85e6cda1-4b22-473d-ab47-2d1407d0d309)

The hoverboard has a split motherboard, each side equipped with a GD32F130C8 microcontroller. It is compatible with the [Hoverboard-Firmware-Hack-Gen2](https://github.com/RoboDurden/Hoverboard-Firmware-Hack-Gen2.x/issues/90) project. Some changes were made to support the GD32F130C8, most notably calls to:

```
    i2c_interrupt_enable(MPU_I2C, I2C_INT_ERR);
    i2c_interrupt_enable(MPU_I2C, I2C_INT_EV);
    i2c_interrupt_enable(MPU_I2C, I2C_INT_BUF);
```
were added. This was necessary because the interrupt handlers were never called. This took a long time to figure out. I'm curious why this was not necessary on the GD32F130C6.

# The IMU
<img width="224" alt="IMU" src="https://github.com/user-attachments/assets/dd6f34fa-91d6-4a8b-8690-2bda375f5e2d">

The chip believed to be the IMU is marked with `16A 3C6A C3C`.

`MPU_DMP_ENABLE` is commented out in this fork because the DMP firmware uploaded was not working with the board I tested. This is not surprising because I don't believe that this chip is actually an MPU-6050, but some kind of clone. This means that the quaternion output does not work.

---
## Example Variants

This firmware currently offers these variants (selectable in [`platformio.ini`](/platformio.ini) or [`config.h`](/Inc/config.h)):
- **VARIANT_DEBUG**: In this variant, the user can interact with the sideboard by sending commands via a Serial Monitor to observe and check the capabilities of the sideboard.

---
## Flashing

On the board, there is a debugging header with GND, 3V3, SWDIO, and SWCLK. Connect GND, SWDIO, and SWCLK to your ST-Link V2 programmer. The 3V3 can be either obtained by connecting the pin to the ST-Link programmer or powering the sideboard with 12/15V.

If you have never flashed your board before, the MCU is probably locked. To unlock the flash, check out the wiki page: [How to Unlock MCU flash](https://github.com/EFeru/hoverboard-firmware-hack-FOC/wiki/How-to-Unlock-MCU-flash).

### Building Using PlatformIO IDE (recommended)

- Open the project folder in your IDE of choice (VS Code or Atom).
- Press the 'PlatformIO: Build' or 'PlatformIO: Upload' button (bottom left in VS Code).


