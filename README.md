# MaSi-OS

Preconfigured **Armbian** images for Qualcomm **SM8550 SM8650 SM8750** handhelds — gaming, Plasma Desktop.

Join the community on [Discord](https://discord.gg/Mqegm7PvV9).

---

## Supported devices

One image set boots on multiple AYN and compatible handhelds via an **ABL multidevice `boot/KERNEL`** (ROCKNIX ABL):

| Device | Status |
|--------|--------|
| AYN Odin 2 | Supported |
| AYN Odin 2 Portal | Supported |
| AYN Odin 2 Mini | Supported |
| AYN Thor | Supported |
| AYN Odin 3 | Supported |
| Retroid Pocket 6 | Supported |
| KONKR Pocket FIT (G3 Gen 3) | Supported |
| AYANEO Pocket EVO | Supported |
| AYANEO Pocket S2 | Supported |
| AYANEO Pocket ACE | Supported |
| AYANEO Pocket DS | Supported |
| AYANEO Pocket DMG | Supported |
| AYANEO Pocket S 2K | Supported |

The **ABL picks the correct device tree automatically**. Do **not** add `devicetree=` or `dtb=` to the boot cmdline.
Just flash SD card and enjoy.

Kernel build and updates: **[MaSi-OS Kernel Updater](https://github.com/MaSieS4Fun/MaSi-OS-Kernel-Updater)**.

---

## Gaming performance — fixed

Recent multidevice / upstream-style kernels on SM8550 often lose **40–50% gaming FPS** once USB, Wi‑Fi, and audio are fully active. That is usually **standard vs performance tuning** (scheduler, cmdline, initrd, governors) — not broken drivers.

**MaSi-OS images ship with the performance-tuned profile** used by the Kernel Updater:

- Gaming kernel config (`golden.config`) — cluster scheduling, tuned storage, PSI off
- Short gaming cmdline — **no** `irqaffinity=0-2`, `psi=0`
- Small ABL initrd — firmware and modules on rootfs, fits bootimg limits
- Full peripherals **and** low-latency gaming on the same binary

Technical breakdown: **[GAMING-PERFORMANCE.md](https://github.com/MaSieS4Fun/MaSi-OS-Kernel-Updater/blob/main/docs/GAMING-PERFORMANCE.md)** (Kernel Updater repo).

## Performance comparison

Same game, same settings, same device — **only the kernel differs**.

| Tuned kernel (this project) | Default Armbian kernel config |
|---------------------------|-------------------------------|
| **~50–60 FPS, smooth** | **~40 FPS, stutters** (~40–50% loss) |

<table>
<tr>
<td width="50%" align="center">

**Tuned kernel**
<video src="https://github.com/user-attachments/assets/700702b9-6073-487a-8e7d-4af519503dec" width="100%" controls muted></video>

</td>
<td width="50%" align="center">

**Default Armbian config**
<video src="https://github.com/user-attachments/assets/053ca87f-cfc2-4c3a-8ab8-d50dc07d6c62" width="100%" controls muted></video>

</td>
</tr>
</table>

---
**Default passwords (all images):** `root` and user = **`1234`** — change them after first login.

**Downloads:** [Releases](https://github.com/MaSieS4Fun/MaSi-OS/releases) — verify with the `*_SHA256SUMS.txt` files bundled per variant.

### Shared stack (gaming variants + Desktop Only)

- Latest stable Armbian base
- **Multidevice performance-tuned kernel** (ABL `boot/KERNEL`)
- Updated Mesa / GPU stack
- Steam preconfigured *(SteamOS, Plasma Mobile, Desktop Only)*
- Lossless Scaling preconfigured *(SteamOS, Plasma Mobile, Desktop Only)*
- Gamescope, MangoHud, Goverlay, Lutris, AntiMicroX *(where applicable)*
- Gaming-oriented system tweaks

---

## What is NOT included

This project does not contain:

- ROMs
- BIOS files
- Emulators (as bundled copyrighted packs)
- Copyrighted game content
- Proprietary game assets

Users are responsible for obtaining and using software according to the corresponding licenses.

---

## Installation

1. **Back up your device ABL** and keep a copy on your PC.
2. Download the image variant you want from **[Releases](https://github.com/MaSieS4Fun/MaSi-OS/releases)**.
3. Flash the `.img` (or `.img.gz`) to SD card with [balenaEtcher](https://etcher.balena.io/) or Rufus.
4. Use the **[ROCKNIX ABL](https://github.com/ROCKNIX/abl)** on your device.  
   MaSi-OS is adapted for **EFI boot compatibility** and the same multidevice **`boot/KERNEL`** layout as ROCKNIX — so you can keep or flash the ROCKNIX ABL instead of a separate custom bootloader. That makes dual-booting and switching between ROCKNIX and MaSi-OS much easier.

   If you already run ROCKNIX on your handheld, you usually **do not need to change the ABL** — flash the MaSi-OS image and boot from SD (or your usual ROCKNIX boot path).

   If you are on stock AYN firmware, back up your ABL first, then install the ROCKNIX ABL from the repo above before first boot.

### Boot / device tree

Multidevice images do **not** need manual device-tree selection — the ABL selects the correct DTB from the embedded 11-DTB chain.

### Video guides

| Topic | Guide |
|-------|--------|
| Install ROCKNIX ABL | [YouTube](https://www.youtube.com/watch?v=tIUWKaChuuA) |
| Install & configure Steam + Proton ARM | [YouTube](https://www.youtube.com/watch?v=hT6jeC8ebWY) |

---

## Update kernel only (keep your rootfs)

To rebuild or refresh the multidevice gaming kernel on an existing MaSi-OS install without reflashing the full image:

```bash
git clone https://github.com/MaSieS4Fun/MaSi-OS-Kernel-Updater.git
cd MaSi-OS-Kernel-Updater
./make.sh
sudo ./update.sh
```

See the [Kernel Updater README](https://github.com/MaSieS4Fun/MaSi-OS-Kernel-Updater) for requirements and options.

---

## Support the project

If this helps you and you want to support development, testing, and hosting:

**[Donate via PayPal](https://paypal.me/masies4fun)**

Thank you to everyone who uses, tests, reports issues, and contributes.

---

## Credits

- [Armbian](https://www.armbian.com/) team
- [Valve](https://store.steampowered.com/) (Steam)
- Lossless Scaling developers
- Community contributors and testers

---

This is an independent community project. It is **not** affiliated with AYN, Armbian, Valve, or the developers of Lossless Scaling.

Special thanks to the Armbian team for their work on the ARM Linux ecosystem.
