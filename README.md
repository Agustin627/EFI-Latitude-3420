# EFI — Dell Latitude 3420

OpenCore EFI configuration for the Dell Latitude 3420 with Intel Tiger Lake graphics.

## Required graphics package

This setup requires **WhateverXe-Graphics 0.4.3** to be installed in macOS:

[Download WhateverXe-Graphics-0.4.3.pkg](https://github.com/b00t0x/WhateverXe-Graphics/releases/download/v0.4.3/WhateverXe-Graphics-0.4.3.pkg)

Install the package in macOS; it is not copied into the EFI folder.

## Required microphone HAL

This EFI includes `TGLDmic.kext` 0.5.0 in OpenCore. To expose the internal digital microphones to Core Audio, also install the matching HAL package in macOS:

[Download the TGLDmic 0.5.0 HAL build with the `imic` source fix](downloads/TGLDmic-0.5.0-HAL-imic-source-fix.pkg)

The installer places `TGLDmic.driver` in `/Library/Audio/Plug-Ins/HAL` and restarts Core Audio. After installation, select **Internal Microphone (TGLDmic)** as the default input in macOS Sound settings. This is a locally built, unsigned package; use macOS Installer or Control-click → Open if Gatekeeper blocks it. The HAL component is separate from the EFI kext, but both are needed for applications such as Siri to use the internal microphone.

## Install the EFI

1. Back up the working EFI before replacing anything.
2. Copy the `EFI` directory in this repository to the root of the EFI System Partition.
3. Before booting, create `EFI/OC/config.plist` from `EFI/OC/config.sample.plist`.
4. Generate unique SMBIOS values for your own machine and set `SystemSerialNumber`, `MLB`, `ROM`, and `SystemUUID` under `PlatformInfo -> Generic`. Do not reuse anyone else's values.
5. Validate the config with the matching OpenCore `ocvalidate` before rebooting.

The real `config.plist` is excluded by `.gitignore`; only `config.sample.plist` is tracked. The sample uses the alternate SMBIOS values supplied for publication, but its `ROM` is redacted because the supplied file reused the active EFI's ROM. Generate your own `SystemSerialNumber`, `MLB`, `ROM`, and `SystemUUID` before using this config on another machine; do not reuse a public SMBIOS identity.

## Contents

- `EFI/BOOT`: UEFI fallback bootloader.
- `EFI/OC/ACPI`: SSDTs used by this configuration.
- `EFI/OC/Drivers`: OpenCore UEFI drivers.
- `EFI/OC/Kexts`: kernel extensions used by this installation.
- `EFI/OC/Resources`: OpenCanopy resources.

## Hardware notes

The running installation reports SMBIOS `MacBookPro16,2`; the physical laptop is a Dell Latitude 3420 with an Intel Core i5-1135G7 and Tiger Lake SST audio (`8086:A0C8`). Adjust device-specific settings before using this EFI on another computer.
