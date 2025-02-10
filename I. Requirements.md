

# CAPÍTULO 3: Preparación del EFI de OpenCore

---

Para preparar el EFI de OpenCore, necesitarás descargar OpenCorePkg. Sigue los pasos a continuación para preparar el EFI de OpenCore para tu sistema objetivo.

## Requisitos

- Paquete OpenCore
- Herramienta OCAuxiliary

1. Descarga el paquete de OpenCore. El paquete de OpenCore viene en dos variantes: `DEBUG` y `RELEASE`.

   - **Versión: DEBUG**
     - Notas: Ayuda a depurar problemas de arranque. Puede añadir algo de retraso en los tiempos de arranque. Recomendado para propósitos de depuración.
   - **Versión: RELEASE**
     - Notas: No ayuda mucho con la depuración de problemas de arranque. La resolución de problemas es difícil. Recomendado para uso en producción.

2. Descarga la carpeta `RELEASE` seguida de la versión de lanzamiento.
3. Extrae el archivo zip. Al extraer, obtendrás 4 carpetas como se enumera a continuación.

   - **Directorios: Docs**
     - Descripción: Contiene documentación, changelog, configuración de ejemplo (config.plist) y muestras de ACPI para OpenCore.
   - **Directorios: IA32**
     - Descripción: Contiene OpenCore EFI, cargador de arranque OpenCore de 32 bits. Requerido para OS X 10.4.1 a 10.4.7.
   - **Directorios: Utilities**
     - Descripción: Contiene varias utilidades.
   - **Directorios: X64**
     - Descripción: Contiene OpenCore EFI, cargador de arranque OpenCore de 64 bits. Requerido para OS X 10.8 y versiones más recientes.

4. Dependiendo del tipo de sistema, copia la carpeta EFI de `IA32` (para CPU de 32 bits) o `X64` (para CPU de 64 bits) a tu directorio de trabajo y deberías tener la siguiente estructura.


   ```plaintext
   EFI
   ├── BOOT
   │   └── BOOTx64.efi
   └── OC
       ├── ACPI
       ├── Drivers
       │   ├── AudioDxe.efi
       │   ├── BIOSVideo.efi
       │   ├── CrScreenshotDxe.efi
       │   ├── HiiDatabase.efi
       │   ├── NvmExpressDxe.efi
       │   ├── OpenCanopy.efi
       │   ├── OpenHfsPlus.efi
       │   ├── OpenLinuxBoot.efi
       │   ├── OpenPartitionDxe.efi
       │   ├── OpenRuntime.efi
       │   ├── OpenUsbKbDxe.efi
       │   ├── Ps2KeyboardDxe.efi
       │   ├── Ps2MouseDxe.efi
       │   ├── ResetNvramEntry.efi
       │   ├── ToggleSipEntry.efi
       │   ├── UsbMouseDxe.efi
       │   └── XhciDxe.efi
       ├── Kexts
       ├── Resources
       │   ├── Audio
       │   ├── Font
       │   ├── Image
       │   └── Label
       ├── Tools
       │   ├── BootKicker.efi
       │   ├── ChipTune.efi
       │   ├── CleanNvram.efi
       │   ├── ControlMsrE2.efi
       │   ├── CsrUtil.efi
       │   ├── GopStop.efi
       │   ├── KeyTester.efi
       │   ├── MmapDump.efi
       │   ├── OpenControl.efi
       │   ├── OpenShell.efi
       │   ├── ResetSystem.efi
       │   ├── RtcRw.efi
       │   └── TpmInfo.efi
       └── OpenCore.efi
   ```

## Estructura de Directorios

- **Directorios y Archivos: BOOT/Bootx64.efi**
  - Notas: Cargadores de arranque iniciales que cargan OpenCore.efi. BOOTx64.efi es cargado por el firmware por defecto, de acuerdo con la especificación UEFI. Sin embargo, también puede ser renombrado y colocado en una ubicación personalizada para permitir que OpenCore coexista junto a sistemas operativos, como Windows, que usan archivos BOOTx64.efi como sus cargadores.
- **Directorios y Archivos: ACPI**
  - Notas: Directorio para almacenar archivos ACPI (DSDT y SSDTs). Solo mantén archivos con la extensión .aml.
- **Directorios y Archivos: Drivers**
  - Notas: Directorio para almacenar controladores (UEFI y Legacy). Solo mantén archivos con la extensión .efi.
