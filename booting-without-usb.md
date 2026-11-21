# 💾 Booting Without the USB

## Why do I need this?

After installing macOS, the OpenCore `EFI` folder usually **does not get copied** to the EFI partition of the internal drive. The macOS installer only installs macOS; it does not install your bootloader.

Because of that, the BIOS has nothing to boot on the internal drive (no OpenCore entry), and you still need the USB stick every time you start the laptop.

The fix has two parts:

1. Copy the `EFI` folder to the internal drive's EFI partition.
2. Make the BIOS see it by adding a boot entry (using the UEFI Shell).

## Step 1: Mount the EFI partitions

You need both EFI partitions mounted at the same time: the **USB** one and the one on the **internal drive** (the SSD that macOS is installed on).

In macOS, you can use a tool such as MountEFI or OpenCore Configurator, or do it in Terminal:

```bash
diskutil list
sudo diskutil mount disk0s1
```

Replace `disk0s1` with the EFI partition of your internal drive.

> [!WARNING]
> Double-check the disk identifier. Make sure each one is the correct drive before you copy anything.

## Step 2: Copy the EFI folder

1. Open the EFI partition of the **internal drive**.
2. If an old `EFI` folder is already in there, delete it.
3. Copy the `EFI` folder from the **USB** EFI partition to the internal drive's EFI partition.

> [!IMPORTANT]
> Only delete the old `EFI` folder if no other OS depends on it. If Windows is installed on the same drive, `EFI/Microsoft` lives there too. In that case, do **not** delete it. Copy only the `BOOT` and `OC` folders next to it.

The result should look like this:

```
EFI
├── BOOT
│   └── BOOTx64.efi
├── OC
│   ├── ACPI
│   ├── Drivers
│   ├── Kexts
│   ├── Tools
│   │   └── OpenShell.efi
│   ├── config.plist
│   └── OpenCore.efi
└── Microsoft        (only if Windows is installed)
```

## Step 3: Add a boot entry with the UEFI Shell

If the laptop does not show a boot entry for the internal drive, you can add one by hand from the UEFI Shell.

### Before you start: enable OpenShell

The EFI on your **USB** needs the UEFI Shell tool (OpenShell):

1. Put `OpenShell.efi` in `EFI/OC/Tools/`.
2. Add it to `config.plist` under `Misc` → `Tools` (`Path` = `OpenShell.efi`, `Enabled` = `YES`). ProperTree's OC Snapshot can do this for you.

### Add the entry

1. Restart and boot from the **USB**.
2. In the OpenCore menu, select **OpenShell**.
3. When the shell opens, **unplug the USB** so you don't mix up the drives.
4. Type `map` to list the devices (`FS0`, `FS1`, `FS2`, ...).
5. Find which `FS` is the internal drive's EFI partition. Switch to each one and list its files, for example:
   ```
   fs1:
   ls
   ```
   The right one contains the `EFI` folder with `OC` inside.
6. Type `bcfg boot dump` to list the existing boot entries and see which slot number is free (for example `03`).
7. Add the new entry (replace the slot number and `fs1:` with your own):
   ```
   bcfg boot add 03 fs1:\EFI\OC\OpenCore.efi "OpenCore"
   ```
   - If you typed the wrong partition or path, the shell says it cannot open the file. Fix the path and try again.
   - If it is correct, the shell reports success. Run `bcfg boot dump` again to confirm the entry is there.
8. Type `exit` to leave the shell.
   - The laptop may freeze at this point. If it does, hold the power button to shut it down, then turn it on again and enter the BIOS.
9. In the BIOS **Boot** menu, you should now see the new **OpenCore** entry. Move it to the first priority and save.

You should now be able to boot macOS without the USB.

## Other options

### Let OpenCore create the entry

In `config.plist`:

- `Misc` → `Boot` → `LauncherOption` = `Full`
- `Booter` → `Quirks` → `RequestBootVarRouting` = `YES`

OpenCore will try to add its own entry to the BIOS boot list. Some firmwares ignore or remove it, so it may not work on every machine.

### Add the entry from Windows (dual boot only)

Run Command Prompt as administrator:

```
bcdedit /set "{bootmgr}" path \EFI\OC\OpenCore.efi
```

The firmware will then load OpenCore first, and you pick Windows or macOS from the OpenCore menu.

> [!WARNING]
> Back up your EFI partition before doing this. To undo it:
> `bcdedit /set "{bootmgr}" path \EFI\Microsoft\Boot\bootmgfw.efi`

## Troubleshooting

- **`cannot open file` in the shell**: the `fsN:` number or the path is wrong. Check with `ls` that `\EFI\OC\OpenCore.efi` exists on that partition.
- **OpenShell is not in the OpenCore menu**: make sure `OpenShell.efi` is in `EFI/OC/Tools/` and enabled in `config.plist`.
- **The entry disappears after a reboot or BIOS update**: the firmware reset its boot list. Add the entry again.
- **It boots from the USB but not from the internal drive**: compare both `EFI` folders and make sure nothing is missing, especially `Drivers` and `Kexts`.

## Reference

- [Dortania OpenCore Post-Install Guide](https://dortania.github.io/OpenCore-Post-Install/)
- Video guide (in Thai, uses Clover; the steps here are adapted for OpenCore): https://www.youtube.com/watch?v=iea49eiQAaI&t=790
