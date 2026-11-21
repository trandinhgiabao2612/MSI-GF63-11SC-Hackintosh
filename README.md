<h1 align="center">MSI GF63 11SC Hackintosh</h1>

<p align="center">
  OpenCore EFI for the MSI GF63 Thin 11SC running macOS Sequoia
</p>

<p align="center">
  <img src="https://img.shields.io/badge/macOS-Sequoia%2015.8.3-blue?logo=apple" />
  <img src="https://img.shields.io/badge/OpenCore-1.0.8-green" />
  <img src="https://img.shields.io/badge/status-WIP-orange" />
</p>

<p align="center">
  <img src="screenshots/about-this-mac.png" width="300" />
</p>

---

> [!WARNING]
> **This project is a work in progress.**
> It is currently waiting for the [WhateverXe](https://github.com/b00t0x/WhateverXe) developer to fix a bug affecting H45 (Tiger Lake-H) CPUs.
> The laptop *can* be used as a daily driver, but it is **not recommended** until the bug is fixed.
> **Do NOT install `WhateverXe.pkg` after installing macOS.**
> This EFI uses **WhateverXe 0.8.3**, which is the version with the H45 bug. Installing the package can leave the laptop **unable to boot into macOS**.

## 📑 Table of Contents

- [Supported macOS](#-supported-macos)
- [Specifications](#-specifications)
- [Status](#-status)
- [BIOS Settings](#️-bios-settings)
- [Installation](#-installation)
- [How to Fix](#-how-to-fix)
- [Kexts & SSDTs](#-kexts--ssdts)
- [Known Issues](#-known-issues)
- [Disclaimer](#-disclaimer)
- [Credits](#-credits)

## 💻 Supported macOS

| Version | Status |
|---|---|
| macOS Sequoia 15.8.3 | ⚠️ Tested (WIP) |
| macOS Tahoe 26.x | ❓ Not tested |

> [!NOTE]
> This EFI is only tested on **macOS 15.8.3**. macOS 26.x has not been tested yet.
> If it gets tested, a separate EFI for it will be uploaded.

## 📋 Specifications

| Component | Details |
|---|---|
| Laptop | MSI GF63 Thin 11SC |
| CPU | Intel Core i5-11400H (Tiger Lake-H45) |
| iGPU | Intel UHD Graphics (Xe) |
| dGPU | NVIDIA GeForce GTX 1650 |
| RAM | 16 GB |
| Wi-Fi / Bluetooth | Intel AX201 |
| Storage | Kingston OM8PDP3512B-AI1 (512 GB, PCIe Gen 3 x4 NVMe) |

> [!NOTE]
> This EFI is only tested on the hardware above. Different RAM, Wi-Fi cards, SSDs or BIOS versions may behave differently.

## ✅ Status

| Feature | Status | Notes |
|---|---|---|
| Boot macOS | ⚠️ | Depends on the WhateverXe H45 fix |
| Intel UHD graphics | ⚠️ | Handled by WhateverXe, WIP |
| Wi-Fi | | |
| Bluetooth | | |
| Audio | | |
| Touchpad | | |
| Keyboard / Fn keys | | |
| Webcam | | |
| Sleep / Wake | | |
| NVIDIA GTX 1650 | | |

> ✅ works, ⚠️ partially works, ❌ not working.

## ⚙️ BIOS Settings

Some required options (such as **CFG Lock** and **DVMT Pre-Allocated**) are hidden by default on MSI laptops.
To unlock the hidden advanced menu, press this combination while in the BIOS setup:

```
Right Ctrl + Right Shift + Left Alt + F2
```

| Setting | Value | Path |
|---|---|---|
| Secure Boot | Disabled | `Security` → `Secure Boot Support` |
| CFG Lock | Disabled | `Advanced` → `Power & Performance` → `CPU - Power Management Control` → `CPU Lock Configuration` → `CFG Lock` |
| DVMT Pre-Allocated | 96M | `Advanced` → `System Agent (SA) Configuration` → `Graphics Configuration` → `DVMT Pre-Allocated` |
| Fast Boot | Disabled | `Boot` → `Fast Boot` |
| VT-d | Enabled | `Advanced` → `VT-d` or `Advanced` → `System Agent (SA) Configuration` → `VT-d` |

<details>
<summary>Secure Boot (Disabled)</summary>

1. Go to `Security` → `Secure Boot Support`.
2. Set it to **Disabled**.

</details>

<details>
<summary>CFG Lock (Disabled)</summary>

> [!IMPORTANT]
> Requires the hidden advanced menu. Unlock it first with `Right Ctrl + Right Shift + Left Alt + F2`.

1. Go to `Advanced` → `Power & Performance` → `CPU - Power Management Control`.
2. Open `CPU Lock Configuration` (it is the last item in the list).
3. Set `CFG Lock` to **Disabled**.

</details>

<details>
<summary>DVMT Pre-Allocated (96M)</summary>

> [!NOTE]
> This option is hidden by default. Unlock the hidden advanced menu first.

1. Go to `Advanced` → `System Agent (SA) Configuration` → `Graphics Configuration`.
2. Set `DVMT Pre-Allocated` to **96M** (64M also works, but 96M is recommended).

</details>

<details>
<summary>Fast Boot (Disabled)</summary>

1. Go to `Boot` → `Fast Boot`.
2. Set it to **Disabled**.

</details>

<details>
<summary>VT-d (Enabled)</summary>

1. Go to `Advanced` → `VT-d` and set it to **Enabled**.
2. If you can't find it there, go to `Advanced` → `System Agent (SA) Configuration` → `VT-d` and set it to **Enabled**.

</details>

## 🚀 Installation

This is a prebuilt EFI, free to use, for anyone who doesn't want to build their own.

1. Download the EFI from the **Releases** page (or **Code → Download ZIP**).
2. Create a macOS installer USB by following the [Dortania OpenCore Install Guide](https://dortania.github.io/OpenCore-Install-Guide/). Download macOS directly from Apple.
3. Copy the `EFI` folder to the EFI partition of the USB drive.
4. Generate **your own SMBIOS** (Serial, MLB, SmUUID, ROM) with [GenSMBIOS](https://github.com/corpnewt/GenSMBIOS) and put it in `config.plist`. The values in this repo are placeholders. Do not reuse anyone else's.
5. Apply the [BIOS settings](#️-bios-settings) above, boot from the USB and install macOS.
6. After installing, follow [Booting Without USB](booting-without-usb.md) to copy the EFI to your internal drive. Otherwise you will still need the USB to boot.

> [!IMPORTANT]
> Back up your data before installing.

## 🛠️ How to Fix

### The laptop only boots with the USB / the BIOS doesn't show OpenCore

After installing macOS, the EFI is usually not copied to the internal drive, so the BIOS has nothing to boot.

👉 Follow the guide: **[Booting Without USB](booting-without-usb.md)**

It covers how to:

- copy the `EFI` folder to the internal drive's EFI partition
- add a boot entry using the UEFI Shell (OpenShell)
- set the new entry as the first boot option in the BIOS

### Other problems

**WIP**

## 📦 Kexts & SSDTs

<details>
<summary>Kext list</summary>

- Lilu
- VirtualSMC
- WhateverXe
- AppleALC
- ...

</details>

<details>
<summary>SSDT list</summary>

- ...

</details>

## 🐛 Known Issues

- Waiting on the WhateverXe fix for H45 CPUs
- macOS 26.x is untested with this EFI
- Installing `WhateverXe.pkg` can break booting
- ...

## 📜 Disclaimer

- This EFI is provided **as-is, without any warranty**. Use it at your own risk.
- It is free and not affiliated with Apple, MSI, or any of the projects listed in the credits.
- Apple's macOS installer is **not** included. Download it from Apple.
- Every kext and tool keeps its own license. See the linked repositories for details.

## 🙏 Credits

- [OpCore Simplify](https://github.com/lzhoang2801/OpCore-Simplify) by lzhoang2801, used to generate the base EFI, then tweaked by hand
- [Acidanthera](https://github.com/acidanthera) for OpenCore and the kexts
- [b00t0x](https://github.com/b00t0x/WhateverXe) for WhateverXe
- [Dortania](https://dortania.github.io/OpenCore-Install-Guide/) for the install guide
- ...

## 📄 License

MIT
