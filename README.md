# Flipperzero-SD-SPI

[![Build FAP](https://github.com/halbothpa/Flipperzero-SD-SPI/actions/workflows/build.yml/badge.svg)](https://github.com/halbothpa/Flipperzero-SD-SPI/actions/workflows/build.yml)
[![Release](https://github.com/halbothpa/Flipperzero-SD-SPI/actions/workflows/release.yml/badge.svg)](https://github.com/halbothpa/Flipperzero-SD-SPI/actions/workflows/release.yml)

Flipper Zero FAP for Lock and Unlock SD card / Micro SD card through SPI protocol (CMD42).

## Supported Firmwares

This application is built and tested for Official Flipper Zero firmware channels:
- **Release Channel** - Stable release version (recommended for most users)
- **Dev Channel** - Bleeding edge development version  
- **RC Channel** - Release candidate version

> **Note:** The official ufbt build system is used, which supports all firmware variants based on the official Flipper Zero firmware. Third-party firmwares like Unleashed, RogueMaster, and Momentum typically maintain compatibility with the official firmware API.

<p align="center">
<img src="SDSPI.gif" />
</p>

## Pinout ##

Without Flipper Zero SDBoard the SD card it must be connected as in the table below

Flipper Zero  | SD Card
------------- | -------------
9/3.3V  | +3.3V
8/GND  | GND
2/A7  | Mosi
3/A6  | Miso
4/A4  | CS
5/B3  | SCK

<p align="center">
<img src="scheme.png" />
</p>

## Usage ##

Whenever an SD card is connected it is required to make a "Init", if the operation is successful in the "status" tab R1 is "NO ERROR" and it is possible to execute other commands.

"Lock" and "Unlock" work with password set in namesake tab.

Force Erase allow the removal of unknown password from SD but erases all content.

After the first save, the password is stored in apps_data/sdspi/pwd.txt. you can change it to use characters not found on the flipper keyboard.

## Installation

### From Releases (Recommended)

1. Download the latest release from the [Releases page](https://github.com/halbothpa/Flipperzero-SD-SPI/releases)
2. Choose the `.fap` file matching your firmware channel:
   - `sd_spi_release.fap` - For Release channel (stable, recommended)
   - `sd_spi_dev.fap` - For Dev channel (bleeding edge)
   - `sd_spi_rc.fap` - For RC channel (release candidate)
3. Copy the `.fap` file to your Flipper Zero SD card at `apps/GPIO/`
4. Navigate to `Apps > GPIO > SD SPI` on your Flipper Zero

### Building from Source

Requirements:
- Python 3.8 or newer
- [ufbt](https://pypi.org/project/ufbt/) - Micro Flipper Build Tool

```bash
# Install ufbt
pip install ufbt

# Update ufbt to latest SDK
ufbt update --channel=release  # or dev, rc

# Clone this repository
git clone https://github.com/halbothpa/Flipperzero-SD-SPI.git
cd Flipperzero-SD-SPI/sd_spi

# Build the FAP
ufbt build

# The built .fap file will be in the dist/ folder
```

## Development

### Building and Testing

```bash
# Build the application
cd sd_spi
ufbt build

# Launch on connected Flipper Zero
ufbt launch

# View logs
ufbt cli
```

### Continuous Integration

This project uses GitHub Actions for continuous integration:
- **Build workflow**: Automatically builds the FAP for all supported firmwares on push/PR
- **Release workflow**: Creates releases with pre-built FAPs for all firmwares when a tag is pushed
