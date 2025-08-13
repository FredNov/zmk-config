# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Overview

This is a ZMK (Zephyr Mechanical Keyboard) firmware configuration repository for custom split keyboards, primarily targeting the ThinkCorney keyboard with nice_nano_v2 controllers.

## Architecture

### West Workspace Configuration
- **config/west.yml**: Defines the ZMK fork and modules used
- Currently uses infused-kim's ZMK fork with mouse PS/2 support (`pr-testing/mouse_ps2_module_base`)
- Includes additional modules:
  - PS/2 mouse & trackpoint driver
  - nice_peri_view support 
  - urob's zmk-helpers for key position defines
  - zmk-tri-state behavior for cmd+tab switching

### Hardware Configuration
- **boards/shields/think_corney/**: Hardware definition files for ThinkCorney keyboard
  - `think_corney.zmk.yml`: Shield metadata and features
  - `*.overlay`: Device tree overlays for left/right halves
  - `*.conf`: Configuration files for each half
- **build.yaml**: GitHub Actions build matrix configuration

### Keymap Structure
- **config/base_fed.keymap**: Primary keymap file with 7 layers:
  - BASE (0): Default QWERTY layer
  - LOWR (1): Lower symbols/numbers
  - RAISE (2): Upper symbols/functions
  - NAV (3): Navigation and arrows
  - GAME (4): Gaming layer
  - MOUSELR (5): Mouse left/right
  - MOUSE_TP_SET (6): Mouse/trackpoint settings

### Modular Includes
The keymap uses a modular structure with includes in `config/includes/`:
- `behaviours_*.dtsi`: Custom behavior definitions
- `combos.dtsi`: Key combination shortcuts
- `conditional_layers.dtsi`: Layer activation logic
- `macros.dtsi`: Custom macro definitions
- `mouse_*.dtsi`: Mouse and trackpoint configurations
- `settings*.dtsi`: Keyboard settings and power management

### Legacy Configurations
- **config/corne.keymap**: Alternative Corne keyboard configuration (commented out in build.yaml)
- **config/base.keymap**: Base template keymap

## Development Workflow

### Building Firmware
This repository uses GitHub Actions for automated building. The build process is triggered by:
- Pushes to main branch
- Pull requests
- Manual workflow dispatch

Build configuration is defined in `build.yaml` which specifies:
- Board: nice_nano_v2
- Shields: think_corney_left, think_corney_right

### Keymap Visualization
- **keymap_img/**: Contains tools for generating keymap diagrams
- `update_keymap_img.sh`: Script to generate SVG keymap visualization
- Requires `keymap` CLI tool (not installed in this environment)
- Generates `keymap.svg` from keymap configuration

### Configuration Changes
When modifying keymaps:
1. Edit the appropriate `.keymap` file in `config/`
2. For hardware changes, modify files in `boards/shields/think_corney/`
3. Commit changes to trigger GitHub Actions build
4. Download compiled firmware from Actions artifacts

### ZMK Module Management
To update or change ZMK modules:
1. Edit `config/west.yml` to specify different remotes/revisions
2. Available ZMK forks in the configuration:
   - Standard zmkfirmware (stable, no mouse support)
   - infused-kim (mouse support, currently active)
   - petejohanson (latest mouse PR)
   - urob (mouse + extra features)

### Layer Architecture
The keymap follows a standard ZMK layer approach:
- Conditional layer activation based on key combinations
- Home row modifiers for ergonomic typing
- Gaming layer with WASD-optimized layout
- Mouse integration with trackpoint support
- Macro system for common shortcuts

## Important Files

- `config/base_fed.keymap`: Main keymap configuration
- `config/west.yml`: ZMK workspace and module definitions
- `build.yaml`: GitHub Actions build matrix
- `boards/shields/think_corney/`: Hardware-specific configurations
- `config/includes/`: Modular behavior and feature definitions