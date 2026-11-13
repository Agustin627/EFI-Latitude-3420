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
| Wi-Fi and Bluetooth | **Working** | Intel Wi-Fi 6 AX201 (PCI `8086:a0f0`, subsystem `8086:4070`); Wi-Fi and Bluetooth work on this installation. |
| GPU acceleration | **Working** | Graphics acceleration is working. |
| YouTube / hardware video processing | **Partially working** | Local logs show repeated CCS hangs during `mediaanalysisd` processing, followed by WindowServer waiting on the Intel driver. Related Tiger Lake reports: [#75](https://github.com/b00t0x/WhateverXe/issues/75) and [#81](https://github.com/b00t0x/WhateverXe/issues/81). An experimental narrow VA workaround is not built or installed. |
| HDMI | **Working with an issue** | 4K output has a purple color filter. |
| Sleep/wake | **Broken** | Related Tiger Lake reports: [#86](https://github.com/b00t0x/WhateverXe/issues/86) describes a hard freeze entering sleep in `GFX0`; [#59](https://github.com/b00t0x/WhateverXe/issues/59) describes sleep that enters but never wakes. Neither report is from this Latitude 3420, so they are references rather than a confirmed diagnosis. |
| Keyboard and special keys | **Working** | Keyboard input and brightness/volume keys work. |
| NVMe storage | **Working** | The NVMe drive works. |

The WhateverXe sleep reports above describe separate Tiger Lake power-management/display paths. Issue #86 reports `GFX0() : Some drivers failed to handle setPowerState` during sleep entry, often after repeated dark wakes on AC; it is marked accepted into the backlog. Issue #59 reports failed wake on a Tiger Lake-H35 laptop and a related external HDMI display problem. The public repository currently shows no open pull request linked to either issue, and neither report confirms the cause on this Latitude 3420. The separate video-processing reports #75 and #81 concern YouTube/VideoToolbox and VA/CCS hangs; they are not sleep/wake fixes.

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
