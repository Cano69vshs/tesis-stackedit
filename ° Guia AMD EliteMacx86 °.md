< h2 align = " center " > (EliteMacx86) Cómo iniciar el instalador de macOS en computadoras de escritorio y portátiles usando OpenCore [UEFI/Legacy] - Guía de instalación de OpenCore</ h2 >

<p align="center">Esta guía detalla el proceso paso a paso para arrancar el instalador de OS X/macOS en una computadora no Apple utilizando OpenCore como Bootloader. Cubre la instalación y post-instalación, tanto en sistemas UEFI como Legacy. Se incluye soporte para sistemas Intel y AMD, tanto de escritorio como portátiles, con modo de arranque UEFI/Legacy. Se recomienda el arranque UEFI sobre Legacy si es posible, aunque se proporcionan instrucciones para ambos casos.</p>

---

## Table of Contents

1. [Introducción](#chapter-1-introduccion)
2. [Creating macOS](#chapter-2-creating-macos)
3. [Preparing OpenCore EFI](#chapter-3-preparing-opencore-efi)
4. [Gathering Files](#chapter-4-gathering-files)
5. [Setting up config.plist](#chapter-5-setting-up-configplist)
6. [Seleccionar un archivo config.plist](#chapter-6-seleccionar-un-archivo-configplist)
7. [Validar config.plist](#chapter-7-validar-configplist)
8. [Instalación de OpenCore en USB de arranque](#chapter-8-instalacion-de-opencore-en-usb-de-arranque)
9. [Preparación de computadora](#chapter-9-preparacion-de-computadora)
10. [Instalación de macOS](#chapter-10-instalacion-de-macos)

---

## Chapter 1: Introducción
**III. Checking Compatibility**

* * *

  
Once you have gathered the system details, the next step is to perform a compatibility check against the hardware you have. This step is important as it will give you an exact idea of whether your system is compatible or not, what exact hardware is compatible, what hardware needs to be replaced with a compatible one, and whether you can proceed with the purpose of installing macOS or not. To check whether your hardware is compatible or not, simply match your hardware from the compatibility list given below.

If you determine that your hardware is compatible, you can proceed to the next step. If not, you'll have to replace the hardware with a compatible one if you're willing to have a perfect setup with general functionality such as graphics acceleration and network. Make sure you pay attention to the hardware limitations while checking the compatibility. You'll find the limitations for each of the hardware types.

**CPU Compatibility**

  
**GPU Compatibility**  
  
**Storage Compatibility**  
  
**Network Compatibility**  
  
**Thunderbolt Compatibility**  


----



## Chapter 2: Creating macOS


If you determine that your hardware is compatible according to the above-provided compatibility lists, you can start your journey by creating a Bootable USB for your target computer.

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

  
**II. Making the Bootable USB**

A macOS Bootable USB can be made either online or offline. Depending on your choice, select one from below



* Method: Online
  * Notes: Online installers use a recovery image approx 700MB in size and once booted, the macOS is downloaded from Apple servers once installation starts.Can be made in macOS/Windows/LinuxRequires a compatible Ethernet or WiFi setup and stable internet to download the macOS during the installation process
* Method: Offline
  * Notes: Offline installers are full installers.Can be made in macOS only


  
Now, depending on the Operating System you have, choose one of the below options to create the Bootable USB. You can skip this step and head to preparing the [**OpenCore EFI**](https://elitemacx86.com/threads/how-to-boot-macos-installer-on-desktops-and-laptops-using-opencore-uefi-legacy-opencore-install-guide.950/post-7588) if you have already a Bootable USB with macOS Installer you wish to install on the target computer.


  
For users with macOS computers, there are two methods available which are described below. Depending on your preferences, choose one of the methods described below. For users with macOS computers, we recommend Offline Method for creating the Bootable USB.

**I. Offline Method**

**For Windows users**

**STEP 1: Downloading macOS**  
You'll need to have the exact Recovery image of the target OS you want to install. To download the recovery image, follow the steps below.

1\. Install the latest Python from the Microsoft Store.  
2\. Download OpenCore Pkg from the downloads section of this forum.  
3\. Extract the downloaded file to your Desktop.  
4\. Move into the OpenCore-0.X.X-RELEASE/Utilities directory  
5\. Right-click on `macreceovery` folder and select `Copy as path`

[![Screenshot 2022-07-15 104158-min.png](https://elitemacx86.com/data/attachments/4/4720-ea28d44c28329bb037671827935712ff.jpg "Screenshot 2022-07-15 104158-min.png")](https://elitemacx86.com/attachments/screenshot-2022-07-15-104158-min-png.4712/)

6\. Open Command Prompt with Administrator Privileges

[![Screenshot 2022-07-15 104112-min.png](https://elitemacx86.com/data/attachments/4/4721-13d4be9007fd077db1f49d3653a037b5.jpg "Screenshot 2022-07-15 104112-min.png")](https://elitemacx86.com/attachments/screenshot-2022-07-15-104112-min-png.4713/)

[![Screenshot 2022-07-15 104244-min.png](https://elitemacx86.com/data/attachments/4/4722-16c8e7d01d2debd1776d44f09953b08f.jpg "Screenshot 2022-07-15 104244-min.png")](https://elitemacx86.com/attachments/screenshot-2022-07-15-104244-min-png.4714/)

7\. Type cd and then paste the path you copied earlier in step 5 and then press enter key. The command would be the following

```
cd "C:\Users\Your User Name\Desktop\OpenCore-0.X.X-RELEASE\Utiities\macrecovery"
```


  
[![Screenshot 2022-07-15 104630-min.png](https://elitemacx86.com/data/attachments/4/4723-a22e53f564df5cde6a7cb8071e00baec.jpg "Screenshot 2022-07-15 104630-min.png")](https://elitemacx86.com/attachments/screenshot-2022-07-15-104630-min-png.4715/)

**NOTE:**  

*   Replace the X with the OpenCore version.
*   Replace Your User Name with your actual username

  
8\. Depending on the macOS version you need (See Recovery Table below), type the command.

**Recovery Table**



* OS Version: OS X Lion
  * Command: ./macrecovery.py -b Mac-C3EC7CD22292981F -m 00000000000F0HM00 download
* OS Version: OS X Mountain Lion
  * Command: ./macrecovery.py -b Mac-7DF2A3B5E5D671ED -m 00000000000F65100 download
* OS Version: OS X Mavericks
  * Command: ./macrecovery.py -b Mac-F60DEB81FF30ACF6 -m 00000000000FNN100 download
* OS Version: OS X Yosemite
  * Command: ./macrecovery.py -b Mac-E43C1C25D4880AD6 -m 00000000000GDVW00 download
* OS Version: OS X El Capitan
  * Command: ./macrecovery.py -b Mac-FFE5EF870D7BA81A -m 00000000000GQRX00 download
* OS Version: macOS Sierra
  * Command: ./macrecovery.py -b Mac-77F17D7DA9285301 -m 00000000000J0DX00 download
* OS Version: macOS High Sierra
  * Command: ./macrecovery.py -b Mac-7BA5B2D9E42DDD94 -m 00000000000J80300 download
* OS Version: macOS Mojave
  * Command: ./macrecovery.py -b Mac-7BA5B2DFE22DDD8C -m 00000000000KXPG00 download
* OS Version: macOS Catalina
  * Command: ./macrecovery.py -b Mac-CFF7D910A743CAAF -m 00000000000PHCD00 download
* OS Version: macOS Big Sur
  * Command: ./macrecovery.py -b Mac-E43C1C25D4880AD6 -m 00000000000000000 download
* OS Version: Latest Version
  * Command: ./macrecovery.py -b Mac-E43C1C25D4880AD6 -m 00000000000000000 -os latest download


[![Screenshot 2022-07-15 104834-min(1).png](https://elitemacx86.com/data/attachments/4/4724-0d33f18ed5fc555bdc06dc59b32d86b5.jpg "Screenshot 2022-07-15 104834-min(1).png")](https://elitemacx86.com/attachments/screenshot-2022-07-15-104834-min-1-png.4716/)

9\. Once the download is completed, you'll see something like below

[![Screenshot 2022-07-15 105224-min.png](https://elitemacx86.com/data/attachments/4/4725-aaf9070d29ef7120d04ddc6bfac1ace6.jpg "Screenshot 2022-07-15 105224-min.png")](https://elitemacx86.com/attachments/screenshot-2022-07-15-105224-min-png.4717/)

You can find `BaseSystem.dmg` and `BaseSystem.chunklist` in `OpenCore-0.X.X-RELEASE/Utilities/macrecovery` directory.

[![Screenshot 2022-07-15 105246-min.png](https://elitemacx86.com/data/attachments/4/4766-7db34fd843668bebcc2bc2a6c92c5dee.jpg "Screenshot 2022-07-15 105246-min.png")](https://elitemacx86.com/attachments/screenshot-2022-07-15-105246-min-png.4758/)

**NOTE:**  

*   Depending on the macOS version, the script will either download BaseSystem or RecoveryImage files.

  
**STEP 2: Preparing Installer**  
Once you have the Recovery image, you can create the installer. To prepare the installer, follow the steps below

1\. Insert your USB Flash Drive (less than 64GB) into your Windows Computer.  
2\. Download Rufus.  
3\. Open Rufus and under Device select your target USB Flash Drive  
4\. Under Boot selection select `Non Bootable`  
5\. Under Volume label type `EFI`  
6\. Under File System select `Large FAT32 (default)` and click on START.

[![Screenshot 2022-07-16 090447-min.png](https://elitemacx86.com/data/attachments/4/4726-b5e8b6b2d1a246f1e10d4d8e945f8699.jpg "Screenshot 2022-07-16 090447-min.png")](https://elitemacx86.com/attachments/screenshot-2022-07-16-090447-min-png.4718/)

7\. When prompted, click on OK

[![Screenshot 2022-07-16 094256-min.png](https://elitemacx86.com/data/attachments/4/4727-342f4cdcb79c4c9169ef4a184d519d37.jpg "Screenshot 2022-07-16 094256-min.png")](https://elitemacx86.com/attachments/screenshot-2022-07-16-094256-min-png.4719/)

Once erased, you'll see the READY status in Rufus

[![Screenshot 2022-07-16 094323.png](https://elitemacx86.com/data/attachments/4/4728-7948d2cd798bcb607731f64a115f8904.jpg "Screenshot 2022-07-16 094323.png")](https://elitemacx86.com/attachments/screenshot-2022-07-16-094323-png.4720/)

8\. When done, click on Close to close Rufus.  
9\. Now open your USB Flash Drive in Explorer  
10\. Delete `autorun.ico` and `autorun.inf` file from the USB Flash Drive  
11\. Create a folder named `com.apple.recovery.boot` in root of the USB Flash Drive  
12\. Copy `BaseSystem.dmg` and `BaseSystem.chunklist` downloaded above into `com.apple.recovery.boot` directory.


------------------------


**For Linux users**

* * *
# Making the installer in Linux | OpenCore Install Guide
While you don't need a fresh install of macOS to use OpenCore, some users prefer having a fresh slate with their boot manager upgrades.

To start you'll need the following:

*   4GB USB Stick
*   [macrecovery.py (opens new window)](https://github.com/acidanthera/OpenCorePkg/releases)


 




## Chapter 3: Preparing OpenCore EFI

**CHAPTER 3: Preparing OpenCore EFI**  

* * *

  
To prepare the OpenCore EFI, you'll need to download OpenCorePkg. Follow the steps below to prepare OpenCore EFI for your target system.

**Requirements**

*   OpenCore Package
*   OCAuxiliary Tool

  
1\. Download OpenCore Pkg. The OpenCore Pkg comes in two variants. `DEBUG` and `RELEASE`.



* Version: DEBUG
  * Notes: Helps in debugging boot issuesCan add some delay in boot timesRecommended for debugging purposes.
* Version: RELEASE
  * Notes: Not much help for debugging boot issues.Troubleshooting is difficultRecommended for production use.


  
2\. Download the `RELEASE` folder followed by the release version.  
3\. Extract the zip. When extracting, you'll get 4 folders as listed below.



* Directories: Docs
  * Description: Contains documentation, changelog, sample config.plist, and ACPI samples for OpenCore.
* Directories: IA32
  * Description: Contains OpenCore EFI, 32-bit OpenCore Boot Loader.Required for OS X 10.4.1 through 10.4.7
* Directories: Utilities
  * Description: Contains several utilities.
* Directories: X64
  * Description: Contains OpenCore EFI, 64-bit OpenCore Boot Loader.Required for OS X 10.8 and newer


  
1\. Depending on your System Type, copy the EFI folder from `IA32` (For 32-bit CPU) or `X64` (For 64-bit CPU) to your working directory and you should have the following structure.

```
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


  
**Directory Structure**



* Directories and Files: BOOT/Bootx64.efi
  * Notes: Initial bootstrap loaders, which load OpenCore.efi. BOOTx64.efi is loaded by the firmware by default consistent with the UEFI specification. However, it may also be renamed and put in a custom location to allow OpenCore to coexist alongside operating systems, such as Windows, that use BOOTx64.efi files as their loaders.
* Directories and Files: ACPI
  * Notes: Directory for storing ACPI Files (DSDT and SSDTs)Only keep files with the .aml extension
* Directories and Files: Drivers
  * Notes: Directory for storing drivers (UEFI and Legacy)Only keep files with the .efi extension
* Directories and Files: Kexts
  * Notes: Directory for storing Kernel Extensions aka DriversOnly keep files with the .kext extension
* Directories and Files: Resources
  * Notes: Directory for storing media resources such as Audio and GUI, including fonts, icons, and images for Boot Picker for use by OpenCanopy.
* Directories and Files: Tools
  * Notes: Directory for storing toolsOnly keep files with the .efi extension
* Directories and Files: OpenCore.efi
  * Notes: The main booter application responsible for loading the operating system. The directory OpenCore.efi resides in is called the root directory, which is set to EFI\OC by default. When launching OpenCore.efi directly or through a custom launcher, however, other directories containing OpenCore.efi files are also supported.


  
**Cleaning Up**  

* * *

  
By default, OpenCore includes numerous Drivers, Resources, and Tools for several purposes and all of them may not be required on the target System. Therefore, a clean-up is required to ensure there is no clutter which makes troubleshooting difficult, and also to reduce the file size of the EFI. Follow the steps below to perform a cleanup.

**I. Drivers**  
Drivers are essentials that allow several important functions and are required to boot the system, including Recovery mode. By default, OpenCore includes numerous drivers for different purposes. You need to use the drivers required by your system. Depending on the System Type you have, only keep the required drivers as instructed below and delete the rest of the drivers (where applicable) from the `EFI/OC/Drivers` directory. You can find all the unlinked drivers in the `EFI/OC/Drivers` directory.



* Driver Name: AudioDxe.efi
  * Required: Optional
  * Notes: Provides Boot Chime support.Only required if you want to use Boot Chime
* Driver Name: BiosVideo.efi
  * Required: Optional
  * Notes: CSM video driver implementing graphics output protocol based on VESA and legacy BIOS interfaces.Used for UEFI firmware with fragile GOP support (e.g. low resolution).Requires ReconnectGraphicsOnConnect. Included in OpenDuet out of the box.
* Driver Name: CrScreenshotDxe.efi
  * Required: Optional
  * Notes: Screenshot driverRequired for taking screenshots of BIOS, UEFI Shell, UEFI Apps, and UEFI Bootloaders using F10.Saves screenshots to the root of the ESP or any available writeable Filesystem
* Driver Name: Ext4Dxe.efi
  * Required: Optional
  * Notes: Open source EXT4 file system driver.Required for booting with OpenLinuxBoot from the file system most commonly used with Linux.
* Driver Name: HiiDatabase.efi
  * Required: Optional
  * Notes: HII services support driver from MdeModulePkg.This driver is included in most types of firmware starting with the Ivy Bridge generation.Some applications with GUI, such as UEFI Shell, may need this driver to work properly.
* Driver Name: NvmExpressDxe.efi
  * Required: Optional
  * Notes: NVMe support driver from MdeModulePkg.This driver is included in most firmware starting with the Broadwell generation.Used for Haswell and older Systems, where no NVMe driver support is available and NVMe SSD is installed.
* Driver Name: OpenCanopy.efi
  * Required: Optional
  * Notes: Provides GUI functionality for OpenCore Boot screen.This driver is required if you need GUI for OpenCore.See Enabling GUI for more information.
* Driver Name: OpenHfsPlus.efi
  * Required: YES
  * Notes: HFS File System Driver with bless support.Provides reading of HFS+ Volumes/drives in OpenCore Boot Picker.Without HFSPlus.efi driver, you won't be able to see any HFS+ volumes including your macOS USB Installer. Recovery and installation drive for macOS Sierra and prior in OpenCore Boot Picker.This driver is an alternative to a closed source HfsPlus driver commonly found in Apple firmware.
* Driver Name: OpenLinuxBoot.efi
  * Required: Optional
  * Notes: OpenCore plugin implementing OC_BOOT_ENTRY_PROTOCOL to allow direct detection and booting of Linux distributions from OpenCore, without chainloading via GRUB.
* Driver Name: OpenNtfsDxe.efi
  * Required: Optional
  * Notes: New Technologies File System (NTFS) read-only driver.
* Driver Name: OpenPartitionDxe.efi
  * Required: Optional
  * Notes: Partition management driver with Apple Partitioning Scheme support.Required to boot into Recovery on OS X 10.7 to 10.9
* Driver Name: OpenRuntime.efi
  * Required: YES
  * Notes: Runtime driver including several other drivers merged such as ApfsDriverLoader.OpenRuntime.efi is a replacement for AptioMemoryFix.efi driver.Required for booting and proper functioning of OpenCore
* Driver Name: OpenUsbKbDxe.efi
  * Required: Optional
  * Notes: USB keyboard driver adding support for AppleKeyMapAggregator protocols on top of a custom USB keyboard driver implementation. This is an alternative to builtin KeySupport, which may work better or worse depending on the firmware.Required for FileVaultRequired for OpenCore on Legacy Systems. Not recommended on Ivy Bridge and later.
* Driver Name: OpenVariableRuntimeDxe.efi
  * Required: Optional
  * Notes: OpenCore plugin for emulated NVRAM support.Required for Legacy systems where native NVRAM support is absent or incompatible.OpenDuet already includes this driverMust load using LoadEarly=True in UEFI>Drivers section of the config.plist.See Fixing NVRAM for more information.
* Driver Name: Ps2KeyboardDxe.efi
  * Required: Optional
  * Notes: PS/2 keyboard driver from MdeModulePkg.Required for PS2 KeyboardRequires KeySupport to be enabled.
* Driver Name: Ps2MouseDxe.efi
  * Required: Optional
  * Notes: PS/2 mouse driver from MdeModulePkg.Required for PS/2 MouseSome very old laptop firmware may not include this driver but it is necessary for the touchpad to work in UEFI graphical interfaces such as OpenCanopy.
* Driver Name: ResetNvramEntry.efi
  * Required: YES
  * Notes: Provides Reset NVRAM support.Required since OpenCore 0.8.1 and laterSee Resetting NVRAM for more information.
* Driver Name: ToggleSipEntry.efi
  * Required: Optional
  * Notes: Provides a boot entry for enabling and disabling System Integrity Protection (SIP) in OpenCore picker.
* Driver Name: UsbMouseDxe.efi
  * Required: Optional
  * Notes: Similar to OpenUSBKbDxeRequired for Legacy Systems using DuetPkg
* Driver Name: XhciDxe.efi
  * Required: Optional
  * Notes: XHCI USB controller support driver from MdeModulePkg.This driver is included in most types of firmware starting with the Sandy Bridge generation.Used for Sandy Bridge and older Systems, where no XHCI driver support is available and an external USB 3.0 PCI Card is installed.


  
**Summary**  

* * *

  
If you're still confused regarding which drivers to use, we've summarized it to understand it better for your convenience. Simply choose the combination of the drivers based on your System Type. Please note that this combination is sufficient in most cases, however, depending on your system, the driver's requirement may vary and your system may require some other drivers.

#### I. Drivers for UEFI-based Systems​

*   OpenRuntime.efi
*   OpenHfsPlus.efi
*   OpenCanopy.efi
*   AudioDxe.efi
*   ResetNvramEntry.efi

#### II. Drivers for Legacy based Systems​

*   OpenRuntime.efi
*   HfsPlusLegacy.efi
*   OpenUsbKbDxe.efi
*   UsbMouseDxe.efi
*   OpenCanopy.efi
*   AudioDxe.efi
*   ResetNvramEntry.efi

**NOTES:**  

*   **OpenCore and Drivers should be from the same RELEASE version and should not mismatch.**
*   **UEFI Drivers from Clover are not supported with OpenCore. See Switching Clover to OpenCore if you're using Clover and want to switch to OpenCore.**

  
The resulting `Drivers` directory should look something like this:

picture here

**II. Tools**  
Tools are of great use but are for specific purposes only and most of them are optional. Although, it will not harm even if you keep these tools. However, to reduce the size and have less clutter, it is advised to delete the tools which you don't need. For debugging and later use, it is recommended to keep these tools. You can either keep them or delete them as per your personal preferences. These standalone tools help to debug the firmware and hardware.



* Tool Name: BootKicker.efi
  * Required: Optional
  * Notes: Enter Apple BootPicker menu (exclusive for Macs with compatible GPUs).
* Tool Name: ChipTune.efi
  * Required: Optional
  * Notes: Test BeepGen protocol and generate audio signals of different styles and lengths.
* Tool Name: CleanNvram.efi
  * Required: Optional
  * Notes: Reset NVRAM alternative bundled as a standalone tool.
* Tool Name: ControlMsrE2.efi
  * Required: Optional
  * Notes: Check CFG Lock (MSR 0xE2 write protection) consistency across all cores and change such hidden options on selected platforms.
* Tool Name: CsrUtil.efi
  * Required: Optional
  * Notes: Simple implementation of SIP-related features of Apple csrutil.
* Tool Name: GopStop.efi
  * Required: Optional
  * Notes: Test GraphicsOutput protocol with a simple scenario.
* Tool Name: KeyTester.efi
  * Required: Optional
  * Notes: Test keyboard input in SimpleText mode.
* Tool Name: MmapDump.efi
  * Required: Optional
  * Notes: 
* Tool Name: OpenControl.efi
  * Required: Optional
  * Notes: Unlock and lock back NVRAM protection for other tools to be able to get full NVRAM access when launching from OpenCore.
* Tool Name: OpenShell.efi
  * Required: Optional
  * Notes: Used for debuggingRequired for mapping entries for Dual Boot
* Tool Name: ResetSystem.efi
  * Required: Optional
  * Notes: Utility to perform system reset. Takes reset type as an argument: cold reset, firmware, shutdown, warm reset. Defaults to cold reset.
* Tool Name: RtcRw.efi
  * Required: Optional
  * Notes: Utility to read and write RTC (CMOS) memory.
* Tool Name: TpmInfo.efi
  * Required: Optional
  * Notes: Check Intel PTT (Platform Trust Technology) capability on the platform, which allows using fTPM 2.0 if enabled.The tool does not check whether fTPM 2.0 is actually enabled.


  
A cleaned-up EFI should be like the following:

#### For UEFI based Systems​

```
EFI
├── BOOT
│   └── BOOTx64.efi
└── OC
    ├── ACPI
    ├── Drivers
    │   ├── OpenRuntime.efi
    │   ├── OpenHfsPlus.efi
    │   ├── OpenCanopy.efi
    │   ├── AudioDxe.efi
    │   └── ResetNvramEntry.efi
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


#### ​

#### For Legacy based Systems​

```
EFI
├── BOOT
│   └── BOOTx64.efi
└── OC
    ├── ACPI
    ├── Drivers
    │   ├── OpenRuntime.efi
    │   ├── HfsPlusLegacy.efi
    │   ├── OpenUsbKbDxe.efi
    │   ├── UsbMouseDxe.efi
    │   ├── OpenCanopy.efi
    │   ├── AudioDxe.efi
    │   └── ResetNvramEntry.efi
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



## Chapter 4: Gathering Files

Here's what your EFI somewhat look like:

Now that you have obtained the OpenCore Base EFI and gathered SSDTs (.aml), Drivers (.efi) and Kexts (.kext), the resulting EFI folder in your working directory should look something like this:





## Chapter 5: Setting up config.plist


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


--------

## Chapter 6: Seleccionar un archivo config.plist

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

* * *


## Chapter 7: Validar config.plist

After validating the config.plist, all you need to do is install OpenCore to Bootable USB. To install OpenCore, follow the steps below.

**For Linux Users**  
Copy the final EFI folder from the working directory to the root of the USB Flash Drive you made earlier.






## Chapter 8: Instalación de OpenCore en USB de arranque
**Installing OpenCore to Bootable USB**  

* * *

  
After validating the config.plist, all you need to do is install OpenCore to Bootable USB. To install OpenCore, follow the steps below.

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





## Chapter 9: Preparación de computadora

**Preparing Computer for booting OS X/macOS**  

  
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




## Chapter 10: Instalación de macOS

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


