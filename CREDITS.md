# Credits

## Original Rewatch Addon

### Original Author
- **Coen van der Wel (Dezine)** - Argent Dawn, Europe (2008-2013)
  - Original creator and primary developer of Rewatch

### Maintainer / Contributions
- **bobn64 (Tyrahis, Shu'halo)**
  - Maintained and contributed to Rewatch during the original author's absence

### Localization Contributors
- **Hee-seon** - Korean translation
- **Thomas Friedrich** - German translation
- **Didier "Silvio" Carmin** - French translation
- **Jkon @ Tyrande** - Spanish translation

**Original Repository**: [coenvdwel/rewatch](https://github.com/coenvdwel/rewatch)  
**Original CurseForge**: [rewatch](https://www.curseforge.com/wow/addons/rewatch)

---

## Rewatch Classic (MoP Classic Adaptation)

### Adaptation Maintainer
- **RossNicholson** - MoP Classic compatibility updates

This project is an **independently maintained adaptation** for **Mists of Pandaria Classic** (client version 5.5.3.64857+). It is not an official release by the original author(s).

**GitHub Repository**: [RossNicholson/rewatch-classic](https://github.com/RossNicholson/rewatch-classic)

### MoP Classic Compatibility Updates (v1.0.0-mop-classic)
- Updated for Mists of Pandaria Classic client version 5.5.3.64857
- Fixed SetBackdrop compatibility (added BackdropTemplate)
- Updated UI templates (UIPanelButtonTemplate, UICheckButtonTemplate)
- Fixed GetSpecialization compatibility (fallback to GetPrimaryTalentTree)
- Fixed CombatLog event handling for MoP Classic API
- Fixed UnitBuff usage (index-based iteration)
- Fixed CooldownFrame_SetTimer compatibility
- Fixed combat lockdown issues with protected functions
- Improved Lifebloom stack detection and color display
- Enhanced Regrowth Lifebloom refresh detection
- Proper InCombatLockdown() checks throughout

---

## License

This project is licensed under the **MIT License**. See [LICENSE](../LICENSE) for details.

Original Rewatch addon copyright (c) 2008-2013 Coen van der Wel (Dezine), all rights reserved. Original credits and attribution preserved in source files.
