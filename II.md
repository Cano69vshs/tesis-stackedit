# GUIDE - How to Boot macOS Installer on Desktops and Laptops using OpenCore [UEFI/Legacy] - OpenCore Install Guide | EliteMacx86 Forum


  
OpenCore is a bootloader - Unlike any other bootloader such as GRUB, it is an advanced bootloader especially designed to boot macOS/OS X on Non-Apple computers and is capable of booting a variety of other Operating Systems including Windows and Linux. OpenCore differs a lot from Clover and has been designed with security and quality, allowing us to use many security features found on real Macs such as System Integrity Protection and FileVault. Moreover, configuring an OpenCore EFI (used for booting) is way less complex than Clover and provides much more modern functionality than Clover. Although, still lacks some of the great features which are implemented in Clover such as on-the-fly hot patching. However, there are more advantages to using OpenCore due to its easy-to-configure in nature and regular updates. More in-depth information can be found in [**Why you should use OpenCore over Clover and other Bootloaders**](https://elitemacx86.com/threads/why-you-should-use-opencore-over-clover-and-other-bootloaders.1229/).

OpenCore is almost always changing to support recent versions of macOS and newer computers. It is complex and more difficult to set up OpenCore when you are not familiar with all the components and a variety of options that can be used to configure OpenCore for booting into macOS.

The purpose of this guide is to show how to create a macOS Bootable USB and create OpenCore EFI which can be used to install macOS/OS X on a target computer. Where creating EFI is the main essence of the guide as that's what most people are looking for. It is strongly advised to create a configuration (OpenCore EFI) from scratch without the involvement of someone's else configuration and files and this is where this guide comes into place.

Using OpenCore EFI from another system or picking from the Internet (mostly from Github or other forums) is relatively easy than creating yours, but will not result in many benefits due to the difference in the hardware and the vendor. Although it may be capable of booting macOS/OS X on a target system, these pre-made EFIs not only come with a lot of unnecessary SSDTs, Kexts, and Quirks but also includes custom branding and are usually way lot cluttered than the original method and are generally not reliable (missing hardware functionality and/or features) which is not the preferred choice. Often, it makes it difficult to inject patches, Device Properties, and Quirks due to being prevented from being injected which is why most of the guides generally don't work with such EFIs. Everything is injected forcibly to ensure the macOS/OS X installer boots anyhow on the target system, which still fails in several cases.

There could be known performance-related issues i.e. getting less performance than the system is actually capable of or it may not perform well on your system in general (even if it is working for the primary user). In addition, despite having the same hardware configuration, there are chances that your system may require some additional configuration than the EFI you're using to boot. Most of the users just want to boot the macOS installer on their systems, without getting to know the basics involved which is the key and this is why it makes it more difficult to troubleshoot if such configuration fails to boot the macOS/OS X installer on the target system.

Just to avoid reading and investing time into building a proper EFI, several users use the EFI of someone else. This is a very common practice often followed by new users building their OpenCore EFI and this is why such users run into different issues and invest their time effortlessly to fix the junk. Rather than investing time in troubleshooting the installation and fixing someone's else EFI configuration, which is not even intended for your particular system, it would make more sense to create your own OpenCore EFI and move in the right direction in the first place. Using someone's EFI not just makes it difficult to boot the macOS/OS X installer, but it invites way more issues than it could have originally.

Due to all these reasons, using OpenCore EFI from some other computer or user is never advised and such practice is highly discouraged, especially on this forum. If you don't follow the guide carefully, after a point of time, you will end up frustrated if you're lacking time and patience. Of course, it's your computer and you have the right to decide whether to install macOS for your use case or not.

Please do not expect to have everything working (in terms of hardware, feature, or functionality) once you're booted into macOS/OS X. This guide only aims to install macOS/OS X on a target computer and the rest of the steps are covered in the Postinstallation guide.

For users who are not familiar with OpenCore or if they haven't used it before, this guide may seem a bit complex to them, but it is quite simple if you read and go through the steps carefully. Those users who are familiar with OpenCore or have used it before will find this guide relatively easy to follow than any other guide!

**III. Checking Compatibility**

* * *
Aquí tienes la traducción del contenido proporcionado al español:

---



## Soporte de CPU

### Requisitos de CPU

- **Arquitectura:** Se admiten CPUs x86_64/AMD64 (64-bit) desde Mac OS X 10.4.1.
- **Requisitos de SSE:**
  - `SSE3` es necesario para todas las versiones de Intel de macOS.
  - `SSSE3` es necesario para todas las versiones de macOS de 64-bit.
  - `SSE4` es necesario para `macOS 10.12` hasta, pero no incluyendo, `macOS 10.14`.
  - `SSE4.2` es necesario para `macOS 10.14` y versiones posteriores y para los controladores de GPU AMD más recientes. Se debe evitar CPUs que solo tengan `SSE4.1` si es posible. Puede funcionar con algunos hacks; consulta [Gathering Files > Kexts](https://chefkissinc.github.io/guides/hackintosh/gathering-files/kexts/).

### Incompatibilidades de CPUs AMD

Lamentablemente, algunas cosas en macOS no funcionan correctamente en sistemas AMD.

#### Máquinas Virtuales que utilizan AppleHV

Conocidos problemas:

- VirtualBox versiones posteriores a la 6.
- VMWare versiones posteriores a la 10.
- Parallels versiones posteriores a la 13.1.
- Docker.
- Android Studio.
- Backend de aceleración hvf de QEMU.

#### Problemas de compatibilidad con aplicaciones que utilizan Intel MKL

Las aplicaciones que utilizan Intel MKL necesitarán un parche de una herramienta como [`AMDHelper`](https://github.com/alvindimas05/AMDHelper).

Intel parece estar saboteando los CPUs AMD al hacer que MKL no funcione en ellos.

Aplicaciones conocidas que utilizan Intel MKL:

- Krisp
- Logic Pro Waves Plug-In
- Software de Adobe
- Discord

#### Aplicaciones de 32 bits

Los parches `AMD Vanilla` no soportan aplicaciones de 32 bits, incluso en `WINE`/`CrossOver`. Sin embargo, parece que funciona en Threadripper (TRX40).

## Soporte de GPU

El soporte de GPU es complicado.

### GPUs AMD

- Las GPUs [GCN](https://en.wikipedia.org/wiki/Graphics_Core_Next) y [RDNA](https://en.wikipedia.org/wiki/RDNA_(microarchitecture)) son compatibles con las versiones más recientes de macOS.
- Las GPUs RDNA 3 no están soportadas, y Navi 24 tampoco es soportada (aún). Navi 22 es compatible a través de [`NootRX`](https://chefkissinc.github.io/applehax/nootrx/).
- Las iGPUs [Raven](https://www.techpowerup.com/gpu-specs/amd-raven.g816) y [Renoir](https://www.techpowerup.com/gpu-specs/amd-renoir.g1058) de AMD son compatibles a través de [`NootedRed`](https://chefkissinc.github.io/applehax/nootedred/).
- Otras generaciones de iGPU (GCN <5, RDNA) **no** están soportadas en absoluto (aún).
- Las GPUs dGPU de AMD [Baffin](https://www.techpowerup.com/gpu-specs/amd-baffin.g796) son compatibles con la versión más reciente de macOS.
- Las GPUs dGPU de AMD [Ellesmere](https://www.techpowerup.com/gpu-specs/amd-ellesmere.g795) son compatibles con la versión más reciente de macOS.
- Las GPUs dGPU de AMD [Lexa](https://www.techpowerup.com/gpu-specs/amd-lexa.g806) son compatibles mediante un spoof de ID de dispositivo a la variante Baffin respectiva.
- Las GPUs dGPU Polaris 20 de AMD no están soportadas (aún).

### GPUs NVIDIA

- Las GPUs Maxwell ([9XX](https://en.wikipedia.org/wiki/GeForce_900_series)) y Pascal ([10XX](https://en.wikipedia.org/wiki/GeForce_10_series)) están **limitadas** a **`macOS 10.13`**.
- Las GPUs Turing ([20XX](https://en.wikipedia.org/wiki/GeForce_20_series), [16XX](https://en.wikipedia.org/wiki/GeForce_16_series)) **no** están soportadas en ninguna versión de macOS.
- Las GPUs Ampere ([30XX](https://en.wikipedia.org/wiki/GeForce_30_series)) **no** están soportadas en ninguna versión de macOS.
- Las GPUs Kepler ([6XX](https://en.wikipedia.org/wiki/GeForce_600_series), [7XX](https://en.wikipedia.org/wiki/GeForce_700_series)) son compatibles hasta **`macOS 11`**.

Dicho esto, comencemos con [la recopilación de los archivos necesarios](https://chefkissinc.github.io/guides/hackintosh/gathering-files/).

---

Si necesitas más ayuda con esta traducción o con algún otro aspecto, no dudes en decírmelo.
  
Once you have gathered the system details, the next step is to perform a compatibility check against the hardware you have. This step is important as it will give you an exact idea of whether your system is compatible or not, what exact hardware is compatible, what hardware needs to be replaced with a compatible one, and whether you can proceed with the purpose of installing macOS or not. To check whether your hardware is compatible or not, simply match your hardware from the compatibility list given below.

If you determine that your hardware is compatible, you can proceed to the next step. If not, you'll have to replace the hardware with a compatible one if you're willing to have a perfect setup with general functionality such as graphics acceleration and network. Make sure you pay attention to the hardware limitations while checking the compatibility. You'll find the limitations for each of the hardware types.

**CPU Compatibility**

  
**GPU Compatibility**  
  
**Storage Compatibility**  
  
**Network Compatibility**  
  
**Thunderbolt Compatibility**  

**I. Requirements**

*   USB Flash Drive (16GB at least for OS X 10.11 and newer and 8GB for OS X 10.10 and prior).
*   Access to a computer with OS X/macOS installed (Offline Method)
*   Access to a computer with macOS/OS X or Windows or Linux installed (Online Method).
*   Internet connection to download the required files.

> **QUICK INFO:**  
> 
> *   **It is recommended to choose a good USB brand with good Read and Write speeds. Avoid the Chinese USBs due to their poor Read and Write speeds. It can literally take hours to create a Bootable USB.**
> *   **External Hard Drive or SSD can offer a good speed but is not recommended due to their large capacity.**
> *   **It would be best to use a USB 2.0 Flash Drive for this purpose due to the USB issue which can be fixed after post-installation. Slow, but reliable.**
> *   **Do not use a Virtual Machine or VMware to create your USB, regardless of the Operating System (especially if using macOS/OS X). Virtual Machines and VMware are known to create invalid/corrupt USB Installers. You must use real hardware to create your Bootable USB.**

  


**CHAPTER 4: Gathering Files**  

* * *

  
Once you have the base OpenCore EFI containing the necessary boot files, you will need to add essential SSDTs, Drivers, and Kexts for booting macOS. As this step would require the system specification of the target machine, we assume that you're already aware of the specification. If you still aren't aware, see Gathering System Details for more information.

**I. ACPI (SSDTs)**  
In order to boot into the installation, you need to add the necessary SSDTs. A majority of ACPI (SSDTs) are already included in the OpenCore Package. These ACPI (SSDTs) also must be linked in the config.plist which you'll get to know in Chapter 4 of this guide. Follow the steps below to place the necessary ACPI (SSDTs).

STEP 1: Depending on the Platform you have, copy the SSDTs from `OpenCore/RELEASE/Docs/AcpiSamples/Binaries` to `EFI/OC/ACPI` directory.

> **QUICK INFO:  
> SSDT marked with \* are bundled with OpenCore. Additional SSDTs described here can be downloaded from the [**SSDTs Download**](https://elitemacx86.com/resources/categories/ssdt.17/) section.**

  



* SSDT Name: SSDT-ALS0.aml*
  * Notes: Enables Ambient Light Sensor ACPI DeviceRequired for Backlight functioningRequired for AIO and Laptops only.
* SSDT Name: SSDT-BRG0.aml
  * Notes: Adds missing ACPI device for proper GPU initialization.Reduces boot time and provides faster userspace.Required for mainly GPUs. If there are other devices that have missing ACPI devices, it is recommended to create an SSDT for such. See Tuning ACPI for more information.Relevant for AMD Vega, RX 5000 series, and newer GPUsRequired for all versions of macOS and is mandatory for macOS Big Sur (11.x) and later.
* SSDT Name: SSDT-EC.aml*
  * Notes: Fixes EC.Required for Penryn to Broadwell
* SSDT Name: SSDT-EC-USBX.aml*
  * Notes: Fixes EC and USB Power SupplyRequired for EC and correcting USB Power Supply.Required for Skylake and Later
* SSDT Name: SSDT-AWAC-DISABLE.aml*
  * Notes: Serves 300 series RTC patch.Fixes stuck at apfs_module_start error while booting into macOSRequired for Coffee Lake and Later.Do not use SSDT-AWAC-DISABLE.aml and SSDT-RTC0.aml or SSDT-RTC0-RANGE.aml together
* SSDT Name: SSDT-PM.aml
  * Notes: Enables CPU Power Management on Legacy Intel CPUs (Ivy Bridg and prior).Fixes potential issues such as sleep/wake function and improves CPU performanceRequired for proper CPU and GPU Power Management.Required for Ivy Bridge and prior. Use SSDT-PLUG.aml for Haswell and later.See X79 CPU Power Management for more information on X79 CPU Power Management.You can generate your own SSDT-PM during the post-installation stage. See Generating SSDT for CPU Power Management for more information.
* SSDT Name: SSDT-PLUG.aml*
  * Notes: Enables native CPU Power Management (XCPM) function on Haswell and newer CPUs.Fixes potential issues such as sleep/wake function and improves CPU performanceRequired for proper CPU and GPU Power Management.Requires MSR unlocked Motherboard. Where not feasible, use the Bootloader patch.Required for Haswell to Rocket Lake Systems.Required for up to macOS Big Sur (11.x) only.Use SSDT-PM.aml generated using ssdtPRGen script for Ivy Bridge and prior.
* SSDT Name: SSDT-PLUG-ALT.aml*
  * Notes: Enables native CPU Power Management function on Alder Lake and newer CPUs.Fixes potential issues such as sleep/wake function and improves CPU performanceRequired for proper CPU and GPU Power Management.Requires MSR unlocked Motherboard. Where not feasible, use the Bootloader patch.Required for Alder Lake and newer CPUs.Use SSDT-PLUG.aml for Comet Lake and prior.
* SSDT Name: SSDT-PNLF.aml*
  * Notes: Provides Backlight supportAdds a Brightness slider in System Preferences>Displays.Required for Brightness control.Requires a full Graphics acceleration with QE/CI.Requires WhateverGreen.kext to function.Must load after all OEM DSDT and SSDTsRequires mapping Brightness keys for a functional Brightness Control via Hotkeys. See remapping Brightness Hotkeys for more information.
* SSDT Name: SSDT-EHCx-DISABLE.aml*
  * Notes: Required for 7, 8, and 9-series Chipsets on 10.11 and newer.
* SSDT Name: SSDT-HV-DEV-WS2022.aml
  * Notes: Disables unsupported devices under macOS on Windows Server 2019/Windows 10 and newerRequired for Hyper-V only. See Installing macOS on Hyper-V for more information.Requires MacHyperVSupport.kext to function.Do not use it on a bare metal.
* SSDT Name: SSDT-HV-CPU.aml*
  * Notes: Enables proper CPU detection under macOSRequired for Hyper-V only. See Installing macOS on Hyper-V for more information.Requires MacHyperVSupport.kext to function.Do not use it on a bare metal.
* SSDT Name: SSDT-HV-VMBUS.aml*
  * Notes: Enables ACPI node identificationRequired for Hyper-V only. See Installing macOS on Hyper-V for more information.Requires MacHyperVSupport.kext to function.Do not use it on a bare metal.
* SSDT Name: SSDT-HV-PLUG.aml*
  * Notes: Enables VMplatformPlugin on Big Sur and newerRequired for Hyper-V only. See Installing macOS on Hyper-V for more information.This SSDT must be loaded after SSDT-HV-CPU.amlRequires MacHyperVSupport.kext to function.Do not use it on a bare metal.
* SSDT Name: SSDT-IMEI.aml*
  * Notes: Enables missing IMEI DeviceRequired for IGPU Graphics acceleration on macOS.Required when no IMEI device (with any name) is present in the native DSDT.Usually required for Sandy Bridge CPUs on 7 Series Chipsets and Ivy Bridge CPUs on 6 Series Chipsets.Requires a custom device-id in DeviceProperties for Sandy Bridge and Ivy Bridge Systems. See injecting Device Properties and booting macOS for more information.Do not use if IMEI Device is present with the 0x00160000 address in the native DSDT.
* SSDT Name: SSDT-PMC.aml*
  * Notes: Enables NVRAM supportFixes NVRAM, Sleep/Wake, and Restart/Shut DownFixes the black screen after macOS finishes loading and is applicable for IGPU, NVIDIA and AMD GPUs.Required for all 300 series motherboards except Z370.
* SSDT Name: SSDT-RTC0.aml*
  * Notes: Required for 300 series chipset onlyCan fix stuck at PCI Configuration begin error when booting into macOSRequired where SSDT-AWAC.aml is not compatibleDo not use SSDT-RTC0.aml or SSDT-RTC0-RANGE.aml and SSDT-AWAC-DISABLE.aml together.
* SSDT Name: SSDT-RTC0-RANGE.aml*
  * Notes: Required for all X99 and X299 MotherboardsCan fix stuck at PCI Configuration begin error when booting into macOSDo not use SSDT-RTC0-RANGE.aml or SSDT-RTC0.aml and SSDT-AWAC-DISABLE.aml together.
* SSDT Name: SSDT-SBUS-MCHC.aml*
  * Notes: Adds missing MCHC DeviceFixes AppleSMBus support in macOS/OS X.Required when no MCHC device is present in the native DSDT.Do not use if MCHC is present in your DSDT.
* SSDT Name: SSDT-UNC.aml*
  * Notes: Disables unused devices in ACPIEnsures IOPCIFamily doesn't Kernel PanicRequired for Sandy Bridge-E to Broadwell-E Systems.


Although an additional SSDT for a target system may be a requirement, to fix a problem or a hardware and/or its function, it is a minimum set of SSDTs that is necessary and is sufficient enough to boot the macOS installer and perform the installation. The rest of the additional required SSDTs can be added during the post-installation time unless it is mandatory for booting the macOS/OS X installer. See [**Tuning ACPI**](https://elitemacx86.com/threads/tuning-acpi-for-macos-clover-opencore.1084/) for more information.

**Summary**

* * *

  
If you're still confused regarding which SSDTs to use, we've summarized it to understand it better for your convenience. Simply choose the combination of the SSDTs based on your CPU Code Name. For example, if you have a `Raptor Lake` as a target system, the typical SSDTs you would need is `SSDT-EC-USBX.aml`+`SSDT-AWAC-DISABLE.aml`+`SSDT-PLUG-ALT.aml` as per the Table given below. Please note that this combination will work in most cases, however, you may need a different combination in special cases for your hardware.

**II. AMD Desktops**


|CPU Code Name                             |EC                      |AWAC|IMEI|NVRAM|USB|CPU          |
|------------------------------------------|------------------------|----|----|-----|---|-------------|
|Bulldozer/Piledriver/Steamroller/Excavator|SSDT-EC-USBX-DESKTOP.aml|-   |-   |-    |-  |-            |
|Jaguar/Puma                               |SSDT-EC-USBX-DESKTOP.aml|-   |-   |-    |-  |-            |
|Zen/Zen+/Zen2                             |SSDT-EC-USBX-DESKTOP.aml|-   |-   |-    |-  |SSDT-CPUR.aml|
|Zen3/Zen3+/Zen4                           |SSDT-EC-USBX-DESKTOP.aml|-   |-   |-    |-  |SSDT-CPUR.aml|



**III. AMD Laptops**


|CPU Code Name|EC              |AWAC|IMEI|NVRAM|USB|CPU          |Backlight    |
|-------------|----------------|----|----|-----|---|-------------|-------------|
|Zen          |SSDT-EC-USBX.aml|-   |-   |-    |-  |-            |SSDT-PNLF.aml|
|Zen+         |SSDT-EC-USBX.aml|-   |-   |-    |-  |-            |SSDT-PNLF.aml|
|Zen2         |SSDT-EC-USBX.aml|-   |-   |-    |-  |SSDT-CPUR.aml|SSDT-PNLF.aml|
|Zen3         |SSDT-EC-USBX.aml|-   |-   |-    |-  |             |SSDT-PNLF.aml|
|Zen3+        |SSDT-EC-USBX.aml|-   |-   |-    |-  |             |SSDT-PNLF.aml|
|Zen4         |SSDT-EC-USBX.aml|-   |-   |-    |-  |             |SSDT-PNLF.aml|


**II. Drivers**  
Unlike the SSDTs, the drivers are one of the essential elements in OpenCore EFI and are mainly required for booting macOS on the target machine for either UEFI or a Legacy environment.  
Depending on the firmware, a different set of drivers may be required. Loading an incompatible driver may lead the system to an unbootable state or even cause permanent firmware damage. There are drivers which provide the ability to scan different formats of drives in the OpenCore picker (such as HFS+ Drives). These drivers also must be linked in the config.plist which you'll get to know in Chapter 4 of this guide. Follow the steps below to place the necessary Drivers.

STEP 1: Download OcBinaryData  
STEP 2: Extract the downloaded .zip file.  
STEP 3: Copy the appropriate drivers from `OCBinaryData/Drivers` to `EFI/OC/Drivers` directory.

Although OpenCorePkg contains the necessary drivers to boot the macOS installer on the target machine, there is an OpenHFSPlus.efi driver which is generally slow as compared to HFSPlus.efi proprietary driver. Therefore, it is advised to use HFSPlus.efi instead of OpenHFSPlus.efi to avoid unnecessary delays. The HFSPlus.efi driver comes in different flavors and must be used accordingly. You can find which version to use below.

* Driver Name: HfsPlus.efi
  * Required: YES
  * Notes: HFS File System Driver with bless support.Provides reading of HFS+ Volumes/drives in OpenCore Boot Picker.Without HFSPlus.efi driver, you won't be able to see any HFS+ volumes including your macOS USB Installer. Recovery and installation drive for macOS Sierra and prior in OpenCore Boot Picker.This driver is an alternative to a closed source HfsPlus driver commonly found in Apple firmware.
* Driver Name: HfsPlusLegacy.efi
  * Required: Optional
  * Notes: Provides reading of HFS+ Volumes/drives in OpenCore Boot Picker.Required for Sandy Bridge and older Systems.
* Driver Name: HfsPlus32.efi
  * Required: Optional
  * Notes: Similar to HfsPlusLegacyRequired for 32-bit CPUs.


**III. Kexts**  
Unlike drivers for other OSes, a Kext (Kernel Extension) is a driver for macOS. In order to boot into the installation, you need to add the necessary kexts. Follow the steps below to place the necessary kexts.

STEP 1: Download the required kexts as per your hardware.  
STEP 2: Extract the kexts from the RELEASE folder.  
STEP 3: Copy the required kexts with `.kext` extension to `EFI/OC/Kexts` directory.

**System**

* * *

*   Required for Booting into macOS and are mandatory.

**Lilu**  

*   Provides arbitrary patching.
*   Required for AppleALC, WhateverGreen, VirtualSMC and several other kexts.

**VirtualSMC**  

*   SMC Emulator. Emulates Apple hardware.
*   VirtualSMC is a successor of FakeSMC.
*   This kext requires `Lilu.kext` to function.

  
**CPU and SMC**  

* * *

  
**CpuTopologyRebuild**  

*   Optimizes Alder Lake/Raptor Lake's heterogenous core configuration
*   Reports Hyper-Threading Technology to Enabled in Alder Lake/Raptor Lake systems.
*   Improves performance by rebuilding the topology for the cores and threads.
*   Requires `ProvideCurrentCpuInfo` Quirk along with `-ctrsmt` boot arg.

**CPUFriend**  

*   Enables dynamic power management data injection
*   Requires `Lilu.kext` to function

**AsusSMC**  

*   Enables Native support for ALS, Keyboard Backlight and Fn Keys for ASUS Laptops
*   Requires `Lilu.kext` to function

**CPUTscSync**  

*   Enables synchronization of the TSC with the CPU
*   Required for Laptops and HEDT Systems.
*   **_A quick way to find if your Laptop needs CpuTscSync.kext, while booting the installer, you may feel lags and the installer getting stuck at several stages or you may see the Apple logo or other pop-ups appearing in background._** _**On some Laptops, you may get CPU panic for threads. f you're having such scenarios, you'll need to use this kext.**_
*   **_This kext is mostly needed by Lenovo and newer ASUS Laptops with the latest BIOS._**
*   This kext requires `Lilu.kext` to function.
*   This kext is only supported by Intel CPUs only.

**AmdTscSync**  

*   Enables synchronization of the TSC with the CPU
*   Required for Laptops and HEDT Systems.
*   **_A quick way to find if your Laptop needs CpuTscSync.kext, while booting the installer, you may feel lags and the installer getting stuck at several stages or you may see the Apple logo or other pop-ups appearing in background._** _**On some Laptops, you may get CPU panic for threads. f you're having such scenarios, you'll need to use this kext.**_
*   **_This kext is mostly needed by Lenovo and newer ASUS Laptops with the latest BIOS._**
*   This kext requires `Lilu.kext` to function.
*   This kext is only supported by AMD CPUs only.

**AppleMCEReporterDisabler**  

*   Disables `AppleMCEReporter.kext` to prevent Kernel Panics
*   Required for AMD CPUs running macOS 12.3 and newer
*   Required for macOS 10.15 and later on Dual-Socket Systems.
*   Only applicable if using MacPro6,1, MacPro7,1 or iMacPro1,1 SMBIOS.

**AMDRyzenCPUPowerManagement.kext**  

*   Required for AMD Power Management Features
*   This kext is also required if you would like to use AMD Power Gadget.
*   Requires `SMCAMDProcessor.kext` to enable sensor data
*   `AMDRyzenCPUPowerManagement.kext` should load before `SMCAMDProcessor.kext`

**CryptexFixup.kext**  

*   Enables Rosetta Cryptex installation in macOS Ventura Ventura.
*   Supports OS Installation and updates.
*   Required for systems lacking AVX2.0 instruction set who wish to run macOS Ventura.
*   Does not supports macOS Delta Updates (typically 1-3GB in size). However, full updates are supported.
*   In addition, Rapid Security Response Updates are currently not supported.
*   This kext does not drop the requirement of AVX2.0 in some of Ventura's Graphics Stack.
*   This kext also disables Cryptex hash verification in `APFS.kext`.
*   Requires `Lilu.kext` to function.

**telemetrap.kext**  

*   Disables AppleMCEReporter kext to prevent Kernel Panics
*   Required for AMD CPUs and Dual-Socket Systems.

**AAAMouSSE.kext**  

*   Enables SSE4.2 Support
*   Allows old CPUs to use the newer AMD drivers.
*   Required by AMD drivers for Metal Support
*   Only required for CPUs lacking SSE4.2 instructions.

**Graphics**  

* * *

  
**WhateverGreen.kext**  

*   Provides GPU patching on macOS.
*   Fixes various Graphics related issues.
*   Required for Intel, NVIDIA, and AMD GPUs.
*   This kext requires `Lilu.kext` to function.

**Nooted.kext**  

*   Provides AMD (APU) patching on macOS.
*   Fixes various Graphics related issues.
*   Required for AMD APUs.
*   This kext requires `Lilu.kext` to function.

**dAGPM.kext**  

*   Enables Power Management for Discrete Graphics (NVIDIA/AMD)
*   Requires Board ID configuration to work.

**USB**  

* * *

*   Required for USB functioning.

**USBInjectAll**  

*   Enables USB ports.
*   Required for Intel-based Motherboards only.
*   It's advised to map the USB ports during the post-install for a better USB speed, improved sleep/wake and hotplug.

**XHCI-Unsupported**  

*   Enables Intel USB Controllers on 9, 200, 300, 400, 500, and 600 series and some HEDT Motherboards such as X99 and X299.
*   Required for Intel-based Motherboards only.

**GenericUSBXHCI.kext**  

*   Enables non-Intel USB3 Controllers.
*   Usually found on Sandy Bridge, Ivy Bridge, and some Haswell
*   Required for non-Intel USB3 Controllers. Only use if you have a non-Intel USB3 Controller
*   Required for AMD Ryzen Laptops having Dual XHC Controllers.

**ASMedia.kext**  

*   Enables ASMedia USB3.1 Controllers.
*   Required for macOS Big Sur and later.
*   Required for ASMedia USB 3.1 Controller. Only use if you have an AsMedia USB3.1 Controller

**mXHCD.kext**  

*   Enables ASMedia USB3.0 1040/1042 Controllers.
*   Required for ASMedia USB 3.0 Controller. Only use if you have an AsMedia USB3.0 Controller

**XLNCUSBFIX**  

*   Fixes USB on AMD FX Systems
*   Do not use it on Ryzen Systems
*   Requires macOS 10.13 and later

**Audio**  

* * *

*   Required for Audio functioning.

**AppleALC.kext**  

*   Provides AppleHDA patching on macOS.
*   Enables onboard/built-in Audio.
*   Fixes various Audio related issues.
*   This kext requires `Lilu.kext` to function.

**AppleALCU.kext**  

*   Required for Onboard/built-in USB Audio.
*   This kext requires `Lilu.kext` to function.
*   Only use if you have an Onboard/built-in Realtek USB Audio Controller

**EMUUSBAudio**  

*   Required for Creative Labs EMU USB Audio.
*   Only use if you have Creative Labs EMU USB Audio Interface

**Ethernet**  

* * *

*   Required for Ethernet functioning.

**IntelMausi.kext**  

*   Enables Intel Family onboard Ethernet Controller.
*   Required for Intel-based Motherboards only.

**SmallTreeIntel82576.kext**  

**SmallTreeIntel8259x.kext**  

**LucyRTL8125Ethernet.kext**  

*   Enables Realtek RTL8125 2.5GBit Ethernet Family onboard Ethernet Controller.

**RealtekRTL8111.kext**  

*   Enables Realtek RTL8111/8168 Family onboard Ethernet Controller.

**RealtekRTL8100.kext**  

*   Enables RTL810X Fast Ethernet Family onboard Ethernet Controller.

**AtherosE2200Ethernet**  

*   Enables Qualcomm Atheros Killer Family onboard Ethernet Controller.

**AtherosL1cEthernet**  

*   Enables Atheros AR813x/815x onboard Ethernet Controller.
*   For newer chipsets, use `AtherosE2200.kext`.

**BCM5722D.kext**  

*   Enables Broadcom BCM5722 NetXtreme and NetLink family gigabit onboard Ethernet Controller.
*   For server-grade Broadcom chipsets, use `AzulNX2Ethernet.kext`.

**AzulNX2Ethernet**  

*   Enables QLogic (formerly Broadcom) NetXtreme II server-grade network cards.
*   For consumer-grade Broadcom chipsets, use `BCM5722D.kext`.

**AppleIntelE1000e.kext**  

*   Enables Intel Fast Ethernet onboard Ethernet Controller

**Intel82566MM**  

**NullEthernetInjector**  

*   Fake Ethernet.
*   Enables Mac App Store access.
*   Required for Laptops where you don't have a built-in Ethernet port or built-in WiFi with supporting drivers
*   Suitable for Third Party (non-native) USB WiFi Adapters.

**WiFi**  

* * *

*   Required for WiFi functioning.

**AirportBrcmFixup**  

*   Required for WiFi functioning of non-Apple/non-Fenvi Broadcom Cards
*   Requires a compatible Broadcom WiFi Card. Only use if you have a compatible Broadcom WiFi Card.
*   This kext requires Lilu.kext to function.

**AirportItlwm**  

*   Works as standalone kext.
*   Has AirDrop and Handoff support.
*   Works as WiFi Interface.
*   Requires a compatible Intel WiFi Card. Only use if you have a compatible Intel WiFi Card.

**itlwm**  

*   Similar to AirportItlwm.
*   Required where AirportIlwm is not compatible. Varies from system to system.
*   Requires HeliPort App, which acts as a WiFi Client for Intel WiFi.
*   Has no AirDrop or Handoff support.
*   Works as Ethernet Interface.
*   Requires a compatible Intel WiFi Card. Only use if you have a compatible Intel WiFi Card.

**Bluetooth**  

* * *

*   Required for Bluetooth functioning.

**BlueToolFixup**  

*   Skips Bluetool's firmware check
*   Required for Broadcom and Intel Bluetooth
*   Mandatory for macOS 12 (Monterey) and Later
*   Do not use it with `BrcmBluetoothInjector.kext` or `IntelBluetoothInjector.kext`
*   This kext requires Lilu.kext to function.

**BrcmBluetoothInjector**  

*   Bluetooth Injector for Broadcom
*   Required for OS X 10.11 (El Capitan) to macOS Big Sur
*   This kext requires `BrcmFirmwareData.kext` and `BrcmPatchRAM3` to function.
*   Do not use BrcmPatchRAM.kext or BrcmPatchRAM2.kext with this kext.

**BrcmBluetoothInjectorLegacy**  

*   Bluetooth Injector for Broadcom
*   Required for OS X 10.10 (Yosemite) and older

**BrcmFirmwareData**  

*   Enables Firmware Upload to Bluetooth Device.
*   Required for all non-Apple and non-Fenvi Cards
*   This kext can be injected via Bootloader and is the preferred configuration.
*   This kext requires `BrcmBluetoothInjector.kext` and `BrcmPatchRAM3.kext` to function.

**BrcmFirmwareRepo**  

*   Enables Firmware Upload to Bluetooth Device.
*   Required for all non-Apple and non-Fenvi Cards
*   Required for OS X 10.11 and later.
*   Must be installed in /S/L/E or /L/E.
*   This kext is slightly more memory efficient than `BrcmFirmwareData.kext`, but cannot be injected by a bootloader.

**BrcmNonPatchRAM2**  

*   Required for non-PatchRAM device
*   For OS X 10.11 (El Capitan) and Later

**BrcmNonPatchRAM**  

*   Required for non-PatchRAM device
*   For OS X 10.10 (Yosemite) and older

**BrcmPatchRAM3**  

*   For macOS Catalina and Later
*   This kext requires `BrcmBluetoothInjector.kext` and `BrcmFirmwareData.kext` to function.

**BrcmPatchRAM2**  

*   For OS X 10.11 (El Capitan) to macOS Mojave
*   This kext requires `BrcmBluetoothInjectorLegacy.kext` and `BrcmFirmwareRepo.kext` to function.

**BrcmPatchRAM**  

*   For OS X 10.10 and older
*   This kext requires `BrcmBluetoothInjectorLegacy.kext` and `BrcmFirmwareRepo.kext` to function.

**IntelBluetoothInjector**  

*   Bluetooth Injector for Intel
*   Required for macOS High Sierra to macOS Big Sur only. For latter, use BlueToolFixup.
*   Do not use his kext on macOS Monterey and newer.
*   This kext requires `IntelBluetoothFirmware.kext` to function.

**IntelBluetoothFirmware**  

*   Enables Intel Bluetooth.
*   Enables Firmware Upload to Bluetooth Device.
*   Required for all compatible Intel Bluetooth

**IntelBTPatcher**  

*   Fixes several Bluetooth bugs for Intel Bluetooth on macOS.
*   On Monterey and newer, this provides a workaround for a bug in `bluetoothd` by initializing the Bluetooth module properly before attempting a LE scan.
*   Prior to Monterey, this provides a workaround for a bug in the `IOBluetoothHostController` that prevents certain Bluetooth 4.X devices from connecting.
*   This kext requires `Lilu.kext` to function.

  
**Storage**  

* * *

*   Required for SATA/NVMe Controller functioning.

**EmeraldSDHC**  

*   Enables eMMC/MMC Cards complying with the Secure Digital Host Controller (SDHC) specification under macOS.
*   Only supports eMMC/MMC cards at up to HS200 speeds.
*   Due to work in progress, you may experience a poor performance or even non-functional on some hardware.
*   SD Cards are not supported by this driver.

**NVMeFix**  

*   Fixes Power Management for NVMe Drives.
*   This kext may slower the R/W speeds of your NVMe Drives.
*   Not recommended for NVMe Drives in RAID Card such as High Point SSD7000.
*   This kext requires `Lilu.kext` to function.

**SATA-Unsupported**  

*   Enables certain SATA Controllers under macOS.
*   _**Certain SATA Controllers may not be recognised by the installer and due to this, you may not be able to see the target drive or the other drive connected to the particular controller.**_
*   _**Such SATA Controllers are not recognised by the SATA kexts provided by Apple and this is due to Apple doesn't uses that specific controller in their hardware.**_
*   _**If you're having a similar issue, you'll need to use this kext to overcome this issue.**_
*   _**_**Use "SATA-unsupported.kext" for macOS Sierra and later.**_**_

**CtlnaAHCIPort**  

*   Similar to SATA-Unsupported.kext
*   Required for macOS Big Sur and Later.

**ACHIPortInjector**  

*   Enables unsupported SATA Controller on macOS

**ATAPortInjector**  

*   Enables unsupported IDE and ATA Controller on macOS

**SASMegaRAID**  

*   Enables LSI RAID 8xxx/92xxx Controllers.
*   Enables Dell PERC 5, PERC6, H310, H700 and H800
*   Enables IBM ServeRAID M1115
*   Enables Intel RAID SRCSAS18E/SRCSAS144E
*   Does not supports LSI Controllers with non-MegaRAID firmware

**DiskArbitrationFixup**  

*   Disables the uninitialized disk message for an uninitialized disk
*   Removes `The disk you inserted was not readable by this computer` message when an uninitialized disk is connected.

**Innie**  

*   Makes PCIe drives appear as Internal
*   An alternative option is to add the built-in device property for each drive installed.

  
**Keyboard/Trackpad/Touchscreen**  

* * *

*   Required for input functioning.
*   Only for Laptops and mobile devices.

**VoodooPS2Controller**  

*   Enables PS/2 Keyboard, Mouse, and Trackpad
*   Enables multi-gestures for Trackpad.

**ApplePS2SmartTouchpad**  

*   Multitouch driver for ELAN, FocalTech, and Synaptics Touchpad
*   Similar to VoodooPS2Controller

**VoodooInput**  

*   Enables Magic Trackpad 2 software emulation for arbitrary input sources like VoodooPS2.
*   Requires VoodooPS2.kext or VoodooI2c.kext to function.

**VoodooRMI**  

*   Required for Laptops with Synaptics SMBus Trackpads
*   Requires `VoodooPS2Controller.kext` and `VoodooSMBus.kext` for SMBus based Trackpads to function.
*   Requires `VoodooI2C.kext` for I2C-based Synaptics Trackpads to function.
*   Similar to VoodooPS2Controller.kext

**VoodooSMBus**  

*   Required for Laptops with ELAN SMBus Trackpads.
*   Enables multi-gestures for Trackpad.
*   Similar to VoodooPS2Controller
*   Provides support for the SMBUs capabilities of Intel I/O Controller Hubs (ICH), also called i801 SMBus.
*   Do not use `VoodooPS2Controller.kext` with this kext.

**VoodooI2C**  

*   Enables I2C functioning.
*   Required for Laptops with I2C Trackpads and Touchscreens.
*   Requires a Satellite kext to function \[`VoodooI2CHID`, `VoodooI2CAtmelMXT`, `VoodooI2CELAN`, `VoodooI2CFTE` or `VoodooI2CSynaptics`\]
*   Enables multi-gestures for Trackpad.
*   Requires DSDT patching to work.
*   Requires a working Battery meter for enabling Trackpad Preferences.

**VoodooI2CHID**  

*   Satellite kext.
*   Adds support for I2C-HID device.
*   Required for Laptops with I2C Trackpads and Touchscreens.
*   Common Satellite kext for VoodooI2C.kext
*   Requires `VoodooI2C.kext` to function

**VoodooI2CAtmelMXT**  

*   Satellite kext.
*   Required for Laptops with Atmel Multitouch I2C Device.
*   Requires `VoodooI2C.kext` to function

**VoodooI2CELAN**  

*   Satellite kext
*   Required for Laptops with ELAN I2C Device.
*   ELAN1200 and later requires VoodooI2CHID.kext to function
*   Requires `VoodooI2C.kext` to function

**VoodooI2CFTE**  

*   Satellite kext
*   Required for Laptops with FTE I2C Device.
*   Requires `VoodooI2C.kext` to function

**VoodooI2CSynaptics**  

*   Satellite kext
*   Required for Laptops with Synaptics I2C Device.
*   Requires `VoodooI2C.kext` to function

**AlpsHID**  

*   Satellite kext
*   Required for Laptops with Alps I2C Device.
*   Requires `VoodooI2C.kext` to function
*   Requires AlpsHID VoodI2CHID version.

**SerialMouse**  

*   Enables Serial Mouse that uses the Microsoft Serial Mouse protocol
*   Requires Mice to be connected before macOS is booted or it will not be detected.
*   Does not supports HotPlug.

**GK701HIDDevice**  

*   Fixes two different keys registering instead of one for certain MSI GK-701 and related Keyboards.
*   Requires SIP disabled.

**BrightnessKeys**  

*   Enables Automatic handling of Brightness Hotkeys.
*   Requires `Lilu.kext` to function

**NoTouchID**  

*   Disables Touch ID support on macOS when a compatible board ID is used.
*   Fixes lag in authentication dialogs and delay in the login screen by disabling the Touch ID support.
*   **_This kext is not required on 10.15.7 and later._**
*   This kext requires `Lilu.kext` to function.

**Battery**  

* * *

  
**SMCBatteryManager.kext**  

*   Enables Battery readings
*   Required for Laptops only.
*   Requires DSDT patching for battery meter.

**Card Reader**  

* * *

*   Required for Card Reader functioning.

**Sinetek-rtsx**  

*   Enables Realtek SD Card Reader

**RealtekCardReader**  

*   Enables Realtek PCIe/USB-based SD card readers
*   Requires `RealtekCardReaderFriend.kext` to function

**RealtekCardReaderFriend**  

*   Supporting kext for `RealtekCardReader.kext`
*   Enables Card Reader information under `System Information>Card Reader`
*   Helps to recognize the Card Reader as a native one.
*   Requires `RealtekCardReader.kext` and `Lilu.kext` to function.

**JMB38X**  

*   Enables JMicron based Card Readers.
*   Requires `HSSDBlockStorage.kext` to function.

**HSSDBlockStorage**  

*   Supporting kext for `JMB38X.kext`.
*   Requires `JMB38X.kext` to function.

**GenericCardReaderFriend**  

*   Enables Card Reader information under System Information>Card Reader
*   Required for built-in USB Card Reader which works OOTB but does not show up in System Information.
*   Do not use this kext if your Card Reader is powered by either `AppleUSBCardReader.kext` or `RealtekCardReader.kext`
*   Requires specifying the Vendor and the Product ID of your Card Reader.
*   Requires `Lilu.kext` to function

**Sleep/Wake**  

* * *

**HibernationFixup.kext**  

*   Enables sync between RTC variables and NVRAM
*   Enables native hibernation on systems with hardware NVRAM
*   Supports Hibdernation modes 3 and 25

**USBWakeFixup.kext**  

*   Creates fake ACPI device with the right wakeup params
*   Fixes display on wakeup from a USB device.
*   Requires SSDT-USBW.aml to function.

  
**Thunderbolt**  

* * *

  
**ThunderboltReset**  

*   Disables the ICM in the Alpine Ridge Controller for OS X/macOS to take over as the LC.
*   Requires `Lilu.kext` to function.

**IOElectrify**  

*   Enables always-on power to Intel Thunderbolt hardware

**Kryptonite**  

*   Enables eGPUs on macOS using Thunderbolt 1 and 2.
*   Intended for real Macs without compromising Security features such as System Integrity Protection, FileVault and Authenticated-Root.
*   Supports NVIDIA/AMD GPUs.
*   Requires `Lilu.kex`t to function.

  
**Virtualization**  

* * *

**MacHyperVSupport**  

*   Provides Hyper-V integration services for macOS
*   Requires `SSDT-HV-CPU.aml`, `SSDT-HV-PLUG.aml` and `SSDT-HV-VMBUS.aml` to function.
*   Requires Windows 8.1 and above

**Sensors and Monitoring**  

* * *

**SMCProcessor.kext**  

*   Enables CPU Temperature for monitoring.
*   Required for CPU sensing.
*   Required for Intel CPUs only.
*   Available in VirtualSMC package.

**SMCSuperIO.kext**  

*   Enables FAN Speed for monitoring.
*   Required for fans reading.
*   Required for Intel CPUs only.

**SMCAMDProcessor.kext**  

*   Enables Power Management and Monitoring for AMD CPUs.
*   Requires `AMDRyzenCPUPowerManagement.kext` and `VirtualSMC.kext` to function.

**RadeonSensor.kext**  

*   Enables Radeon GPUs temperature monitoring.

**SMCLightSensor.kext**  

*   Required for Ambient Light Sensor
*   Required for Laptops only.
*   Available in VirtualSMC package.

**SMCDellSensors.kext**  

*   Enables finer monitoring and Control of FANs
*   Only required for Dell systems supporting System Management Mode (SMM).
*   Available in VirtualSMC package.

**Miscellaneous**  

* * *

**RestrictEvents.kext**  

*   Blocks unwanted processes causing compatibility issues on different hardware and unlocks the support for certain features restricted to other hardware
*   Fixes ExpansionSlotNotification and MemorySlotNotification on MacPro7,1 SMBIOS.
*   Required for MacPro7,1 SMBIOS.

**FeatureUnlock.kext**  

*   Enables Sidecar, NightShift, AirPlay, and Universal Control

**RTCMemoryFixup**  

*   Emulates some offsets in CMOS (RTC) memory
*   Helps to avoid some conflicts between macOS AppleRTC and BIOS/firmware of your System.

**SystemProfilerMemoryFixup**  

*   Enables memory tab on systems when using MacBook SMBIOS which have soldered RAM.
*   Requires `Lilu.kext` to function

**TOSMotionSensor**  

*   Enables accelerometer for Toshiba Laptops.
*   This kext does not offer HDD protection.

  
**Debugging**  

* * *

**DebugEnhancer**  

*   Enables Debug putout in the macOS Kernel.
*   Requires `Lilu.kext` to function

  

**NOTES:**  

*   **Do not download the project files. The pre-built binaries/downloads are available in the README.md section. Make sure you read it carefully.**
*   **Download the latest version for better support.**
*   **Use the Kexts from the RELEASE folder and the RELEASE.zip file.**
*   **If you're following the process on Windows or Linux, you'll see the Kexts like normal folders.**
*   **Only copy the files with the `.kext` Extension.**
*   **Do not copy the `.dsYM` file from the RELEASE folder. They're only for debugging purposes.**
*   **Do not place unnecessary kexts here. It might prevent booting the installer.**
*   **Only use basic kexts which are required to boot off the installer. You can install the rest required kexts at the time of post-installation.**
*   **The VirtualSMC package includes Battery and Sensors plugins (SMCBatteryManager.kext, SMCDellSensors.kext, SMCLightSensor.kext, SMCProcessor.kext, and SMCSuperIO.kext). You do not need these kexts for booting the installer.**
*   **Do not use "SATA-unsupported.kext" and "CtlnaAHCIPort.kext" together.**
*   **The Optional kexts are purely dependent on the hardware you have. If you're having matching hardware, then only add the kexts otherwise adding unnecessary kexts isn't a good idea and is highly discouraged.**

  
Here's what your EFI somewhat look like:

Now that you have obtained the OpenCore Base EFI and gathered SSDTs (.aml), Drivers (.efi) and Kexts (.kext), the resulting EFI folder in your working directory should look something like this:

Last edited: Oct 11, 2023

[![EliteMacx86](https://elitemacx86.com/data/avatars/m/0/1.jpg?1683055052)](https://elitemacx86.com/members/elitemacx86.1/)

Joined

Jul 22, 2018

Messages

7,500

Motherboard

Supermicro X11SPA-T

CPU

Intel Xeon W-3275 28 Core

Graphics

2xAMD RX 580 8GB

OS X/macOS

13.x

Bootloader

1.  OpenCore (UEFI)

Mac

1.  Mac mini
2.  MacBook Pro

Mobile Phone

1.  Android
2.  iOS

*   [](https://elitemacx86.com/threads/how-to-boot-macos-installer-on-desktops-and-laptops-using-opencore-uefi-legacy-opencore-install-guide.950/post-7590)
*   [#5](https://elitemacx86.com/threads/how-to-boot-macos-installer-on-desktops-and-laptops-using-opencore-uefi-legacy-opencore-install-guide.950/post-7590)

**CHAPTER 5: Setting up config.plist**  

* * *

Now, as you have things in order, the next step is to set up config.plist. It is highly recommended to create your own config.plist as the hardware differs. Copying from somewhere else isn't a good idea. You can obtain a sample.plist which is bundled with OpenCore pkg and this sample.plist will be used as a base for setting up config.plist.

**I. Obtaining Sample config.plist**

1\. As you already have downloaded OpenCore Pkg, simply copy `sample.plist` from `OpenCore/RELEASE/Docs` to `EFI/OC` working directory and rename it to `config.plist`. The directory structure for OpenCore Pkg for Docs has been provided below.

```
Docs
├── AcpiSamples
│   ├── Binaries
│   └── Source
├── Changelog.md
├── configuration.pdf
├── Differences.pdf
├── Sample.plist
└── SampleCustom.plist
```


  

**NOTE:**  

*   **To avoid any conflicts and outdated settings, OpenCore and sample.plist should be from the same RELEASE version and should not mismatch.**

  
Now, as we have setup OpenCore and the required SSDTs (.aml), Drivers (.efi), Kexts (.kext) and config (.plist), the resulting EFI folder in your working directory should look something like this:

```
EFI
├── BOOT
│   └── BOOTx64.efi
└── OC
    ├── ACPI
    │   ├── SSDT-AWAC-DISABLE.aml
    │   ├── SSDT-EC-USBX.aml
    │   └── SSDT-PLUG.aml
    ├── Drivers
    │   ├── OpenRuntime.efi
    │   ├── OpenHfsPlus.efi
    │   └── ResetNvramEntry.efi
    ├── Kexts
    │   ├── Lilu.kext
    │   ├── VirtualSMC.kext
    │   ├── WhateverGreen.kext
    │   ├── AppleALC.kext
    │   ├── USBInjectAll.kext
    │   └── RealtekRTL8111.kext
    ├── Resources
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
    ├── config.plist
    └── OpenCore.efi
```


  

**NOTE:**  

*   **Your EFI might differ as each system is different and will have different requirements.**

  
**II. Cleaning up config.plist**  

* * *

Now before you start working with your config.plist, it is highly recommended to clean up the config.plist to have the required settings only. A basic clean-up is required as by default, OpenCore includes numerous settings, for several purposes and all of them may not be required by a particular system. You need to use the settings required by your system and remove all irrelevant entries and settings from your config.plist. Although, most of such settings are already off, but do exist. Keeping such settings will not harm but to reduce the size and have less clutter, it is strongly advised to delete the irrelevant settings which you don't need.

Starting from this step, you'll need to use a tool to edit the config.plist and we'll use OC Auxiliary Tools to edit the config.plist and configure it accordingly. With OC Auxiliary Tools, you have the advantage of Cross-Platform i.e. you can use the tool on Windows, Mac, and Linux. To edit your OpenCore config.plist using OC Auxiliary Tools, follow the steps below.

1\. Download OC Auxiliary Tools from the downloads section of this forum.

**For macOS**  
Mount the .DMG file by openening the .dmg file  
Move the OCAuxiliaryTools to your Applications folder  
Open OCAuxiliaryTools.app to launch the Application

**For Windows**  
Extract the zip and you'll get the OCAT-Win64 folder  
Use OCAuxiliaryTools.exe to launch the Application

**For Linux**

**Update OC Auxiliary Tools**

  
Perform an upgrade check for OC Auxiliary Tools App using Help>Download Upgrade Packages. If there's any available package for an upgrade, it will download it. Once it finishes downloading, click on `Close and start upgrade` and the App will upgrade to its latest version. After the upgrade, when the App reopens, check for an update using Help> Update Check. If you're using the latest version, you'll see the following

Note that you don't need to follow this step if you have downloaded the latest OC Auxiliary Tools.

**Update OpenCore Package**  
Now, click on the Sync icon Upgrade OpenCore and Kexts icon and you'll see something similar to the following  
Select Latest Version from Choose OpenCore Version option.  
Click on Get OpenCore and it will update the OpenCore Database.  
Once the OpenCore Database is updated, you'll see the following

Open your config.plist from the directory `EFI/OC/` using OC Auxiliary Tools

**ACPI**

* * *

  
**Add**

1\. Select all the ACPI entries using Command+A or CTRL+A on your Keyboard.  
[![Screen Shot 2022-07-07 at 10.25.17 PM-min.png](https://elitemacx86.com/data/attachments/4/4729-e08fa02c1fd898ee3d27f4094912995e.jpg "Screen Shot 2022-07-07 at 10.25.17 PM-min.png")](https://elitemacx86.com/attachments/screen-shot-2022-07-07-at-10-25-17-pm-min-png.4721/)  
2\. Click on the delete button to remove all the entries and you should have all the entries removed.  
[![Screen Shot 2022-07-07 at 10.20.09 PM-min.png](https://elitemacx86.com/data/attachments/4/4730-a98b6ee8d11c6a62b5cbf09dac0c5450.jpg "Screen Shot 2022-07-07 at 10.20.09 PM-min.png")](https://elitemacx86.com/attachments/screen-shot-2022-07-07-at-10-20-09-pm-min-png.4722/)  
**Delete**

1\. Move to the Delete Tab and select all the entries using Command+A or CTRL+A on your Keyboard  
[![Screen Shot 2022-07-07 at 10.21.47 PM-min.png](https://elitemacx86.com/data/attachments/4/4731-07d55c4a06a5e7ec6004fa0df69fa078.jpg "Screen Shot 2022-07-07 at 10.21.47 PM-min.png")](https://elitemacx86.com/attachments/screen-shot-2022-07-07-at-10-21-47-pm-min-png.4723/)  
2\. Click on the delete button to remove all the entries and you should have all the entries removed.  
[![Screen Shot 2022-07-07 at 10.22.38 PM-min.png](https://elitemacx86.com/data/attachments/4/4732-0b4c1be01c390526d8cdde2a5c68906a.jpg "Screen Shot 2022-07-07 at 10.22.38 PM-min.png")](https://elitemacx86.com/attachments/screen-shot-2022-07-07-at-10-22-38-pm-min-png.4724/)  
**Patch**

1\. Move to Patch Tab and select all the entries using Command+A or CTRL+A on your Keyboard  
[![Screen Shot 2022-07-07 at 10.23.23 PM-min.png](https://elitemacx86.com/data/attachments/4/4733-d33b6ad094ee7e89d4acdd0a1cc72738.jpg "Screen Shot 2022-07-07 at 10.23.23 PM-min.png")](https://elitemacx86.com/attachments/screen-shot-2022-07-07-at-10-23-23-pm-min-png.4725/)  
2\. Click on the delete button to remove all the entries and you should have all the entries removed.  
[![Screen Shot 2022-07-07 at 10.23.47 PM-min.png](https://elitemacx86.com/data/attachments/4/4734-2b4bee2eb380cb5d061ebf38e8a34071.jpg "Screen Shot 2022-07-07 at 10.23.47 PM-min.png")](https://elitemacx86.com/attachments/screen-shot-2022-07-07-at-10-23-47-pm-min-png.4726/)

Quirks  
Remove ResetLogoStatus

**Booter**

* * *

  
**MmioWhitelist**

1\. Select all the entries using Command+A or CTRL+A on your Keyboard  
[![Screen Shot 2022-07-08 at 7.53.50 AM-min.png](https://elitemacx86.com/data/attachments/4/4735-3bc4a26dcc1f3449f03835a4802dab66.jpg "Screen Shot 2022-07-08 at 7.53.50 AM-min.png")](https://elitemacx86.com/attachments/screen-shot-2022-07-08-at-7-53-50-am-min-png.4727/)  
2\. Click on the delete button to remove all the entries and you should have all the entries removed.  
[![Screen Shot 2022-07-08 at 7.53.59 AM-min.png](https://elitemacx86.com/data/attachments/4/4736-da3c2e06889ab9a0aec728ef832592a7.jpg "Screen Shot 2022-07-08 at 7.53.59 AM-min.png")](https://elitemacx86.com/attachments/screen-shot-2022-07-08-at-7-53-59-am-min-png.4728/)  
**Patch**

1\. Move to Patch Tab and select all the entries using Command+A or CTRL+A on your Keyboard  
[![Screen Shot 2022-07-08 at 7.54.14 AM-min.png](https://elitemacx86.com/data/attachments/4/4737-f2c389aaf56cac74f7cf77e4f55a6792.jpg "Screen Shot 2022-07-08 at 7.54.14 AM-min.png")](https://elitemacx86.com/attachments/screen-shot-2022-07-08-at-7-54-14-am-min-png.4729/)  
2\. Click on the delete button to remove all the entries and you should have all the entries removed.  
[![Screen Shot 2022-07-08 at 7.54.32 AM-min.png](https://elitemacx86.com/data/attachments/4/4738-1d3eff64aef5855615bba44c30994e71.jpg "Screen Shot 2022-07-08 at 7.54.32 AM-min.png")](https://elitemacx86.com/attachments/screen-shot-2022-07-08-at-7-54-32-am-min-png.4730/)

**DP (Device Properties)**

* * *

  
**Add**

1\. Select all the Device entries using Command+A or CTRL+A on your Keyboard  
[![Screen Shot 2022-07-08 at 7.55.17 AM-min.png](https://elitemacx86.com/data/attachments/4/4739-ed88fabd38b71b13e9b8bccef4be5a3a.jpg "Screen Shot 2022-07-08 at 7.55.17 AM-min.png")](https://elitemacx86.com/attachments/screen-shot-2022-07-08-at-7-55-17-am-min-png.4731/)  
2\. Click on the delete button to remove all the entries and you should have all the entries removed.  
[![Screen Shot 2022-07-08 at 7.55.26 AM-min.png](https://elitemacx86.com/data/attachments/4/4740-5b427f15c6dd0c828d5ebea5cc8580e0.jpg "Screen Shot 2022-07-08 at 7.55.26 AM-min.png")](https://elitemacx86.com/attachments/screen-shot-2022-07-08-at-7-55-26-am-min-png.4732/)

**Kernel**

* * *

  
**Add**

1\. Select all the Kext entries using Command+A or CTRL+A on your Keyboard  
[![Screen Shot 2022-07-08 at 7.56.13 AM-min.png](https://elitemacx86.com/data/attachments/4/4741-b2b3bbda63084eddee66d35f5ad2a59b.jpg "Screen Shot 2022-07-08 at 7.56.13 AM-min.png")](https://elitemacx86.com/attachments/screen-shot-2022-07-08-at-7-56-13-am-min-png.4733/)  
2\. Click on the delete button to remove all the entries and you should have all the entries removed.  
[![Screen Shot 2022-07-08 at 7.56.38 AM-min.png](https://elitemacx86.com/data/attachments/4/4742-4236f0659d4f3e51262882c6b4271730.jpg "Screen Shot 2022-07-08 at 7.56.38 AM-min.png")](https://elitemacx86.com/attachments/screen-shot-2022-07-08-at-7-56-38-am-min-png.4734/)  
**Block**

1\. Move to Patch Tab and select all the entries using Command+A or CTRL+A on your Keyboard  
[![Screen Shot 2022-07-08 at 7.56.50 AM-min.png](https://elitemacx86.com/data/attachments/4/4743-016efbb2101aaf4420d3173200e8f39f.jpg "Screen Shot 2022-07-08 at 7.56.50 AM-min.png")](https://elitemacx86.com/attachments/screen-shot-2022-07-08-at-7-56-50-am-min-png.4735/)  
2\. Click on the delete button to remove all the entries and you should have all the entries removed.  
[![Screen Shot 2022-07-08 at 7.56.56 AM-min.png](https://elitemacx86.com/data/attachments/4/4744-b25be9b21b63529fa0d4c1ddc26ffe04.jpg "Screen Shot 2022-07-08 at 7.56.56 AM-min.png")](https://elitemacx86.com/attachments/screen-shot-2022-07-08-at-7-56-56-am-min-png.4736/)  
**Force**

1\. Move to Patch Tab and select all the entries using Command+A or CTRL+A on your Keyboard  
[![Screen Shot 2022-07-08 at 7.57.18 AM-min.png](https://elitemacx86.com/data/attachments/4/4745-a6c4f54353c6628034495e4e6638e33e.jpg "Screen Shot 2022-07-08 at 7.57.18 AM-min.png")](https://elitemacx86.com/attachments/screen-shot-2022-07-08-at-7-57-18-am-min-png.4737/)  
2\. Click on the delete button to remove all the entries and you should have all the entries removed.  
[![Screen Shot 2022-07-08 at 7.57.25 AM-min.png](https://elitemacx86.com/data/attachments/4/4746-0e1ddf58fe04a4a961b3682cd04fafe2.jpg "Screen Shot 2022-07-08 at 7.57.25 AM-min.png")](https://elitemacx86.com/attachments/screen-shot-2022-07-08-at-7-57-25-am-min-png.4738/)  
**Patch**

1\. Move to Patch Tab and select all the entries using Command+A or CTRL+A on your Keyboard  
[![Screen Shot 2022-07-08 at 7.57.40 AM-min.png](https://elitemacx86.com/data/attachments/4/4747-de78a9795ddaa727c9b2e778a7b4efc6.jpg "Screen Shot 2022-07-08 at 7.57.40 AM-min.png")](https://elitemacx86.com/attachments/screen-shot-2022-07-08-at-7-57-40-am-min-png.4739/)  
2\. Click on the delete button to remove all the entries and you should have all the entries removed.  
[![Screen Shot 2022-07-08 at 7.57.49 AM-min.png](https://elitemacx86.com/data/attachments/4/4748-4500b51b1cc3517292efc849f0bb642f.jpg "Screen Shot 2022-07-08 at 7.57.49 AM-min.png")](https://elitemacx86.com/attachments/screen-shot-2022-07-08-at-7-57-49-am-min-png.4740/)  
**Misc**

* * *

  
**Entries**

1\. Select all the Boot entries using Command+A or CTRL+A on your Keyboard  
[![Screen Shot 2022-07-08 at 8.00.46 AM-min.png](https://elitemacx86.com/data/attachments/4/4749-ce728244d20357a685301695e0cbd915.jpg "Screen Shot 2022-07-08 at 8.00.46 AM-min.png")](https://elitemacx86.com/attachments/screen-shot-2022-07-08-at-8-00-46-am-min-png.4741/)  
2\. Click on the delete button to remove all the entries and you should have all the entries removed.  
[![Screen Shot 2022-07-08 at 8.00.53 AM-min.png](https://elitemacx86.com/data/attachments/4/4750-3a44881340d298e4085be04ae6cb45a4.jpg "Screen Shot 2022-07-08 at 8.00.53 AM-min.png")](https://elitemacx86.com/attachments/screen-shot-2022-07-08-at-8-00-53-am-min-png.4742/)  
**Tools**

1\. Move to Tools Tab and select all the Tools entries using Command+A or CTRL+A on your Keyboard  
[![Screen Shot 2022-07-08 at 8.01.23 AM-min.png](https://elitemacx86.com/data/attachments/4/4751-3f4d2b425027fbf6cac4eef653fba293.jpg "Screen Shot 2022-07-08 at 8.01.23 AM-min.png")](https://elitemacx86.com/attachments/screen-shot-2022-07-08-at-8-01-23-am-min-png.4743/)  
2\. Click on the delete button to remove all the entries and you should have all the entries removed.  
[![Screen Shot 2022-07-08 at 8.03.04 AM-min.png](https://elitemacx86.com/data/attachments/4/4752-b91ffb202051008392e643c09c764cc8.jpg "Screen Shot 2022-07-08 at 8.03.04 AM-min.png")](https://elitemacx86.com/attachments/screen-shot-2022-07-08-at-8-03-04-am-min-png.4744/)  
**NVRAM**

* * *

  
**Add**

1\. Select the UUID `7C436110-AB2A-4BBB-A880-FE41995C9F82`  
[![Screen Shot 2022-07-08 at 8.03.43 AM-min.png](https://elitemacx86.com/data/attachments/4/4753-63746255d1f542393f86ac87f5f13c65.jpg "Screen Shot 2022-07-08 at 8.03.43 AM-min.png")](https://elitemacx86.com/attachments/screen-shot-2022-07-08-at-8-03-43-am-min-png.4745/)  
2\. From the right pane, select the entry shown and click on the delete button to remove the key and you should have the key entry removed.  
[![Screen Shot 2022-07-08 at 8.04.04 AM-min.png](https://elitemacx86.com/data/attachments/4/4754-f50f8ece7421b8c77b1073b27ac5b089.jpg "Screen Shot 2022-07-08 at 8.04.04 AM-min.png")](https://elitemacx86.com/attachments/screen-shot-2022-07-08-at-8-04-04-am-min-png.4746/)  
3\. From the right pane, select the value for the Key `prev-lang:kbd` and click on the Delete button to delete the value.  
[![Screen Shot 2022-07-08 at 8.04.26 AM-min.png](https://elitemacx86.com/data/attachments/4/4755-b580388d0a612d28b7e33c0d825c8c2d.jpg "Screen Shot 2022-07-08 at 8.04.26 AM-min.png")](https://elitemacx86.com/attachments/screen-shot-2022-07-08-at-8-04-26-am-min-png.4747/)  
[![Screen Shot 2022-07-08 at 8.04.38 AM-min.png](https://elitemacx86.com/data/attachments/4/4756-69f572d4d957c1c09a6596e0501e26c8.jpg "Screen Shot 2022-07-08 at 8.04.38 AM-min.png")](https://elitemacx86.com/attachments/screen-shot-2022-07-08-at-8-04-38-am-min-png.4748/)

**UEFI**

* * *

  
**Drivers**

1\. Select all the Drivers entries using Command+A or CTRL+A on your Keyboard  
[![Screen Shot 2022-07-08 at 9.17.20 AM-min.png](https://elitemacx86.com/data/attachments/4/4757-80b423c632ec09b90304c102efd5ae86.jpg "Screen Shot 2022-07-08 at 9.17.20 AM-min.png")](https://elitemacx86.com/attachments/screen-shot-2022-07-08-at-9-17-20-am-min-png.4749/)  
2\. Click on the delete button to remove all the entries and you should have all the entries removed.  
[![Screen Shot 2022-07-08 at 8.05.40 AM-min.png](https://elitemacx86.com/data/attachments/4/4758-f25f6f5caa5a60ac6c189728d413c79c.jpg "Screen Shot 2022-07-08 at 8.05.40 AM-min.png")](https://elitemacx86.com/attachments/screen-shot-2022-07-08-at-8-05-40-am-min-png.4750/)  
**ReservedMemory**

1\. Move to Patch Tab and select all the entries using Command+A or CTRL+A on your Keyboard  
[![Screen Shot 2022-07-08 at 8.05.53 AM-min.png](https://elitemacx86.com/data/attachments/4/4759-66b5257726fe3d375b9e484d49bb5868.jpg "Screen Shot 2022-07-08 at 8.05.53 AM-min.png")](https://elitemacx86.com/attachments/screen-shot-2022-07-08-at-8-05-53-am-min-png.4751/)  
2\. Click on the delete button to remove all the entries and you should have all the entries removed.  
[![Screen Shot 2022-07-08 at 8.06.00 AM-min.png](https://elitemacx86.com/data/attachments/4/4760-755e3a2fdf707c31ed6eaf23497a46fc.jpg "Screen Shot 2022-07-08 at 8.06.00 AM-min.png")](https://elitemacx86.com/attachments/screen-shot-2022-07-08-at-8-06-00-am-min-png.4752/)

Finally, Save your config.plist using the Menu `File>Save` option

**III. Adding SSDTs, Kexts and Drivers in config.plist**

* * *

Now, as we have got a cleaned-up config.plist, we can start adding the SSDTs (.aml), Kexts (.kext) and Drivers (.efi) to our config.plist. Unlike Clover, OpenCore requires the entries of the particular SSDTs, Kexts and Drivers in the appropriate section of the config.plist which are present in their respective directories. This linking step is necessary as OpenCore requires the entries to be present in the config.plist and without them, OpenCore will simply not boot to the target OS. To add the SSDTs, Kexts and Drivers, follow the steps below.

1\. Open your config.plist using OC Auxiliary Tools from `EFI/OC` directory.

**SSDTs**

1\. In the Add Tab, click on Add button and select all the .aml files from `EFI/OC/ACPI` directory and you should have all the .aml files entries added.  
[![Screen Shot 2022-07-11 at 11.36.09 PM-min.png](https://elitemacx86.com/data/attachments/4/4761-51111914a7f5e2b52983e6573daaed68.jpg "Screen Shot 2022-07-11 at 11.36.09 PM-min.png")](https://elitemacx86.com/attachments/screen-shot-2022-07-11-at-11-36-09-pm-min-png.4753/)

**NOTES:**  

*   **The .aml files must exist in EFI/OC/ACPI directory**
*   **As the ACPI loading order is important for OpenCore, it is advised to load the SSDTs in sorted order. This means all the necessary SSDTs should be loaded first and dependencies SSDTs should load later to avoid issues.**
*   **Your entries might differ as each system is different and will have different requirements.**

  
**Kexts**

1\. In the Add tab, click on Add button and select all the .kext files from `EFI/OC/Kexts` directory and you should have all the .kext files entries added.  
[![Screen Shot 2022-07-11 at 11.41.24 PM-min.png](https://elitemacx86.com/data/attachments/4/4762-bcaa9f6e37ab190dc8b2f2c00ef3eb6f.jpg "Screen Shot 2022-07-11 at 11.41.24 PM-min.png")](https://elitemacx86.com/attachments/screen-shot-2022-07-11-at-11-41-24-pm-min-png.4754/)  
2\. Arrange the kexts using arrow buttons. `<` button for up and `>` for down  
[![Screen Shot 2022-07-11 at 11.41.52 PM-min.png](https://elitemacx86.com/data/attachments/4/4763-71a3f7c963458823d929253bbdc0606a.jpg "Screen Shot 2022-07-11 at 11.41.52 PM-min.png")](https://elitemacx86.com/attachments/screen-shot-2022-07-11-at-11-41-52-pm-min-png.4755/)

**NOTE:**  

*   **The .kext files must exist in EFI/OC/Kexts directory**
*   **As the kext loading order is important for OpenCore, it is advised to load the kexts in sorted order. This means all the necessary kexts should be loaded first and dependencies kexts should load later to avoid issues.**
*   **Your entries might differ as each system is different and will have different requirements.**

  
**Drivers**

1\. In the Drivers tab, click on Add button and select all the .efi files from `EFI/OC/Drivers` directory and you should have all the .efi files entries added.  
[![Screen Shot 2022-07-11 at 11.46.05 PM-min.png](https://elitemacx86.com/data/attachments/4/4764-81c822c06fd7e4edf0b526a55b5c0969.jpg "Screen Shot 2022-07-11 at 11.46.05 PM-min.png")](https://elitemacx86.com/attachments/screen-shot-2022-07-11-at-11-46-05-pm-min-png.4756/)  
2\. Arrange the drivers using arrow buttons. `<` button for up and `>` for down  
[![Screen Shot 2022-07-11 at 11.46.15 PM-min.png](https://elitemacx86.com/data/attachments/4/4765-9d0146924685c6c0d03b67c958ac69ff.jpg "Screen Shot 2022-07-11 at 11.46.15 PM-min.png")](https://elitemacx86.com/attachments/screen-shot-2022-07-11-at-11-46-15-pm-min-png.4757/)

> **NOTES:**  
> 
> *   **The .efi files must exist in EFI/OC/Drivers directory**
> *   **As the driver loading order is important for OpenCore, it is advised to load the drivers in sorted order. This means all the necessary drivers should be loaded first.**
> *   **Your entries might differ as each system is different and will have different requirements.**
> *   **The config.plist must match the SSDTs, Kexts, and Drivers in the respective directories of the EFI folder. If you tend to delete a file in these directories and the entries are left linked in the config.plist, OpenCore will halt and will not boot further.**

  
2\. Finally, Save your config.plist using the Menu `File>Save` option.

[![EliteMacx86](https://elitemacx86.com/data/avatars/m/0/1.jpg?1683055052)](https://elitemacx86.com/members/elitemacx86.1/)

Joined

Jul 22, 2018

Messages

7,500

Motherboard

Supermicro X11SPA-T

CPU

Intel Xeon W-3275 28 Core

Graphics

2xAMD RX 580 8GB

OS X/macOS

13.x

Bootloader

1.  OpenCore (UEFI)

Mac

1.  Mac mini
2.  MacBook Pro

Mobile Phone

1.  Android
2.  iOS

*   [](https://elitemacx86.com/threads/how-to-boot-macos-installer-on-desktops-and-laptops-using-opencore-uefi-legacy-opencore-install-guide.950/post-7591)
*   [#6](https://elitemacx86.com/threads/how-to-boot-macos-installer-on-desktops-and-laptops-using-opencore-uefi-legacy-opencore-install-guide.950/post-7591)

**Selecting a config.plist File**  

* * *

OpenCore comes with default sample config.plist file. This config.plist cannot be used to boot most of the systems. You'll need to configure the config.plist according to your hardware, mainly the Quirks. Each System has a different set of Quirks which is required for booting. Therefore knowing your hardware is very important. In addition, you may need to configure Graphics and other PCIe devices in order to boot.

We've already created a separate thread on configuring your config.plist to boot the installer. Depending on your hardware, configure the config.plist according to the guide. Depending on the Platform and CPU you have, select the appropriate config.plist building guide.

**Intel Desktops**

* * *

  


|CPU Code Name|Guide Link|
|-------------|----------|
|Penryn       |          |
|Lynfield     |          |
|Sandy Bridge |          |
|Ivy Bridge   |          |
|Haswell      |          |
|Broadwell    |          |
|Skylake      |          |
|Kaby Lake    |          |
|Coffee Lake  |          |
|Comet Lake   |          |
|Rocket Lake  |          |
|Alder Lake   |          |


**NOTE:**  

*   Intel NUC and a few AIO use Mobile CPUs. For all such hardware, it is advised to use the config.plist from `Intel Laptops` section.

  
**Intel Laptops**  

* * *

  


|CPU Code Name|Guide Link|
|-------------|----------|
|Penryn       |          |
|Lynfield     |          |
|Sandy Bridge |          |
|Ivy Bridge   |          |
|Haswell      |          |
|Broadwell    |          |
|Skylake      |          |
|Kaby Lake    |          |
|Coffee Lake  |          |
|Comet Lake   |          |
|Rocket Lake  |          |
|Alder Lake   |          |


  
Intel HEDT


|CPU Code Name|Guide Link|
|-------------|----------|
|Penryn       |          |
|Lynfield     |          |
|Sandy Bridge |          |
|Ivy Bridge   |          |
|Haswell      |          |
|Broadwell    |          |
|Skylake      |          |
|Kaby Lake    |          |
|Coffee Lake  |          |
|Comet Lake   |          |
|Rocket Lake  |          |
|Alder Lake   |          |


  
AMD Desktops

[![EliteMacx86](https://elitemacx86.com/data/avatars/m/0/1.jpg?1683055052)](https://elitemacx86.com/members/elitemacx86.1/)

Joined

Jul 22, 2018

Messages

7,500

Motherboard

Supermicro X11SPA-T

CPU

Intel Xeon W-3275 28 Core

Graphics

2xAMD RX 580 8GB

OS X/macOS

13.x

Bootloader

1.  OpenCore (UEFI)

Mac

1.  Mac mini
2.  MacBook Pro

Mobile Phone

1.  Android
2.  iOS

*   [](https://elitemacx86.com/threads/how-to-boot-macos-installer-on-desktops-and-laptops-using-opencore-uefi-legacy-opencore-install-guide.950/post-7610)
*   [#7](https://elitemacx86.com/threads/how-to-boot-macos-installer-on-desktops-and-laptops-using-opencore-uefi-legacy-opencore-install-guide.950/post-7610)

**Validating config.plist**  

* * *

  
Once you're done editing the config.plist file, the next step is to validate the config.plist to make sure no error exists. To validate your config.plist, follow the steps below.

[![EliteMacx86](https://elitemacx86.com/data/avatars/m/0/1.jpg?1683055052)](https://elitemacx86.com/members/elitemacx86.1/)

Joined

Jul 22, 2018

Messages

7,500

Motherboard

Supermicro X11SPA-T

CPU

Intel Xeon W-3275 28 Core

Graphics

2xAMD RX 580 8GB

OS X/macOS

13.x

Bootloader

1.  OpenCore (UEFI)

Mac

1.  Mac mini
2.  MacBook Pro

Mobile Phone

1.  Android
2.  iOS

*   [](https://elitemacx86.com/threads/how-to-boot-macos-installer-on-desktops-and-laptops-using-opencore-uefi-legacy-opencore-install-guide.950/post-7612)
*   [#8](https://elitemacx86.com/threads/how-to-boot-macos-installer-on-desktops-and-laptops-using-opencore-uefi-legacy-opencore-install-guide.950/post-7612)

**Installing OpenCore to Bootable USB**  

* * *

  
After validating the config.plist, all you need to do is install OpenCore to Bootable USB. To install OpenCore, follow the steps below.

**For macOS Users**

**I. UEFI**

  
If the target system is UEFI capable system, follow the steps below to install OpenCore

If you have used Online Method to create the Bootable USB (see Chapter 2), simply copy the final EFI folder from the working directory to the root of the USB Flash Drive you made earlier.  
[![Screen Shot 2023-06-02 at 6.40.24 PM-min.png](https://elitemacx86.com/data/attachments/6/6033-17dfe7000c66b02efd17181aa8622ad4.jpg "Screen Shot 2023-06-02 at 6.40.24 PM-min.png")](https://elitemacx86.com/attachments/screen-shot-2023-06-02-at-6-40-24-pm-min-png.6025/)  
If you have used Offline Method to create the Bootable USB (see Chapter 2), follow the steps below

Assuming the Bootable USB is still plugged into the computer running macOS, follow the steps below

1\. Mount the EFI Partition of your Bootable USB Flash Drive  
2\. Copy the final EFI folder from the working directory to the EFI Partition of the Bootable USB Flash Drive you made earlier.

[![Screen Shot 2023-06-02 at 6.54.02 PM-min.png](https://elitemacx86.com/data/attachments/6/6032-10dd71eed7e0cc36ba84c49c1e3d1582.jpg "Screen Shot 2023-06-02 at 6.54.02 PM-min.png")](https://elitemacx86.com/attachments/screen-shot-2023-06-02-at-6-54-02-pm-min-png.6024/)

**II. Legacy**  
If the target system is not UEFI capable and only supports Legacy Boot Mode, it will involve a few more steps to install OpenCore.

For either of the method used for creating the Bootable USB, either the Offline or Online Method, you'll need to

For booting OpenCore on Legacy systems which does not support UEFI Boot Mode, a third-party UEFI environment provider is required. OpenDuetPkg is one such UEFI environment provider for legacy systems. To run OpenCore on such a legacy system, OpenDuetPkg can be installed with a dedicated tool BootInstall (bundled with OpenCore). Third-party utilities can be also used to perform the same on systems other than macOS.

For either of the method used for creating the Bootable USB, either the Offline or Online Method, you can use the same method to install the UEFI environment provider.

Within the downloaded OpenCorePkg (RELEASE), navigate to the Utilities/LegacyBoot directory.

[![Screenshot 2023-06-02 at 2.58.23 AM-min.png](https://elitemacx86.com/data/attachments/5/5978-aee784037dd0e4416e2f26089cb16427.jpg "Screenshot 2023-06-02 at 2.58.23 AM-min.png")](https://elitemacx86.com/attachments/screenshot-2023-06-02-at-2-58-23-am-min-png.5970/)

1\. Open the Terminal and execute the following command:

```
#Move to the OpenCore directory
cd Downloads/OpenCore-0.X.X-RELEASE/Utilities/LegacyBoot
```


  
[![Screenshot 2023-06-02 at 4.30.29 AM-min.png](https://elitemacx86.com/data/attachments/5/5979-5fea3a337fb450ffcb2288806da39269.jpg "Screenshot 2023-06-02 at 4.30.29 AM-min.png")](https://elitemacx86.com/attachments/screenshot-2023-06-02-at-4-30-29-am-min-png.5971/)

NOTE: Replace the X with the OpenCore version.

2\. Depending on the CPU type, execute `BootInstallIA32.tool` (for 32-bit) or `BootInstall_X64.tool` (for 64-bit) in Terminal. When prompted, enter your password.

```
#Run BootInstall tool
sudo ./BootInstall X64.tool
```


[![Screenshot 2023-06-02 at 4.31.07 AM-min.png](https://elitemacx86.com/data/attachments/5/5980-9da34aab06b158cc1d28f1358c5d2162.jpg "Screenshot 2023-06-02 at 4.31.07 AM-min.png")](https://elitemacx86.com/attachments/screenshot-2023-06-02-at-4-31-07-am-min-png.5972/)

You'll see something similar to the following as an output of the operation:

[![Screenshot 2023-06-02 at 3.31.20 AM-min.png](https://elitemacx86.com/data/attachments/5/5981-4657df3631344075503c63ba328815d3.jpg "Screenshot 2023-06-02 at 3.31.20 AM-min.png")](https://elitemacx86.com/attachments/screenshot-2023-06-02-at-3-31-20-am-min-png.5973/)  
3\. Determine the disk number of your Bootable USB and type the same and press enter key. In our case, it is disk2.

[![Screenshot 2023-06-02 at 3.31.26 AM-min.png](https://elitemacx86.com/data/attachments/5/5982-1047c5acc10c7a7b485f7dfbc49ca562.jpg "Screenshot 2023-06-02 at 3.31.26 AM-min.png")](https://elitemacx86.com/attachments/screenshot-2023-06-02-at-3-31-26-am-min-png.5974/)  
Now, the BootInstall tool will install the required boot file on the target drive and you'll see the following as an output of the operation.  
 [![Screenshot 2023-06-02 at 3.31.33 AM-min.png](https://elitemacx86.com/data/attachments/5/5983-fdce31d3ffde600f803ded990781bc37.jpg "Screenshot 2023-06-02 at 3.31.33 AM-min.png")](https://elitemacx86.com/attachments/screenshot-2023-06-02-at-3-31-33-am-min-png.5975/)[![Screenshot 2023-06-02 at 3.32.58 AM-min.png](https://elitemacx86.com/data/attachments/5/5984-6081baae3c14b2afe424e6ce2ff5dbae.jpg "Screenshot 2023-06-02 at 3.32.58 AM-min.png")](https://elitemacx86.com/attachments/screenshot-2023-06-02-at-3-32-58-am-min-png.5976/)

In case you're using the BootInstall tool for the Online Method, you'll see the following as an output of the operation.  
 [![Screenshot 2023-06-02 at 4.31.31 AM-min.png](https://elitemacx86.com/data/attachments/5/5985-90f4320a1a964fa43e9b75731c23b7e1.jpg "Screenshot 2023-06-02 at 4.31.31 AM-min.png")](https://elitemacx86.com/attachments/screenshot-2023-06-02-at-4-31-31-am-min-png.5977/)[![Screenshot 2023-06-02 at 4.38.15 AM-min.png](https://elitemacx86.com/data/attachments/5/5986-49a5fb95135825405b68fdf964e9a6f7.jpg "Screenshot 2023-06-02 at 4.38.15 AM-min.png")](https://elitemacx86.com/attachments/screenshot-2023-06-02-at-4-38-15-am-min-png.5978/)  
4\. Once the process is finished, you'll be provided with a boot file in the EFI Partition of your macOS Bootable USB.  
 [![Screenshot 2023-06-02 at 4.02.04 AM-min.png](https://elitemacx86.com/data/attachments/5/5987-61e786d23d9c8c5b70e449947a01467b.jpg "Screenshot 2023-06-02 at 4.02.04 AM-min.png")](https://elitemacx86.com/attachments/screenshot-2023-06-02-at-4-02-04-am-min-png.5979/)[![Screenshot 2023-06-02 at 4.33.05 AM-min.png](https://elitemacx86.com/data/attachments/5/5988-e8c584078d18c90f62bb17c0cfa23566.jpg "Screenshot 2023-06-02 at 4.33.05 AM-min.png")](https://elitemacx86.com/attachments/screenshot-2023-06-02-at-4-33-05-am-min-png.5980/)  
5\. Quit Terminal.

**For Windows Users**  
Copy the final EFI folder from the working directory to the root of the USB Flash Drive you made earlier.

Legacy

1\. Open BOOTICE and select the macOS Bootable USB as a Destination Disk.

2\. Click on Process MBR and then select Restore MBR. When prompted, select the boot0 file from the `Utilities/LegacyBoot` directory found in OpenCorePlg you downloaded earlier (see Chapter 2).

3\. Click on Process PBR and then select Restore PBR. When prompted, select the boot1f32 file from the `Utilities/LegacyBoot` directory found in OpenCorePlg you downloaded earlier (see Chapter 2).

4\. Depending on the target system, copy bootx64 (64-bit CPUs) or bootia32 (32-bit CPUs) file from the `Utilities/LegacyBoot` directory found in OpenCorePlg you downloaded earlier (see Chapter 2) to the root of the USB Flash Drive you made earlier.

5\. Rename bootx64 or bootia32 to boot to ensure it can boot properly.

**For Linux Users**  
Copy the final EFI folder from the working directory to the root of the USB Flash Drive you made earlier.

[![EliteMacx86](https://elitemacx86.com/data/avatars/m/0/1.jpg?1683055052)](https://elitemacx86.com/members/elitemacx86.1/)

Joined

Jul 22, 2018

Messages

7,500

Motherboard

Supermicro X11SPA-T

CPU

Intel Xeon W-3275 28 Core

Graphics

2xAMD RX 580 8GB

OS X/macOS

13.x

Bootloader

1.  OpenCore (UEFI)

Mac

1.  Mac mini
2.  MacBook Pro

Mobile Phone

1.  Android
2.  iOS

*   [](https://elitemacx86.com/threads/how-to-boot-macos-installer-on-desktops-and-laptops-using-opencore-uefi-legacy-opencore-install-guide.950/post-7613)
*   [#9](https://elitemacx86.com/threads/how-to-boot-macos-installer-on-desktops-and-laptops-using-opencore-uefi-legacy-opencore-install-guide.950/post-7613)

**Preparing Computer for booting OS X/macOS**  

* * *

  
Before you rush and try to boot using the USB Installer, there are a few steps that you need to perform. This will eliminate any possible conflicts and make the installation hassle free with great reliability.

**Performing a Backup**  
Although, while performing OS X/macOS installation, unless you delete Disk/Partition by mistake, OS X/macOS does not format the Disk/partition without your consent. Even though, it is recommended to perform a complete backup of your target Disk before proceeding to avoid any possible data loss. This will ensure that your data is safe and you can debug the installation without any hassle. This becomes necessary for Dual Booting Configuration.

**Removing Extra Installed and Connected Devices**  
When booting the OS X/macOS Installer, it is important that you only have the basic Devices connected and any extra installed hardware and connected Devices must be removed or the installer may fail to boot or it can even throw a Kernel Panic. Therefore it is advised to remove the following if it exists

\- Thunderbolt Cards  
\- FireWire Cards  
\- External PCIe and USB Sound Cards  
\- eGPU  
\- SATA/SAS Expanders  
\- SATA/RAID Cards  
\- External PCIe Ethernet Card  
\- Any USB Flash Drive, External Storage Drive, NAS, Mobile Phones, Audio Interface (USB/FireWire/Thunderbolt)

> **NOTE:**  
> 
> *   ****Once the installation is complete, the compatible Devices can be installed back to the computer and can be configured during postinstallation time.****

  
**Setting up Installation Drive**  
In order to have a seamless experience with OS X/macOS, this guide recommends using a separate/spare drive for the installation. This way, you don't need to create a separate partition on your main drive on which you have the primary OS. If you don't have an option to use a separate/spare drive, follow Multiboot Installation Guide.

**Setting Up BIOS/UEFI**  
In order to boot OS X/macOS, you need to set up your BIOS/UEFI. To set up your BIOS/UEFI, refer to the guide linked below

Last edited: Jul 21, 2022

[![EliteMacx86](https://elitemacx86.com/data/avatars/m/0/1.jpg?1683055052)](https://elitemacx86.com/members/elitemacx86.1/)

Joined

Jul 22, 2018

Messages

7,500

Motherboard

Supermicro X11SPA-T

CPU

Intel Xeon W-3275 28 Core

Graphics

2xAMD RX 580 8GB

OS X/macOS

13.x

Bootloader

1.  OpenCore (UEFI)

Mac

1.  Mac mini
2.  MacBook Pro

Mobile Phone

1.  Android
2.  iOS

*   [](https://elitemacx86.com/threads/how-to-boot-macos-installer-on-desktops-and-laptops-using-opencore-uefi-legacy-opencore-install-guide.950/post-7617)
*   [#10](https://elitemacx86.com/threads/how-to-boot-macos-installer-on-desktops-and-laptops-using-opencore-uefi-legacy-opencore-install-guide.950/post-7617)

**OS X/macOS Installation**  

* * *

**STEP 1: Booting the macOS Installer**  
After preparing your installation USB, OpenCore EFI, and setting up your BIOS/UEFI, you're ready to install macOS on the target computer.

1\. Turn on your target Computer.  
2\. Insert the Bootable USB containing macOS Installer and OpenCore EFI. It is always recommended to use the USB 2.0 ports for the installation. If the USB 2.0 ports does not work, try USB 3.0. The ports which will be working without patching (post-install process) are usually hardware dependent and may differ from case to case basis.  
2\. Boot to Boot Menu. The common Boot Menu Keys are Esc (ASUS Laptops), F8 (ASUS Desktops), F9 (HP), F12 (GIGABYTE)

> **NOTE:**  
> 
> *   **Several people use BIOS to override the Boot Device. It is advised to use the Boot Menu Key to enter the Boot Menu**
> *   **Refer to your Computer user manual if necessary**

  
3\. Select your USB Flash Drive with the UEFI Prefix and press enter to boot. For Legacy BIOS, select the USB Flash Drive  
4\. When at OC Boot picker, you'll something similar to this.

 [![2022071806284606-min.png](https://elitemacx86.com/data/attachments/4/4792-4295b5eaaf8de4abc9c20d5afa9e6174.jpg "2022071806284606-min.png")](https://elitemacx86.com/attachments/2022071806284606-min-png.4784/)[![2022071806085381-min.png](https://elitemacx86.com/data/attachments/4/4793-8c4376443369818443bdb6d88f36155b.jpg "2022071806085381-min.png") ](https://elitemacx86.com/attachments/2022071806085381-min-png.4785/)[![2022071804112659-min.png](https://elitemacx86.com/data/attachments/4/4794-f9f7d63e429db24dd06180b7e48f9ab0.jpg "2022071804112659-min.png") ](https://elitemacx86.com/attachments/2022071804112659-min-png.4786/)[![2022071804503395-min.png](https://elitemacx86.com/data/attachments/4/4795-2d682bb2e93eec328aed6c4103a5f26b.jpg "2022071804503395-min.png")](https://elitemacx86.com/attachments/2022071804503395-min-png.4787/)

> **NOTE:**  
> 
> *   **Depending on the type of the Bootable USB you created, you'll see either EFI (External) (dmg) or Install macOS Big Sur (External)**

  
5\. Select Reset NVRAM and press enter.  
6\. Boot back to OC by following steps 2 and 3.  
7\. When at the OC Boot picker, select macOS Base System (External) or Install macOS (External) and press enter to boot and the installer will load in a while. You'll see lots of scrolling texts while the installer loads.

**STEP 2: Installing OS X/macOS**  
1\. When at the installation screen, select your preferred language and continue  
2\. Select Disk Utility and continue, click on View, and select Show all Devices.  
3\. Now select your Hard Drive or SSD on which you want to install macOS and use the following parameters to erase your drive.


|Name  |Macintosh HD      |
|------|------------------|
|Format|APFS              |
|Scheme|GUID Partition Map|


  

> **NOTES:**  
> 
> *   ****The target disk/partition must be GUID****
> *   ****APFS format is recommended for High Sierra and Later****
> *   ****Users who plan to use High Sierra on HDD should use macOS Journaled (HFS+) Format instead of APFS Format.****
> *   ****See Multiboot Guide for more info on installing on an existing Windows Drive****

  
4\. Close Disk Utility  
5\. Select Install macOS and continue with the options.  
6\. Now select Macintosh HD and click on Continue.

> **NOTES:**  
> 
> *   **Step #6 will format the target disk. Proceed with caution.**
> *   **This will take a couple of minutes and will restart at "Less than a minute is remaining". Upon completion, the system will automatically restart. Your Mac will restart to complete the installation.**

  
Here it ends the first phase of the installation.

7\. When your Mac restarts, select Boot macOS Install from Macintosh HD or (Your drive name) and then boot. The installer screen will appear and continue the second phase of the installation. During this phase, the installer will install files to your target disk and create a Recovery HD partition. Upon completion, your Mac will automatically restart.

**STEP 3: Finishing OS X/macOS Setup**  
After finishing the macOS installation, it's time to set up the macOS for the first usage with the newly installed macOS.

When you're at the welcome screen, continue with the basics options such as Keyboard setup, Network, Computer Account, and Privacy settings.

Now the installation is complete! You should be logged in to your Desktop with the beautiful Wallpaper

> **NOTE:**  
> 
> *   **The first boot may be slower as the caches are not built yet. Once the caches are built, it will boot normally.**

  
**_Please read post #2 of this thread for additional information on Post-Installation_**

Last edited: May 28, 2023

### Similar threads

[![EliteMacx86](https://elitemacx86.com/data/avatars/s/0/1.jpg?1683055052)](https://elitemacx86.com/members/elitemacx86.1/)

*   This site uses cookies to help personalise content, tailor your experience and to keep you logged in if you register.  
    By continuing to use this site, you are consenting to our use of cookies.
