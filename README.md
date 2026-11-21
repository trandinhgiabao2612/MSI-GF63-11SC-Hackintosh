<h1 align="center">MSI GF63 11SC Hackintosh</h1>

<p align="center">
  OpenCore EFI for the MSI GF63 Thin 11SC running macOS Tahoe
</p>

<p align="center">
  <img src="https://img.shields.io/badge/macOS-Tahoe%2026.x-blue?logo=apple" />
  <img src="https://img.shields.io/badge/OpenCore-x.x.x-green" />
  <img src="https://img.shields.io/badge/status-WIP-orange" />
</p>

<p align="center">
  <img src="screenshots/about-this-mac.png" width="600" />
</p>

---

> [!WARNING]
> **This project is a work in progress.**
> It is currently waiting for the [WhateverXe](https://github.com/b00t0x/WhateverXe) developer to fix a bug affecting H45 (Tiger Lake-H) CPUs.
> The laptop *can* be used as a daily driver, but it is **not recommended** until the bug is fixed.

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

> Fill in the blanks after testing: ✅ works, ⚠️ partially works, ❌ not working.

## ⚙️ BIOS Settings

Some required options (such as **CFG Lock** and **DVMT Pre-Allocated**) are hidden by default on MSI laptops.
To unlock the hidden advanced menu, press this combination while in the BIOS setup:

```
Right Ctrl + Right Shift + Left Alt + F2
```

<details>
<summary>Disable CFG Lock</summary>

<!-- Steps go here -->

</details>

<details>
<summary>Set DVMT Pre-Allocated to 96M</summary>

<!-- Steps go here -->

</details>

<details>
<summary>Other BIOS options</summary>

<!-- Steps go here -->

</details>

## 🚀 Installation

1. Download the EFI from the **Releases** page (or **Code → Download ZIP**).
2. Create a macOS installer USB by following the [Dortania OpenCore Install Guide](https://dortania.github.io/OpenCore-Install-Guide/).
3. Copy the `EFI` folder to the EFI partition of the USB drive.
4. Generate **your own SMBIOS** (Serial, MLB, SmUUID) with GenSMBIOS. Do not reuse anyone else's.
5. Apply the BIOS settings above, boot from the USB and install macOS.

> [!IMPORTANT]
> Back up your data before installing. Use this EFI at your own risk.

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
- ...

## 🙏 Credits

- [Acidanthera](https://github.com/acidanthera) for OpenCore and the kexts
- [b00t0x](https://github.com/b00t0x/WhateverXe) for WhateverXe
- [Dortania](https://dortania.github.io/OpenCore-Install-Guide/) for the install guide
- ...

## 📄 License

MIT
