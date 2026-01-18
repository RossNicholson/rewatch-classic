# Rewatch Classic

A **Mists of Pandaria Classic** adaptation of the **Rewatch** World of Warcraft addon.

[![Latest Release](https://img.shields.io/github/v/release/RossNicholson/rewatch-classic?label=Latest%20Release)](https://github.com/RossNicholson/rewatch-classic/releases/latest)
[![License](https://img.shields.io/badge/license-All%20Rights%20Reserved-lightgrey)](LICENSE)

## Overview

Rewatch is a powerful addon designed to help Druid healers monitor their healing-over-time (HoT) spells with ease. The addon provides an intuitive interface with individual tracking bars for each group or raid member, allowing you to quickly see the status of Lifebloom, Rejuvenation, Regrowth, and Wild Growth effects. Simply click a spell bar to cast that spell on the corresponding player, and watch the timer count down as the HoT expires.

**Party- and raidhealing has never been so easy!**

### Key Features

- **Real-time HoT Monitoring**: Track Lifebloom, Rejuvenation, Regrowth, and Wild Growth timers for all party/raid members
- **One-Click Casting**: Click spell bars directly to cast spells on specific players
- **Visual Feedback**: Color-coded bars that change based on stack counts and remaining duration
- **Quick Actions**: Swiftmend, Remove Corruption, Ironbark, and other utility spells at your fingertips
- **Smart Group Management**: Automatically adds/removes players as your group composition changes
- **Customizable Interface**: Adjustable layouts, colors, and display options
- **Combat-Safe**: Proper handling of protected functions during combat

## Installation

1. Download the latest release from the [Releases page](https://github.com/RossNicholson/rewatch-classic/releases)
2. Extract the `Rewatch` folder to your World of Warcraft addons directory:
   - **Windows**: `World of Warcraft\_classic_\Interface\AddOns\`
   - **Mac**: `World of Warcraft/_classic_/Interface/AddOns/`
3. Launch World of Warcraft and log in to your Druid character
4. The Rewatch frame will appear in the center of your screen - simply click and drag it to your desired location

**Note**: The addon automatically disables itself for non-Druid characters, so you don't need to manually manage it.

## Target Environment

- **World of Warcraft**: Mists of Pandaria Classic
- **Client version**: 5.5.3.64857 (and later)

## Baseline

This repository started from an **unmodified import of Rewatch v50507**, downloaded from CurseForge and verified to load and function in-game. The original baseline is preserved in the repository under the [`baseline-50507`](https://github.com/RossNicholson/rewatch-classic/releases/tag/baseline-50507) tag.

## Usage

### Getting Started

After installation, the Rewatch frame will appear in the middle of your screen. You can move it by clicking and dragging the small blank bar at the bottom of the frame.

### Adding Players

Players are automatically added when you join a group or raid. You can also manually add players using:
- `/rewatch add` - Adds your current target
- `/rewatch add <name>` - Adds a specific player by name
- Right-click a player's HP bar → "Remove player" to remove them

### Casting Spells

- **Click spell bars** to cast the corresponding spell on that player
  - Lifebloom (top bar) - Click to cast/refresh Lifebloom
  - Rejuvenation - Click to cast/refresh Rejuvenation
  - Regrowth - Click to cast/refresh Regrowth
  - Wild Growth - Click to cast Wild Growth
- **Swiftmend button** - Quick access to Swiftmend
- **Utility buttons** - Remove Corruption, Ironbark, Healing Touch, and Nourish appear as needed
- **Modifiers** - Right-click or use Shift/Ctrl/Alt modifiers for additional functionality:
  - Shift-click: Resurrect (auto-selects combat rez or normal rez)
  - Alt-click: Nature's Cure (quick cleansing)
  - Right-click HP bar: Additional options menu

### Configuration

Access options via:
- `/rewatch options` or `/rew options` (shortcut)
- Or: `Esc` → `Interface` → `AddOns` → `Rewatch`

**Important**: Options are not applied live - click "Okay" to save changes. Color changes are applied immediately and cannot be undone.

## MoP Classic Compatibility Updates

This version includes comprehensive updates for Mists of Pandaria Classic compatibility:

- ✅ Fixed `SetBackdrop` compatibility (added BackdropTemplate)
- ✅ Updated UI templates (UIPanelButtonTemplate, UICheckButtonTemplate)
- ✅ Fixed `GetSpecialization` compatibility (fallback to GetPrimaryTalentTree)
- ✅ Fixed CombatLog event handling for MoP Classic API
- ✅ Fixed `UnitBuff` usage (index-based iteration)
- ✅ Fixed `CooldownFrame_SetTimer` compatibility
- ✅ Fixed combat lockdown issues with protected functions
- ✅ Improved Lifebloom stack detection and color display
- ✅ Enhanced Regrowth Lifebloom refresh detection
- ✅ Proper `InCombatLockdown()` checks throughout

## Credits and Provenance

**Rewatch** was originally created by **Coen van der Wel (Dezine)**, with ongoing maintenance and contributions from the community.

This repository is an **independently maintained adaptation** for MoP Classic and is **not an official release** by the original author(s).

Original credits and documentation are preserved in the source files included in the baseline.

## Contributing

Contributions, bug reports, and feature requests are welcome! Please feel free to:
- Open an issue for bugs or feature requests
- Submit a pull request with improvements
- Share feedback and suggestions

## Support

For issues, questions, or to report bugs:
- Check existing [Issues](https://github.com/RossNicholson/rewatch-classic/issues) on GitHub
- Create a new issue with details about the problem
- Include your client version and any relevant error messages

## Releases

- **[v1.0.0-mop-classic](https://github.com/RossNicholson/rewatch-classic/releases/tag/v1.0.0-mop-classic)**: First working release for MoP Classic 5.5.3
- **[baseline-50507](https://github.com/RossNicholson/rewatch-classic/releases/tag/baseline-50507)**: Original unmodified version from CurseForge

## Additional Resources

- Original addon documentation can be found in the `Readme/` directory
- Macro suggestions are available in `Readme/Macros.txt`
- Changelog history is preserved in `Readme/Changelog.txt`

## License

All rights reserved. This is an adaptation of the original Rewatch addon for Mists of Pandaria Classic compatibility.
