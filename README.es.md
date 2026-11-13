# EFI Hackintosh Dell Latitude 3420

[English](README.md)

Configuración OpenCore para **Dell Latitude 3420**, Intel **Core i5-1135G7 (Tiger Lake)** y macOS Tahoe. Incluye el estado conocido de los componentes y los paquetes necesarios para gráficos y micrófono.

## Estado del hardware

| Componente o función | Estado | Evidencia / nota |
|---|---|---|
| Touchpad | **Funciona** | El touchpad funciona correctamente con esta EFI. |
| Micrófono digital interno | **Funciona** | TGLDmic capturó 144,000 frames en 3.02 s. Core Audio informa `imic`; Siri reconoció una frase con TGLDmic como entrada predeterminada. |
| Micrófono en WhatsApp | **Funciona** | WhatsApp recibió audio en una prueba anterior. |
| Dictado de ChatGPT y avisos de permisos | **Pendiente de verificar** | Antes se reportó que los avisos de permiso del micrófono no aparecían de forma consistente; no se volvió a probar tras corregir Siri. |
| Teclado e interrupciones GPIO | **Sin prueba independiente registrada** | La EFI contiene la configuración VoodooI2C/VoodooPS2 y ACPI actual, pero falta una prueba específica. |
| Escalado de frecuencia de CPU, reposo/reactivación, Wi-Fi/Bluetooth y HDMI | **No verificados** | No hay resultados actuales registrados; esto no significa que se haya confirmado que fallen. |

### Resultados de la prueba del micrófono

- `start: 0x0`; 144,000 frames en 3.02 s (47,673 frames/s).
- Canales 0/1: RMS −54.0 dBFS, pico −31.1 dBFS.
- Canales 2/3: RMS −37.3 dBFS, pico −12.0 dBFS.
- La prueba calculó niveles en memoria; no guardó un WAV.

## Componentes requeridos

### Gráficos Intel

Instala **WhateverXe-Graphics 0.4.3** en macOS:

[Descargar WhateverXe-Graphics 0.4.3](https://github.com/b00t0x/WhateverXe-Graphics/releases/download/v0.4.3/WhateverXe-Graphics-0.4.3.pkg)

El paquete se instala en macOS; no va dentro de la partición EFI.

### Micrófono interno

La EFI contiene `TGLDmic.kext` 0.5.0. Instala también su componente HAL en macOS y selecciona **Internal Microphone (TGLDmic)** como entrada predeterminada:

[Descargar instalador HAL TGLDmic 0.5.0 con arreglo `imic`](https://raw.githubusercontent.com/Agustin627/TGLDmic/fix/hal-internal-microphone-source/downloads/TGLDmic-0.5.0-HAL-imic-source-fix.pkg) · [copia local](downloads/TGLDmic-0.5.0-HAL-imic-source-fix.pkg)

El instalador coloca `TGLDmic.driver` en `/Library/Audio/Plug-Ins/HAL` y reinicia Core Audio. El paquete compilado localmente no está firmado; macOS podría pedir Control-clic → Abrir. El HAL se instala en macOS, no en la EFI.

## Instalación

1. Haz una copia de seguridad de la EFI funcional.
2. Copia la carpeta `EFI` de este repositorio a la raíz de la partición EFI.
3. Crea `EFI/OC/config.plist` a partir de `EFI/OC/config.sample.plist`.
4. Antes de usarlo en otro equipo, genera valores SMBIOS propios para ese equipo y configura `SystemSerialNumber`, `MLB`, `ROM` y `SystemUUID` en `PlatformInfo -> Generic`. No reutilices identidades SMBIOS publicadas.
5. Valida el plist con la versión correspondiente de OpenCore `ocvalidate` antes de reiniciar.
6. Instala los paquetes de macOS enlazados arriba y, si usarás el micrófono interno, selecciona TGLDmic como entrada predeterminada.

La EFI activa `config.plist` está excluida por `.gitignore`. El archivo de muestra se basa en el plist entregado; su `ROM` está redactada porque coincidía con la de la EFI activa. Los datos del ejemplo son solo de referencia: genera valores únicos para el equipo real.

## Contenido

- `EFI/BOOT`: cargador UEFI de respaldo.
- `EFI/OC/ACPI`: SSDT de esta configuración.
- `EFI/OC/Drivers`: controladores UEFI de OpenCore.
- `EFI/OC/Kexts`: extensiones de kernel incluidas.
- `EFI/OC/Resources`: recursos de OpenCanopy.
- `downloads/`: copia local del instalador HAL compilado de TGLDmic.
