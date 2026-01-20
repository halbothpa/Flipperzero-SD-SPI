# Contributing to SD SPI

Thank you for your interest in contributing to the Flipper Zero SD SPI application!

## Development Setup

### Prerequisites

- Python 3.8 or newer
- [ufbt](https://pypi.org/project/ufbt/) (Micro Flipper Build Tool)
- A Flipper Zero device (for testing)

### Setting Up Your Environment

1. Install ufbt:
   ```bash
   pip install ufbt
   ```

2. Update ufbt to the latest SDK:
   ```bash
   ufbt update --channel=release  # For stable release
   # or
   ufbt update --channel=dev      # For bleeding edge
   # or
   ufbt update --channel=rc       # For release candidate
   ```

3. Clone the repository:
   ```bash
   git clone https://github.com/halbothpa/Flipperzero-SD-SPI.git
   cd Flipperzero-SD-SPI/sd_spi
   ```

### Building

Build the FAP:
```bash
cd sd_spi
ufbt build
```

The compiled `.fap` file will be in the `dist/` directory.

### Testing

Deploy and launch on a connected Flipper Zero:
```bash
ufbt launch
```

View logs:
```bash
ufbt cli
```

## CI/CD Workflows

This repository uses GitHub Actions for automated building and releases:

### Build Workflow

- **Trigger**: Push to main/master/dev branches, pull requests
- **Purpose**: Validates that the code builds successfully for all firmware channels and runs linting
- **Outputs**: Build artifacts for Release, Dev, and RC channels
- **Technology**: Uses official [flipperdevices/flipperzero-ufbt-action](https://github.com/marketplace/actions/build-flipper-application-package-fap)

### Release Workflow

- **Trigger**: Push of version tags (e.g., `v0.6.0`) or manual workflow dispatch
- **Purpose**: Creates GitHub releases with pre-built FAPs for all firmware channels
- **Outputs**: GitHub release containing 6 FAP files: 3 official SDK builds (release, dev, rc) and 3 alternative firmware builds (Unleashed, RogueMaster, Momentum)

To create a release:
```bash
git tag v0.6.0
git push origin v0.6.0
```

## Supported Firmware Channels

This project builds for Official Flipper Zero firmware channels:
- **Release Channel**: Stable release (flipperdevices/flipperzero-firmware, release branch)
- **Dev Channel**: Bleeding edge development (flipperdevices/flipperzero-firmware, dev branch)
- **RC Channel**: Release candidate (flipperdevices/flipperzero-firmware, rc branch)

The builds are compatible with third-party firmwares (Unleashed, RogueMaster, Momentum) that maintain API compatibility with official firmware.

## Code Style

- Follow the existing code style in the project
- Use meaningful variable and function names
- Add comments for complex logic
- Keep functions focused and modular

## Submitting Changes

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Make your changes
4. Test thoroughly with `ufbt build` and `ufbt launch`
5. Commit your changes (`git commit -m 'Add amazing feature'`)
6. Push to your fork (`git push origin feature/amazing-feature`)
7. Open a Pull Request

## Questions?

Feel free to open an issue if you have questions or need help!
