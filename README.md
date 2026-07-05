# MaSi-OS

Preconfigured **Armbian** images for Qualcomm **SM8550** handhelds — SteamOS-like gaming, Plasma Desktop, or a clean base install.

Join the community on [Discord](https://discord.gg/Mqegm7PvV9).

---

## Supported devices

One image set boots on multiple AYN and compatible handhelds via an **ABL multidevice `boot/KERNEL`** (11-DTB chain — same idea as ROCKNIX):

| Device | Status |
|--------|--------|
| AYN Odin 2 | Supported |
| AYN Odin 2 Mini | Supported |
| AYN Odin 2 Portal | Supported |
| AYN Thor | Supported |
| Retroid Pocket 6 | Same bootimg format (DTB chain slots 9–10) |

The **ABL picks the correct device tree automatically**. Do **not** add `devicetree=` or `dtb=` to the boot cmdline.

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

### Performance demos

Same game, same device — tuned kernel vs standard multidevice build:

| | MaSi-OS (performance-tuned) | Standard kernel (FPS loss) |
|---|------------------------------|----------------------------|
| Demo video | [Add link — upload to Releases](https://github.com/MaSieS4Fun/MaSi-OS/releases) | [Add link — upload to Releases](https://github.com/MaSieS4Fun/MaSi-OS/releases) |

Replace the links above when the comparison clips are published (short ~5 MB captures work well in Releases).

---

## Image variants

Four prebuilt images share the same **multidevice kernel stack** and base Armbian SM8550 system. They differ in desktop session and preconfiguration.

| Variant | Session / UX | Best for |
|---------|--------------|----------|
| **SteamOS** | Gamescope + Steam Gaming Mode (SteamOS-like) | Play PC games with Steam ARM + Proton; boot straight into gaming UX |
| **Plasma Mobile** | KDE Plasma Mobile | Handheld-first touch UI, mobile workflows |
| **Desktop Only** | KDE Plasma Desktop | Traditional desktop; gaming tools available without SteamOS session |
| **Clean Install** | KDE Plasma Desktop only | Fresh Linux install — **no** Steam, Lossless Scaling, or extra gaming preconfig |

**Downloads:** [Releases](https://github.com/MaSieS4Fun/MaSi-OS/releases) — verify with the `*_SHA256SUMS.txt` files bundled per variant.

### Shared stack (gaming variants + Desktop Only)

- Latest stable Armbian base (SM8550)
- **Multidevice performance-tuned kernel** (ABL `boot/KERNEL`)
- Updated Mesa / GPU stack
- Steam preconfigured *(SteamOS, Plasma Mobile, Desktop Only)*
- Lossless Scaling preconfigured *(SteamOS, Plasma Mobile, Desktop Only)*
- Gamescope, MangoHud, GOverlay, Lutris, AntiMicroX *(where applicable)*
- Gaming-oriented system tweaks

**Clean Install** includes Armbian + Plasma Desktop + the same multidevice kernel — you add software yourself.

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
| MaSi-OS SteamOS-style image — flash & setup | [YouTube](https://www.youtube.com/watch?v=txjB7dYeyIk) |
| Install & configure Steam + Proton ARM | [YouTube](https://www.youtube.com/watch?v=hT6jeC8ebWY) |

---

## First boot

**Default passwords:** `root` and user = **`1234`** — change them after first login.

### SteamOS / gaming images

Before opening **Gaming Mode** for the first time, **sign in to Steam from the desktop session**.

If you skip this, Gaming Mode may loop on Steam updates (ARM Steam is still beta-heavy) and block interaction.

After signing in, install in Steam:

- **Runtime 4 ARM**
- **Proton 11 (ARM)** — not the x86 Proton 11 package

### Desktop Only / Plasma Mobile

Boot goes to Plasma (Desktop or Mobile). Configure Steam and tools from the desktop if you use those images.

### Clean Install

Standard Plasma Desktop session — no preconfigured Steam session. Use as a normal Armbian handheld Linux install.

---

## Known issues

### Proton 11 (ARM) — toolmanifest fix

If Proton 11 (ARM) fails to launch games, edit:

```text
~/.local/share/Steam/steamapps/common/Proton 11.0 (ARM64)/toolmanifest.vdf
```

Remove line 5:

```text
"require_tool_appid" "4185400"
```

That disables Steam’s validation check for the tool. With the AppID present, the compatibility tool may not start correctly.

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
