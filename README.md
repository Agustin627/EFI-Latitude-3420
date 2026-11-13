# Dell Latitude 3420 Hackintosh EFI

[Español](README.es.md)

OpenCore EFI for the **Dell Latitude 3420**, Intel **Core i5-1135G7 (Tiger Lake)**, and macOS Tahoe. This repository documents the tested configuration, required graphics and microphone components, and the current hardware status.

## Hardware status

| Component or feature | Status | Evidence / notes |
|---|---|---|
| Touchpad | **Working** | The touchpad works correctly with the current EFI. |
| CPU frequency scaling | **Working** | Frequency scaling works well. |
| Sound output | **Working** | Audio output works. |
| Internal digital microphone | **Working** | TGLDmic probe captured 144,000 frames in 3.02 s. Core Audio reports source `imic`; Siri recognized a spoken phrase with TGLDmic selected as the default input. |
| WhatsApp microphone input | **Working** | Microphone input worked in WhatsApp. |
| Wi-Fi and Bluetooth | **Working** | Both are working on this installation. |
| GPU acceleration | **Working** | Graphics acceleration is working. |
| HDMI | **Working with an issue** | 4K output has a purple color filter. |
| Sleep/wake | **Broken** | The issue is attributed to WhateverXe-Graphics; a PR to fix it is already open. |
| Keyboard and GPIO interrupts | **Not independently verified** | The EFI includes the current VoodooI2C/VoodooPS2 and ACPI setup, but there is no separate recorded test result. |

### Microphone probe results

- `start: 0x0`; 144,000 frames in 3.02 s (47,673 frames/s).
- Channels 0/1: RMS −54.0 dBFS, peak −31.1 dBFS.
- Channels 2/3: RMS −37.3 dBFS, peak −12.0 dBFS.
- The probe calculated levels in memory and did not save a WAV file.

## Required components

### Intel graphics

Install **WhateverXe-Graphics 0.4.3** in macOS:

[Download WhateverXe-Graphics 0.4.3](https://github.com/b00t0x/WhateverXe-Graphics/releases/download/v0.4.3/WhateverXe-Graphics-0.4.3.pkg)

The package is installed in macOS; it is not copied into the EFI partition.

### Internal microphone

The EFI includes `TGLDmic.kext` 0.5.0. Install its matching HAL component in macOS, then select **Internal Microphone (TGLDmic)** as the default input:

[Download patched TGLDmic 0.5.0 HAL installer](https://raw.githubusercontent.com/Agustin627/TGLDmic/fix/hal-internal-microphone-source/downloads/TGLDmic-0.5.0-HAL-imic-source-fix.pkg) · [local copy](downloads/TGLDmic-0.5.0-HAL-imic-source-fix.pkg)

The installer places `TGLDmic.driver` in `/Library/Audio/Plug-Ins/HAL` and restarts Core Audio. This locally built package is unsigned; macOS may require Control-click → Open. The HAL installer is a macOS component, not an EFI driver.

## Installation

1. Back up the working EFI before replacing it.
2. Copy the `EFI` directory from this repository to the root of the EFI System Partition.
3. Copy `EFI/OC/config.sample.plist` to `EFI/OC/config.plist`.
4. Before booting on another computer, generate and set that computer's own `SystemSerialNumber`, `MLB`, `ROM`, and `SystemUUID` under `PlatformInfo -> Generic`. Do not reuse a published SMBIOS identity.
5. Validate the config with the matching OpenCore `ocvalidate` before rebooting.
6. Install the required macOS packages linked above and select TGLDmic as the default input if using the internal microphone.

The active `config.plist` is excluded by `.gitignore`. The tracked sample is based on the supplied configuration; its `ROM` is redacted because it matched the active EFI. The sample SMBIOS is for reference only—generate unique values for the actual computer.

## Repository contents

- `EFI/BOOT`: UEFI fallback loader.
- `EFI/OC/ACPI`: SSDTs used by this configuration.
- `EFI/OC/Drivers`: OpenCore UEFI drivers.
- `EFI/OC/Kexts`: kext bundles used by this installation.
- `EFI/OC/Resources`: OpenCanopy resources.
- `downloads/`: local copy of the patched TGLDmic HAL installer.
