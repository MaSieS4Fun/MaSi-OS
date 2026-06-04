# MaSi-OS
ARMBIAN, SteamOS like for AYN ODIN 2, ODIN 2 Poral, ODIN 2 Mini and AYN Thor.
________________
### AYN Armbian Gaming Image

Preconfigured Armbian image for

- AYN Odin 2
- AYN Odin 2 Portal
- AYN Odin 2 Mini
- AYN Thor

Features.
- Latest stable Armbian base system
- Updated kernel
- Updated GPU drivers
- Steam preconfigured
- Lossless Scaling preconfigured
- Gaming-oriented tweaks
- Performance optimizations
_____________
What is NOT included.

This project does not contain:

ROMs
BIOS files
Emulators
Copyrighted game content
Proprietary game assets

Users are responsible for obtaining and using software according to the corresponding licenses.

# Installation

Installation is simple. Flash image to SD card using balenaEtcher or rufus.
Flash cutom ABL for ARMBIAN.
[LINK](https://github.com/strongtz/linux-next/releases/tag/odin2-abl)

## Warning 1

Before proceeding, make a backup of your device's ABL and keep a copy stored safely on your PC.

Rocknix uses a different ABL implementation. If your device is currently using the Rocknix ABL, you must first restore the default AYN ABL and then apply the custom ABL required for Armbian.

### Device Tree Configuration

This image is preconfigured to boot on the AYN Odin 2 (Base model).

If you are using an Odin 2 Portal, Odin 2 Mini, or AYN Thor, the required DTB files are already included in the FAT32 boot partition.

To boot correctly on your device:

1. Open the file `LinuxLoader.cfg`.
2. Locate line 56.
3. Replace:

```text
qcs8550-ayn-odin2.dtb
```

with the DTB file corresponding to your device model.

The available DTB files can be found in the boot partition.

---

## Warning 2

ROOT and USER password = 1234

The system boots into the KDE Plasma Desktop environment by default.

Before launching Gaming Mode for the first time, you must sign in to Steam from the desktop session.

If you skip this step, Gaming Mode may enter an update loop where Steam continuously attempts to update itself, fails because the ARM build is currently distributed as a beta, and prevents further interaction.

### Known Issues

After signing in to Steam, install the following components:

* Runtime 4 ARM
* Proton 11 (ARM)

**Important:** Do not confuse **Proton 11 (ARM)** with the standard **Proton 11** release. The ARM version is required.

### Proton 11 (ARM) Fix

To ensure Proton 11 (ARM) launches games correctly, edit the following file:

```text
~/.local/share/Steam/steamapps/common/Proton 11.0 (ARM64)/toolmanifest.vdf
```

Remove line 5:

```text
"require_tool_appid" "4185400"
```

Removing this entry disables Steam's application validation check for the tool.

If the AppID remains present, Steam attempts to validate the package and the compatibility tool may fail to launch correctly. After removing the line, Proton 11 (ARM) should operate normally.

_____

Credits
Armbian Team
Valve Corporation (Steam)
Lossless Scaling developers
Community contributors and testers
_____

This project is an independent community effort and is not affiliated with AYN, Armbian, Valve or the developers of Lossless Scaling.

Special thanks to the Armbian team for their dedication, hard work and continued support of the ARM Linux ecosystem.
