# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Overview

This is a custom QMK firmware repository that combines Oryx's graphical layout editor with advanced QMK features. It allows you to create keyboard layouts through Oryx's interface while adding custom QMK functionality like macros, layers, and advanced key behaviors.

## Architecture

- **Layout Directory**: `/0LgDQ/` contains the keyboard layout configuration
  - `keymap.c` - Main keyboard layout and custom key behavior logic
  - `config.h` - Hardware and feature configuration
  - `rules.mk` - QMK feature enables/disables
  - `keymap.json` - Oryx layout export (auto-generated)

- **QMK Firmware**: `/qmk_firmware/` submodule containing the QMK firmware codebase
- **GitHub Actions**: Automated workflow for fetching Oryx layouts and building firmware
- **Docker**: Containerized build environment for consistent compilation

## Key Development Commands

### Building Firmware
All firmware building is done through GitHub Actions. The workflow:
1. Fetches latest layout from Oryx
2. Merges with custom QMK code
3. Updates QMK firmware submodule
4. Builds firmware using Docker
5. Provides downloadable artifacts

**Trigger build**: Go to Actions tab → "Fetch and build layout" → Run workflow
- Layout ID: `0LgDQ` (default)
- Keyboard type: `moonlander/revb` (default)

### Local Docker Build (if needed)
```bash
docker build -t qmk .
```

### Manual QMK Commands (inside Docker container)
```bash
# Setup QMK environment
qmk setup zsa/qmk_firmware -b firmware<VERSION> -y

# Build specific layout
make zsa/moonlander/revb:0LgDQ
```

## Workflow Integration

### Oryx to QMK Process
1. Edit layout in Oryx web interface
2. Confirm changes and compile in Oryx
3. Run GitHub Action to sync and build
4. Download firmware from Action artifacts
5. Flash using Keymapp

### Custom QMK Development
- Edit `keymap.c` for custom key behaviors, macros, and logic
- Modify `config.h` for hardware settings and feature configuration
- Update `rules.mk` to enable/disable QMK features
- Custom code persists through Oryx updates due to merge strategy

## Important Configuration

### Current Layout Features (0LgDQ)
- **Layers**: 8 layers (0-7) with different purposes
- **Custom Macros**: 6 string macros (ST_MACRO_0 through ST_MACRO_5)
- **Dual Function Keys**: LT(10, KC_J) with tap/hold behavior
- **Enabled Features**: AUTO_SHIFT, CAPS_WORD, RGB_MATRIX_CUSTOM_KB
- **RGB Controls**: HSV_0_0_255 for lighting effects

### QMK Feature Configuration
Key settings in `rules.mk`:
- `ORYX_ENABLE = yes` - Enables Oryx integration
- `AUTO_SHIFT_ENABLE = yes` - Auto-shift for capitalization
- `CAPS_WORD_ENABLE = yes` - Smart caps word functionality
- `RGB_MATRIX_CUSTOM_KB = yes` - Custom RGB lighting

## Branch Strategy

- **main**: Contains custom QMK code and merged layouts
- **oryx**: Auto-updated with latest Oryx exports
- Workflow merges oryx branch into main to preserve custom modifications