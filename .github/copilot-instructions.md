# Copilot Instructions for Flipperzero-SD-SPI

This is a C-based Flipper Zero FAP (Flipper Application Package) for managing SD card locking through the SPI protocol (CMD42). The application allows users to lock, unlock, and manage password-protected SD cards connected to the Flipper Zero device.

## Code Standards

### Required Before Each Commit
- Build the application with `ufbt build` in the `sd_spi` directory to ensure code compiles successfully
- Test thoroughly with `ufbt launch` on a connected Flipper Zero device when possible
- Follow C89/C99 standards as required by the Flipper Zero SDK
- Ensure code follows the existing style in the project

### Development Flow
- **Change directory**: Always work from the `sd_spi/` directory for build operations
- **Build**: `cd sd_spi && ufbt build`
- **Deploy and test**: `cd sd_spi && ufbt launch` (requires connected Flipper Zero)
- **View logs**: `cd sd_spi && ufbt cli`
- **Update SDK**: `ufbt update --channel=release` (or `dev`/`rc` for other channels)

## Repository Structure
- `sd_spi/`: Main application source code directory
  - `sd_spi.c`, `sd_spi.h`: Core SD card SPI protocol implementation
  - `sd_spi_app.c`: Application entry point and UI logic
  - `application.fam`: Flipper application manifest file
  - `sd_spi_app_10px.png`: Application icon (10x10 1-bit PNG)
  - `changelog.md`: Version history and changes
  - `screenshots/`: UI screenshots for documentation
  - `dist/`: Build output directory (created by ufbt)
- `.github/workflows/`: CI/CD automation
  - `build.yml`: Builds FAP for official firmware channels (release, dev, rc)
  - `release.yml`: Creates releases with pre-built FAPs for all firmware variants
  - `build-alternative-firmwares.yml`: Builds for Unleashed, RogueMaster, Momentum
  - `super-linter.yml`: Code linting workflow
- `README.md`: User documentation with installation and usage instructions
- `CONTRIBUTING.md`: Developer documentation with setup and contribution guidelines

## Key Guidelines

### 1. Flipper Zero SDK Compatibility
- This application builds against the **Official Flipper Zero SDK** using ufbt
- Support three SDK channels: `release` (stable), `dev` (bleeding edge), `rc` (release candidate)
- The application must maintain compatibility with all three channels
- Third-party firmwares (Unleashed, RogueMaster, Momentum) typically maintain API compatibility

### 2. Application Manifest
- Edit `sd_spi/application.fam` for application metadata changes
- Version format: `fap_version=(major, minor)` tuple
- Icon must be 10x10 1-bit PNG named `sd_spi_app_10px.png`
- Category is `GPIO` - do not change unless absolutely necessary

### 3. Code Style and Conventions
- Follow existing C code style in the project
- Use meaningful variable and function names
- Add comments for complex logic, especially SPI protocol operations
- Keep functions focused and modular
- Use consistent indentation (match existing files)

### 4. Testing Requirements
- Manual testing on Flipper Zero hardware is essential for SPI operations
- Test basic operations: Init, Lock, Unlock, Force Erase
- Verify password storage in `apps_data/sdspi/pwd.txt`
- Test with different SD card types when possible
- Document any hardware setup requirements

### 5. Documentation Updates
- Update `README.md` for user-facing feature changes
- Update `CONTRIBUTING.md` for developer workflow changes
- Update `sd_spi/changelog.md` for version changes
- Keep documentation in sync with code changes

### 6. CI/CD Integration
- Build workflow validates builds for all three official SDK channels
- Release workflow triggers on version tags (e.g., `v0.6.0`)
- All PRs must pass build validation before merging
- Linting is currently commented out but may be enabled in the future

### 7. Hardware Considerations
- Application interacts with SD cards via SPI protocol
- Default pinout configuration (see README.md for details)
- GPIO pins: A7 (Mosi), A6 (Miso), A4 (CS), B3 (SCK)
- Power: 3.3V supply required
- Consider hardware safety in any SPI protocol changes

## Build Tool: ufbt
The project uses **ufbt** (Micro Flipper Build Tool), not the full fbt toolchain. Key differences:
- Lighter weight and faster than full SDK
- Installed via pip: `pip install ufbt`
- Must be updated to match target SDK channel: `ufbt update --channel=release`
- All commands run from the `sd_spi/` subdirectory
- Build output goes to `sd_spi/dist/`

## Common Tasks

### Adding a New Feature
1. Modify source files in `sd_spi/` directory
2. Build with `cd sd_spi && ufbt build`
3. Test with `cd sd_spi && ufbt launch`
4. Update documentation if user-facing
5. Update `changelog.md` with feature description

### Fixing a Bug
1. Identify the issue in `sd_spi.c`, `sd_spi.h`, or `sd_spi_app.c`
2. Make minimal code changes
3. Test thoroughly with `ufbt launch`
4. Document fix in `changelog.md`

### Updating Dependencies
- SDK version is managed by ufbt channel selection
- No external dependencies beyond Flipper Zero SDK
- Application is self-contained

### Creating a Release
1. Update version in `application.fam`: `fap_version=(major, minor)`
2. Update `changelog.md` with release notes
3. Commit changes: `git commit -m "Release v0.X.0"`
4. Create and push tag: `git tag v0.X.0 && git push origin v0.X.0`
5. GitHub Actions will automatically build and create release

## Important Notes
- This is a hardware-interfacing application - always test on real hardware
- SD card operations can be destructive (Force Erase) - handle carefully
- Password storage is in plain text - consider security implications
- SPI protocol operations require careful timing and error handling
- Always verify R1 response codes after SPI operations
