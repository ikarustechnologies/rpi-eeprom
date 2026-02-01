# rpi-eeprom
This repository contains the scripts and pre-compiled binaries used to create the `rpi-eeprom` package which is used to update the Raspberry Pi 4 and Raspberry Pi 5 bootloaders EEPROM images.

# Support
Please check the Raspberry Pi [general discussion forum](https://forums.raspberrypi.com/viewforum.php?f=63) if you have a support question.

# Board-Specific Firmware Directories
The `rpi-eeprom-update` script now supports board-specific bootloader directories, which is useful for installer images that need to include firmware for both Raspberry Pi 4 (BCM2711) and Raspberry Pi 5 (BCM2712).

## Automatic Board Detection
The script automatically detects which Raspberry Pi board is running and selects the appropriate firmware directory:

* **Raspberry Pi 4** (BCM2711): Uses `/usr/lib/firmware/raspberrypi/bootloader-2711/`
* **Raspberry Pi 5** (BCM2712): Uses `/usr/lib/firmware/raspberrypi/bootloader-2712/`

## Directory Structure
For installer images containing firmware for both board types:

```
/usr/lib/firmware/raspberrypi/
├── bootloader-2711/
│   ├── stable/
│   │   ├── pieeprom-*.bin
│   │   ├── recovery.bin
│   │   └── vl805-*.bin
│   ├── default/
│   └── latest/
└── bootloader-2712/
    ├── stable/
    │   ├── pieeprom-*.bin
    │   └── recovery.bin
    ├── default/
    └── latest/
```

## Usage
The board detection is automatic. When you run `rpi-eeprom-update`, it will:

1. Detect the board type (Pi 4 or Pi 5)
2. Look for board-specific directories first (e.g., `bootloader-2711` or `bootloader-2712`)
3. Fall back to the generic `bootloader` directory for backward compatibility
4. Use the firmware from the appropriate `stable/`, `default/`, or `latest/` subdirectory

This allows a single installer image to support offline EEPROM updates for both Raspberry Pi 4 and Raspberry Pi 5.

## Environment Variables
You can override the automatic detection by setting:
```bash
export FIRMWARE_ROOT=/path/to/custom/firmware
```

To change the release channel (stable/default/latest):
```bash
export FIRMWARE_RELEASE_STATUS=stable
```

# Reset to factory defaults
To reset the bootloader back to factory defaults use [Raspberry Pi Imager](https://www.raspberrypi.com/software/) to write an EEPROM update image to a spare SD card. Select `Misc utility images` under the `Operating System` tab.

# Bootloader documentation
* [Config.txt boot options](https://www.raspberrypi.com/documentation/computers/config_txt.html#boot-options)
* [Bootloader EEPROM](https://www.raspberrypi.com/documentation/computers/raspberry-pi.html#raspberry-pi-boot-eeprom)
* [Bootloader configuration](https://www.raspberrypi.com/documentation/computers/raspberry-pi.html#raspberry-pi-bootloader-configuration)
* [Updating the Compute Module 4 bootloader](https://www.raspberrypi.com/documentation/computers/compute-module.html#update-the-compute-module-bootloader)
* [Releases and release notes](releases.md)
