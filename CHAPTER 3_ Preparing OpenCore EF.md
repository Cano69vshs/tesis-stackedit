


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


  
The resulting cleaned up EFI should look something like this:

UEFI picture

