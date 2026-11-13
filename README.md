# EFI — Dell Latitude 3420

OpenCore EFI configuration for the Dell Latitude 3420 with Intel Tiger Lake graphics.

## Required graphics package

This setup requires **WhateverXe-Graphics 0.4.3** to be installed in macOS:

[Download WhateverXe-Graphics-0.4.3.pkg](https://github.com/b00t0x/WhateverXe-Graphics/releases/download/v0.4.3/WhateverXe-Graphics-0.4.3.pkg)

Install the package in macOS; it is not copied into the EFI folder.

## Install the EFI

1. Back up the working EFI before replacing anything.
2. Copy the `EFI` directory in this repository to the root of the EFI System Partition.
3. Before booting, create `EFI/OC/config.plist` from `EFI/OC/config.sample.plist`.
4. Generate unique SMBIOS values for your own machine and set `SystemSerialNumber`, `MLB`, `ROM`, and `SystemUUID` under `PlatformInfo -> Generic`. Do not reuse anyone else's values.
5. Validate the config with the matching OpenCore `ocvalidate` before rebooting.

The real `config.plist` is excluded by `.gitignore`; only the redacted sample is tracked.

## Contents

- `EFI/BOOT`: UEFI fallback bootloader.
- `EFI/OC/ACPI`: SSDTs used by this configuration.
- `EFI/OC/Drivers`: OpenCore UEFI drivers.
- `EFI/OC/Kexts`: kernel extensions used by this installation.
- `EFI/OC/Resources`: OpenCanopy resources.

## Hardware notes

The running installation reports SMBIOS `MacBookPro16,2`; the physical laptop is a Dell Latitude 3420 with an Intel Core i5-1135G7 and Tiger Lake SST audio (`8086:A0C8`). Adjust device-specific settings before using this EFI on another computer.