- **Directorios y Archivos: Kexts**
  - Notas: Directorio para almacenar extensiones del núcleo (también conocidos como controladores). Solo mantén archivos con la extensión .kext.
- **Directorios y Archivos: Resources**
  - Notas: Directorio para almacenar recursos multimedia como audio y GUI, incluyendo fuentes, íconos e imágenes para el selector de arranque para usar con OpenCanopy.
- **Directorios y Archivos: Tools**
  - Notas: Directorio para almacenar herramientas. Solo mantén archivos con la extensión .efi.
- **Directorios y Archivos: OpenCore.efi**
  - Notas: La aplicación principal de arranque responsable de cargar el sistema operativo. El directorio en el que reside OpenCore.efi se llama directorio raíz, que está configurado por defecto en EFI\OC. Sin embargo, al lanzar OpenCore.efi directamente o a través de un lanzador personalizado, también se admiten otros directorios que contienen archivos OpenCore.efi.

## Limpieza

---

Por defecto, OpenCore incluye numerosos Drivers, Resources y Tools para varios propósitos y no todos ellos pueden ser necesarios en el sistema objetivo. Por lo tanto, se requiere una limpieza para asegurar que no haya desorden que dificulte la resolución de problemas y también para reducir el tamaño del archivo del EFI. Sigue los pasos a continuación para realizar una limpieza.

### I. Drivers

Los controladores son esenciales que permiten varias funciones importantes y son necesarios para arrancar el sistema, incluyendo el modo de recuperación. Por defecto, OpenCore incluye numerosos controladores para diferentes propósitos. Debes usar los controladores necesarios para tu sistema. Dependiendo del tipo de sistema que tienes, solo conserva los controladores necesarios según se indica a continuación y elimina el resto de los controladores (donde sea aplicable) del directorio `EFI/OC/Drivers`. Puedes encontrar todos los controladores no vinculados en el directorio `EFI/OC/Drivers`.

- **Nombre del Controlador: AudioDxe.efi**
  - Requerido: Opcional
  - Notas: Proporciona soporte para el sonido de arranque. Solo necesario si deseas usar el sonido de arranque.
- **Nombre del Controlador: BiosVideo.efi**
  - Requerido: Opcional
  - Notas: Controlador de video CSM que implementa el protocolo de salida de gráficos basado en VESA e interfaces BIOS legadas. Utilizado para firmware UEFI con soporte frágil de GOP (por ejemplo, baja resolución). Requiere ReconnectGraphicsOnConnect. Incluido en OpenDuet de forma predeterminada.
- **Nombre del Controlador: CrScreenshotDxe.efi**
  - Requerido: Opcional
  - Notas: Controlador de captura de pantalla. Necesario para tomar capturas de pantalla de BIOS, UEFI Shell, UEFI Apps y UEFI Bootloaders usando F10. Guarda capturas de pantalla en la raíz del ESP o en cualquier sistema de archivos escribible disponible.
- **Nombre del Controlador: Ext4Dxe.efi**
  - Requerido: Opcional
  - Notas: Controlador de sistema de archivos EXT4 de código abierto. Requerido para arrancar con OpenLinuxBoot desde el sistema de archivos más comúnmente usado con Linux.
- **Nombre del Controlador: HiiDatabase.efi**
  - Requerido: Opcional
  - Notas: Controlador de soporte de servicios HII de MdeModulePkg. Este controlador está incluido en la mayoría de los tipos de firmware a partir de la generación Ivy Bridge. Algunas aplicaciones con GUI, como UEFI Shell, pueden necesitar este controlador para funcionar correctamente.
- **Nombre del Controlador: NvmExpressDxe.efi**
  - Requerido: Opcional
  - Notas: Controlador de soporte NVMe de MdeModulePkg. Este controlador está incluido en la mayoría de los firmwares a partir de la generación Broadwell. Utilizado para sistemas Haswell y anteriores, donde no hay soporte de controlador NVMe disponible y se instala un SSD NVMe.
- **Nombre del Controlador: OpenCanopy.efi**
  - Requerido: Opcional
  - Notas: Proporciona funcionalidad GUI para la pantalla de arranque de OpenCore. Este controlador es necesario si necesitas GUI para OpenCore. Consulta Habilitación de GUI para
