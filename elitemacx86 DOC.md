
# GUIDE - How to Boot macOS Installer on Desktops and Laptops using OpenCore [UEFI/Legacy] - OpenCore Install Guide | EliteMacx86 Forum
*   Become a [Premium Member](https://elitemacx86.com/account/upgrades/) for $25/year with no ads to improve your community experience.
    

You are using an out of date browser. It may not display this or other websites correctly.  
You should upgrade or use an [alternative browser](https://www.google.com/chrome/).

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

*   [](https://elitemacx86.com/threads/how-to-boot-macos-installer-on-desktops-and-laptops-using-opencore-uefi-legacy-opencore-install-guide.950/post-7519)
*   [#1](https://elitemacx86.com/threads/how-to-boot-macos-installer-on-desktops-and-laptops-using-opencore-uefi-legacy-opencore-install-guide.950/post-7519)

### How to Boot macOS Installer on Desktops and Laptops using OpenCore \[UEFI/Legacy\] - OpenCore Install Guide​

  
Booting the OS X/macOS installers on a non-Apple computer can be challenging for new users. This guide is intended for those who wish to use OpenCore as a Bootloader and it covers a step-by-step process to boot the OS X/macOS installer on your target Desktop or Laptop using the OpenCore bootloader along with the installation and post-installation. Both installing using OpenCore UEFI and OpenCore Legacy are described in this guide.

By following this guide, you'll also be able to create a complete OpenCore EFI for your particular system. This guide supports both Intel and AMD Desktops and Intel and AMD Laptops with UEFI/Legacy boot mode. Of course, the hardware compatibility must be taken care of. Those who are still using Clover as a primary bootloader, can either [**switch to OpenCore**](https://elitemacx86.com/threads/how-to-switch-from-clover-to-opencore-uefi-legacy-clover-conversion-guide.1053/) or can follow [**Clover Installation Guide**](https://elitemacx86.com/threads/how-to-boot-macos-installer-on-desktops-and-laptops-using-clover-uefi-legacy-clover-install-guide.954/).

Although UEFI Capable Systems have several advantages over Legacy, there can be systems that do not support UEFI booting and are only capable of Legacy booting. But if you do have a system that supports UEFI booting, it is recommended to use UEFI booting over the legacy boot.

*   If you're having a computer that is UEFI capable, follow the UEFI instructions.
*   If you're having a system that doesn't have UEFI capabilities, then follow the Legacy instructions.

  
What is OpenCore?  

* * *

  
OpenCore is a bootloader - Unlike any other bootloader such as GRUB, it is an advanced bootloader especially designed to boot macOS/OS X on Non-Apple computers and is capable of booting a variety of other Operating Systems including Windows and Linux. OpenCore differs a lot from Clover and has been designed with security and quality, allowing us to use many security features found on real Macs such as System Integrity Protection and FileVault. Moreover, configuring an OpenCore EFI (used for booting) is way less complex than Clover and provides much more modern functionality than Clover. Although, still lacks some of the great features which are implemented in Clover such as on-the-fly hot patching. However, there are more advantages to using OpenCore due to its easy-to-configure in nature and regular updates. More in-depth information can be found in [**Why you should use OpenCore over Clover and other Bootloaders**](https://elitemacx86.com/threads/why-you-should-use-opencore-over-clover-and-other-bootloaders.1229/).

OpenCore is almost always changing to support recent versions of macOS and newer computers. It is complex and more difficult to set up OpenCore when you are not familiar with all the components and a variety of options that can be used to configure OpenCore for booting into macOS.

The purpose of this guide is to show how to create a macOS Bootable USB and create OpenCore EFI which can be used to install macOS/OS X on a target computer. Where creating EFI is the main essence of the guide as that's what most people are looking for. It is strongly advised to create a configuration (OpenCore EFI) from scratch without the involvement of someone's else configuration and files and this is where this guide comes into place.

Using OpenCore EFI from another system or picking from the Internet (mostly from Github or other forums) is relatively easy than creating yours, but will not result in many benefits due to the difference in the hardware and the vendor. Although it may be capable of booting macOS/OS X on a target system, these pre-made EFIs not only come with a lot of unnecessary SSDTs, Kexts, and Quirks but also includes custom branding and are usually way lot cluttered than the original method and are generally not reliable (missing hardware functionality and/or features) which is not the preferred choice. Often, it makes it difficult to inject patches, Device Properties, and Quirks due to being prevented from being injected which is why most of the guides generally don't work with such EFIs. Everything is injected forcibly to ensure the macOS/OS X installer boots anyhow on the target system, which still fails in several cases.

There could be known performance-related issues i.e. getting less performance than the system is actually capable of or it may not perform well on your system in general (even if it is working for the primary user). In addition, despite having the same hardware configuration, there are chances that your system may require some additional configuration than the EFI you're using to boot. Most of the users just want to boot the macOS installer on their systems, without getting to know the basics involved which is the key and this is why it makes it more difficult to troubleshoot if such configuration fails to boot the macOS/OS X installer on the target system.

Just to avoid reading and investing time into building a proper EFI, several users use the EFI of someone else. This is a very common practice often followed by new users building their OpenCore EFI and this is why such users run into different issues and invest their time effortlessly to fix the junk. Rather than investing time in troubleshooting the installation and fixing someone's else EFI configuration, which is not even intended for your particular system, it would make more sense to create your own OpenCore EFI and move in the right direction in the first place. Using someone's EFI not just makes it difficult to boot the macOS/OS X installer, but it invites way more issues than it could have originally.

Due to all these reasons, using OpenCore EFI from some other computer or user is never advised and such practice is highly discouraged, especially on this forum. If you don't follow the guide carefully, after a point of time, you will end up frustrated if you're lacking time and patience. Of course, it's your computer and you have the right to decide whether to install macOS for your use case or not.

Please do not expect to have everything working (in terms of hardware, feature, or functionality) once you're booted into macOS/OS X. This guide only aims to install macOS/OS X on a target computer and the rest of the steps are covered in the Postinstallation guide.

For users who are not familiar with OpenCore or if they haven't used it before, this guide may seem a bit complex to them, but it is quite simple if you read and go through the steps carefully. Those users who are familiar with OpenCore or have used it before will find this guide relatively easy to follow than any other guide!

**I. Requirements**

* * *

  
Before you jump into making an OpenCore based EFI, you need to take care of the following.

**Hardware Information**  
You should have at least the following information to check the compatibility of your system.

*   Motherboard Chipset Series
*   CPU Model and Generation
*   GPU Model
*   Storage Devices and their Configuration (AHCI/RAID/IDE)
*   Laptop/Desktop model if it is branded/OEM
*   Ethernet chip Model
*   WiFi/BT Model

**A Compatible Hardware**  
This guide requires compatible hardware to work. Even if you manage to install OS X/macOS, some features either may work partially or may not work at all. See the compatibility section for more information.

**Storage Devices and Space**

*   A Compatible HDD or SSD (NVMe/PCIe/SATA SSD) for installation. If you're using High Sierra and later, an SSD is recommended for optimal performance.
*   At least 16GB of USB if you're going to use macOS to create the Bootable USB using the Offline Method.
*   At least 2GB of USB if you're going to use macOS, Windows, or Linux to create the Bootable USB using the Online Method.
*   At least 30GB of space for macOS/OS X installation on the target Drive.

**Internet Connection**  
An internet connection is required to download the files. If using the Online Method for Installation, you'll also require a compatible working Ethernet or WiFi connection with a proper speed. Ethernet is generally preferred over WiFi or any other network setup to avoid hassles. However, if you do not wish to use Ethernet or don't have a proper setup yet, you can also use a Compatible WiFi Card to achieve the same. Please be advised that a majority of WiFi Cards are not supported under macOS. USB based WiFi Dongles and USB to LAN Adapters may not work in this case. iPhone/iPad supporting Mobile Data can be used but requires proper USB mapping (since Big Sur 11.3 and later). If you do not have a compatible Ethernet, WiFi Card, or iPhone/iPad, you can use HoRNDIS to tether your Android's Mobile Data. In addition, please be advised that carrier charges may apply if using Mobile Data for the internet.

**A Computer**  
To create the Bootable USB and the OpenCore EFI, you'll need a computer with either macOS (preferred) or Windows (Windows 10 or newer) or any Linux distribution (Ubuntu preferred), and the OS must be functioning properly. In addition, you'll need at least 30GB (macOS) and 15GB (Windows or Linux) of disk space to download the required files.

**BIOS**  
Although always recommended, you must flash the latest available BIOS/UEFI on the target system, if not already. However, there could be exceptional cases where the new/latest BIOS has bugs which can prevent from booting the macOS/OS X Installer. In such cases, you'll need to roll back the BIOS until you find the stable version.

**Time and Patience**  
To create a clutter-free, highly reliable OpenCore EFI especially curated for your particular system, you'll get to know the basics of the OpenCore EFI and how to build it for a target system, you should be prepared to read, learn, and even search for the specific issue on the internet if you're encountering one. This is not a simple one-click setup or the copy/paste method. Having said that, please be advised that it would require a tremendous amount of patience and time to get the expected results.

**Experience**  
Although this guide includes every crucial step, including the basic ones, having experience with the basic usage of tools such as Command Prompt, Terminal or File Explorer can be really helpful when following the guide.

**II. Gathering System Info**

* * *

  
Before you start, get to know about your hardware. This is mainly required for OEM machines like Laptops and pre-built systems. Starting your journey without knowing your hardware will make your journey difficult with no chance of success. Also when seeking support on this forum, the hardware details should be present in your profile. Before asking for support, make sure your profile includes the hardware details.

To find out the complete details of your system, refer to the following guide linked below for more information.

  
You can skip this section and head to [**Creating macOS Bootable USB**](https://elitemacx86.com/threads/how-to-boot-macos-installer-on-desktops-and-laptops-using-opencore-uefi-legacy-opencore-install-guide.950/post-7592) if you're already aware of the system specifications.

**III. Checking Compatibility**

* * *

  
Once you have gathered the system details, the next step is to perform a compatibility check against the hardware you have. This step is important as it will give you an exact idea of whether your system is compatible or not, what exact hardware is compatible, what hardware needs to be replaced with a compatible one, and whether you can proceed with the purpose of installing macOS or not. To check whether your hardware is compatible or not, simply match your hardware from the compatibility list given below.

If you determine that your hardware is compatible, you can proceed to the next step. If not, you'll have to replace the hardware with a compatible one if you're willing to have a perfect setup with general functionality such as graphics acceleration and network. Make sure you pay attention to the hardware limitations while checking the compatibility. You'll find the limitations for each of the hardware types.

**CPU Compatibility**

  
**GPU Compatibility**  
  
**Storage Compatibility**  
  
**Network Compatibility**  
  
**Thunderbolt Compatibility**  

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

*   [](https://elitemacx86.com/threads/how-to-boot-macos-installer-on-desktops-and-laptops-using-opencore-uefi-legacy-opencore-install-guide.950/post-7592)
*   [#2](https://elitemacx86.com/threads/how-to-boot-macos-installer-on-desktops-and-laptops-using-opencore-uefi-legacy-opencore-install-guide.950/post-7592)

**CHAPTER 2: Creating macOS/OS X Bootable USB**

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

**For macOS users**

* * *

  
For users with macOS computers, there are two methods available which are described below. Depending on your preferences, choose one of the methods described below. For users with macOS computers, we recommend Offline Method for creating the Bootable USB.

**I. Offline Method**

* * *

  
**1\. Downloading macOS (10.13 and newer)

(a). Method #1: Using App Store

**

If you're running OS X El Capitan 10.11.6 or macOS Sierra 10.12.5 or later, you can use App Store Method to download the required version of macOS. However, this method is limited and will only allow you to download macOS High Sierra 10.13 and newer. For macOS Sierra (10.12) and older, see Downloading Legacy OS. Follow the steps below to download macOS High Sierra and newer using Mac App Store.

1\. Depending on the macOS version you want to install, open the appropriate link given below using Safari. You can use other browsers as well (such as Chrome or Firefox). When using other browsers than Safari, click on Open App Store when prompted. This will open the App Store download page for the particular macOS. For the active macOS release versions, you can also directly go to App Store and download the desired macOS release and continue Preparing the installer.

```
macOS Sonoma (14.x)
macappstores://apps.apple.com/app/macos-sonoma/id6450717509?mt=12

macOS Ventura (13.x)
macappstores://apps.apple.com/app/macos-ventura/id1638787999?mt=12

macOS Monterey (12.x)
macappstores://apps.apple.com/us/app/macos-monterey/id1576738294?mt=12

macOS Big Sur (11.x)
macappstores://apps.apple.com/us/app/macos-big-sur/id1526878132?mt=12

macOS Catalina (10.15)
macappstores://apps.apple.com/us/app/macos-catalina/id1466841314?mt=12

macOS Mojave (10.14)
macappstores://apps.apple.com/us/app/macos-mojave/id1398502828?mt=12

macOS High Sierra (10.13)
macappstores://apps.apple.com/us/app/macos-high-sierra/id1246284741?mt=12
```


[![Screenshot 2023-06-02 at 9.51.37 AM-min.png](https://elitemacx86.com/data/attachments/5/5989-c33bdd1bd479d4862a71fb8ec54b5794.jpg "Screenshot 2023-06-02 at 9.51.37 AM-min.png")](https://elitemacx86.com/attachments/screenshot-2023-06-02-at-9-51-37-am-min-png.5981/)  
2\. When the App Store opens, click on the `GET` button to begin downloading the macOS installer.

[![Screenshot 2023-06-02 at 10.28.34 AM-min.png](https://elitemacx86.com/data/attachments/5/5990-d9d3f81407fad5e3b1be57cc5e0474af.jpg "Screenshot 2023-06-02 at 10.28.34 AM-min.png")](https://elitemacx86.com/attachments/screenshot-2023-06-02-at-10-28-34-am-min-png.5982/)  
If you're downloading the macOS on a system running macOS Monterey or later, you'll see something like below. Click on the Download button to download the macOS.  
[![Screenshot 2023-06-02 at 10.34.23 AM.png](https://elitemacx86.com/data/attachments/5/5997-8a830ace21c4d16ad70702384b127fa6.jpg "Screenshot 2023-06-02 at 10.34.23 AM.png")](https://elitemacx86.com/attachments/screenshot-2023-06-02-at-10-34-23-am-png.5989/)

Once you click on the Download button, the system will start downloading the required version of macOS.

[![Screenshot 2023-06-02 at 11.55.54 AM-min.png](https://elitemacx86.com/data/attachments/5/5999-0a0a0af34e6340cc70b1cc5989b1139d.jpg "Screenshot 2023-06-02 at 11.55.54 AM-min.png")](https://elitemacx86.com/attachments/screenshot-2023-06-02-at-11-55-54-am-min-png.5991/)

3\. Once the download is complete, the macOS installer will appear in the Applications folder and will open automatically.  
 [![Screenshot 2023-06-02 at 12.02.23 PM-min.png](https://elitemacx86.com/data/attachments/6/6000-30b21d5268994d61775577748904c04e.jpg "Screenshot 2023-06-02 at 12.02.23 PM-min.png")](https://elitemacx86.com/attachments/screenshot-2023-06-02-at-12-02-23-pm-min-png.5992/)[![Screenshot 2023-06-02 at 11.56.38 AM-min.png](https://elitemacx86.com/data/attachments/6/6001-cd92ce1cd1035efd17ab2fbdf308d447.jpg "Screenshot 2023-06-02 at 11.56.38 AM-min.png")](https://elitemacx86.com/attachments/screenshot-2023-06-02-at-11-56-38-am-min-png.5993/)

4\. Quit the installer using Command+Q. When prompted, click on Quit.  
[![Screenshot 2023-06-02 at 11.56.51 AM-min.png](https://elitemacx86.com/data/attachments/6/6002-8a99a8bff5763f2449145ecbe60f5876.jpg "Screenshot 2023-06-02 at 11.56.51 AM-min.png")](https://elitemacx86.com/attachments/screenshot-2023-06-02-at-11-56-51-am-min-png.5994/)  
For old versions of macOS (such as High Sierra), you may see a warning once the system finishes downloading macOS. Simply click on Quit as we'll have to create a Bootable USB instead of installing it on the same computer.

[![Screenshot 2023-06-02 at 12.11.07 PM-min.png](https://elitemacx86.com/data/attachments/6/6003-91e86aacc93f2b45b2e9520a869632f8.jpg "Screenshot 2023-06-02 at 12.11.07 PM-min.png")](https://elitemacx86.com/attachments/screenshot-2023-06-02-at-12-11-07-pm-min-png.5995/)  
**(b). Method #2: Using Command Line Software Update Utility**

If you're running an old macOS/OS X version or cannot use App Store or cannot download from App Store, you can use this method to download a desired copy of macOS. However, unlike the App Store method, this method is also limited and will allow you to download macOS High Sierra (10.13) and newer. For macOS Sierra (10.12) and older, see Downloading Legacy OS. Follow the steps below to download macOS High Sierra and newer using Command Line Software Update Utility.

1\. Open the Terminal and execute the following command. When prompted for a password, enter your password.

```
softwareupdate --list-full-installers;echo;echo "Please enter the version number you wish to download:";read;$(if [ -n "$REPLY" ]; then; echo "softwareupdate --fetch-full-installer --full-installer-version "$REPLY; fi);
```


[![Screenshot 2023-06-02 at 6.40.29 AM.png](https://elitemacx86.com/data/attachments/6/6004-3fa4a307db3bc77f1edb9317f6aaef75.jpg "Screenshot 2023-06-02 at 6.40.29 AM.png")](https://elitemacx86.com/attachments/screenshot-2023-06-02-at-6-40-29-am-png.5996/)  
You'll get a list of available products for download:  
[![Screenshot 2023-06-02 at 6.43.10 AM.png](https://elitemacx86.com/data/attachments/6/6005-c9ea62981a20a4be0a7724dcb1f9b8d8.jpg "Screenshot 2023-06-02 at 6.43.10 AM.png")](https://elitemacx86.com/attachments/screenshot-2023-06-02-at-6-43-10-am-png.5997/)  
3\. Depending on the macOS version you want to install, type the version number right to the macOS release name and press enter key. The Terminal will start downloading the files.  
[![Screenshot 2023-06-02 at 6.48.09 AM.png](https://elitemacx86.com/data/attachments/6/6006-4f6010a380cbe2fe223d3c7eebab3bde.jpg "Screenshot 2023-06-02 at 6.48.09 AM.png")](https://elitemacx86.com/attachments/screenshot-2023-06-02-at-6-48-09-am-png.5998/)  
Once the download is completed, you'll see the following screen:  
[![Screenshot 2023-06-02 at 6.53.33 AM.png](https://elitemacx86.com/data/attachments/6/6007-af46ee55f20f8b5f3a4681d798e375f0.jpg "Screenshot 2023-06-02 at 6.53.33 AM.png")](https://elitemacx86.com/attachments/screenshot-2023-06-02-at-6-53-33-am-png.5999/)  
4\. Quit Terminal. You'll find the macOS installer in .app format under the Applications folder.  
[![Screenshot 2023-06-02 at 12.02.23 PM-min.png](https://elitemacx86.com/data/attachments/6/6000-30b21d5268994d61775577748904c04e.jpg "Screenshot 2023-06-02 at 12.02.23 PM-min.png")](https://elitemacx86.com/attachments/screenshot-2023-06-02-at-12-02-23-pm-min-png.5992/)  
5\. Verify the size of the downloaded installer by getting the info of the Installer. Usually, this should be near about 12GiB approx in size. This is required as sometimes the installer gets downloaded as an incomplete installer which can prevent booting the macOS/OS X installer.  
[![Screenshot 2023-06-02 at 12.25.29 PM-min.png](https://elitemacx86.com/data/attachments/6/6008-cd6c55f1eb2bd529906cd5c2f08977db.jpg "Screenshot 2023-06-02 at 12.25.29 PM-min.png")](https://elitemacx86.com/attachments/screenshot-2023-06-02-at-12-25-29-pm-min-png.6000/)  
**(c). Method #3: Using OCLP**

If you're running an old macOS/OS X version or cannot use App Store or cannot download from App Store, you can use this method to download a desired copy of macOS. However, unlike the App Store, Command Line method, and macadmin scripts, this method is also limited and will allow you to download macOS High Sierra (10.13) and newer. For macOS Sierra (10.12) and older, see Downloading Legacy OS. Follow the steps below to download macOS High Sierra and newer using OCLP.

1\. Download OpenCore Legacy Patcher, if you haven't already  
2\. Open OCLP and you'll see something similar to the screenshot attached below.  
[![Screen Shot 2023-09-26 at 10.11.14 PM-min.png](https://elitemacx86.com/data/attachments/6/6526-fe52e7552027d138d6b33cabdcdb64c5.jpg "Screen Shot 2023-09-26 at 10.11.14 PM-min.png")](https://elitemacx86.com/attachments/screen-shot-2023-09-26-at-10-11-14-pm-min-png.6518/)  
3\. Click on Create macOS Installer. When prompted, click on Download macOS Installer  
[![Screen Shot 2023-09-26 at 10.11.18 PM-min.png](https://elitemacx86.com/data/attachments/6/6527-fb2ddd759be173f639c7860806479481.jpg "Screen Shot 2023-09-26 at 10.11.18 PM-min.png")](https://elitemacx86.com/attachments/screen-shot-2023-09-26-at-10-11-18-pm-min-png.6519/)  
You'll see a list of recent macOS installers. For a complete list, click on Show all available installers and you'll get a complete list of the available macOS installers.  
 [![Screen Shot 2023-09-26 at 10.12.30 PM-min.png](https://elitemacx86.com/data/attachments/6/6528-eab2a774a8d7a1e782778c6a2e2f5c46.jpg "Screen Shot 2023-09-26 at 10.12.30 PM-min.png")](https://elitemacx86.com/attachments/screen-shot-2023-09-26-at-10-12-30-pm-min-png.6520/)[![Screen Shot 2023-09-26 at 10.12.44 PM-min.png](https://elitemacx86.com/data/attachments/6/6529-1cbf05cdf4208ecd479ef3c44720485c.jpg "Screen Shot 2023-09-26 at 10.12.44 PM-min.png")](https://elitemacx86.com/attachments/screen-shot-2023-09-26-at-10-12-44-pm-min-png.6521/)  
4\. Depending on the macOS version you want to install, simply click on that version and the OCLP will start downloading the files. You can also get the installer size right next to the installer's name.  
[![Screen Shot 2023-09-26 at 10.15.04 PM-min.png](https://elitemacx86.com/data/attachments/6/6531-5eb251a9a376c14230d0963698497d0f.jpg "Screen Shot 2023-09-26 at 10.15.04 PM-min.png")](https://elitemacx86.com/attachments/screen-shot-2023-09-26-at-10-15-04-pm-min-png.6523/)  
Once the download is completed, OCLP will verify the macOS installer.  
[![Screen Shot 2023-09-26 at 10.17.25 PM-min.png](https://elitemacx86.com/data/attachments/6/6532-821938d7e375622f16ea12ac38a8e6b2.jpg "Screen Shot 2023-09-26 at 10.17.25 PM-min.png")](https://elitemacx86.com/attachments/screen-shot-2023-09-26-at-10-17-25-pm-min-png.6524/)  
5\. Once OCLP completes validating, it will prompt for a password to extract the installer into the `/Applications` directory. When prompted, enter your password and click on OK to continue with the extraction.

 [![Screen Shot 2023-09-26 at 10.18.13 PM-min.png](https://elitemacx86.com/data/attachments/6/6534-7a3a1099571755237b629b65a6235154.jpg "Screen Shot 2023-09-26 at 10.18.13 PM-min.png")](https://elitemacx86.com/attachments/screen-shot-2023-09-26-at-10-18-13-pm-min-png.6526/)[![Screen Shot 2023-09-26 at 10.18.05 PM-min.png](https://elitemacx86.com/data/attachments/6/6533-2a5344856d7dff3dcd8bcbf58c5b7991.jpg "Screen Shot 2023-09-26 at 10.18.05 PM-min.png")](https://elitemacx86.com/attachments/screen-shot-2023-09-26-at-10-18-05-pm-min-png.6525/)  
Once OCLP finishes extracting the installer, you'll see a success message. OCLP will also prompt to create the bootable USB.  
 [![Screen Shot 2023-09-26 at 10.18.44 PM-min.png](https://elitemacx86.com/data/attachments/6/6535-65ea9a62144198352cdf9fe7c9d197b2.jpg "Screen Shot 2023-09-26 at 10.18.44 PM-min.png")](https://elitemacx86.com/attachments/screen-shot-2023-09-26-at-10-18-44-pm-min-png.6527/)[![Screen Shot 2023-09-26 at 10.18.48 PM-min.png](https://elitemacx86.com/data/attachments/6/6536-686e21b597dce91bcb1a2cc0ec3ecad3.jpg "Screen Shot 2023-09-26 at 10.18.48 PM-min.png")](https://elitemacx86.com/attachments/screen-shot-2023-09-26-at-10-18-48-pm-min-png.6528/)  
6\. Click on No and then click on Return to Main Menu.  
7\. Quit OCLP.

You can find the respective macOS Installer in the `/Applications` directory.

**(d). Method #4: Using macadmin scripts**

If you're running an old macOS/OS X version or cannot use App Store or cannot download from App Store, you can use this method to download a desired copy of macOS. However, unlike the App Store and Command Line method, this method is also limited and will allow you to download macOS High Sierra (10.13) and newer. For macOS Sierra (10.12) and older, see Downloading Legacy OS. Follow the steps below to download macOS High Sierra and newer using macadmin scripts.

1\. Install Command Line Tools, if you haven't already.  
2\. Open the Terminal and execute the following commands.

```
#Clone macadmin scripts
git clone https://github.com/munki/macadmin-scripts

#Move to the directory
cd macadmin-scripts

#Run installinstallmacos.py
sudo ./installinstallmacos.py
```


  
[![Screen Shot 2023-06-02 at 3.59.05 PM-min.png](https://elitemacx86.com/data/attachments/6/6016-4487c8db441be448f28e161997109e4e.jpg "Screen Shot 2023-06-02 at 3.59.05 PM-min.png")](https://elitemacx86.com/attachments/screen-shot-2023-06-02-at-3-59-05-pm-min-png.6008/)  

Starting from macOS Monterey 12.3, Apple removed support for Python 2.7 and if you continue to download macOS using the macadmin script, it will throw an error:

`This tool requires the Python xattr module. Perhaps run 'pip install xattr' to install it.`

To overcome this issue, you'll need to follow the steps below:

1\. Open the Terminal and execute the following commands. When prompted for the password, enter your password.

```
#Install Command Line Tools for Xcode
sudo xcode-select --install

#Install xattr module
pip3 install xattr

#Move to the directory
cd macadmin-scripts

#Run installinstallmacos.py
sudo python3 ./installinstallmacos.py --catalogurl https://swscan.apple.com/content/catalogs/others/index-10.16-10.15-10.14-10.13-10.12-10.11-10.10-10.9-mountainlion-lion-snowleopard-leopard.merged-1.sucatalog
```


  
2\. Continue with step #6.

  
6\. When prompted for a password, enter your password. You'll get a list of available products for download.

[![Screen Shot 2023-06-02 at 4.00.04 PM-min.png](https://elitemacx86.com/data/attachments/6/6009-d0e76cbbcd73700c279e279edd4a757a.jpg "Screen Shot 2023-06-02 at 4.00.04 PM-min.png")](https://elitemacx86.com/attachments/screen-shot-2023-06-02-at-4-00-04-pm-min-png.6001/)  
7\. Depending on the macOS version you want to install, type the number left to the macOS version and press enter key. The script will start downloading the files.  
[![Screen Shot 2023-06-02 at 4.00.34 PM-min.png](https://elitemacx86.com/data/attachments/6/6011-7c8c3446402940bb19e3f24e1c407f9d.jpg "Screen Shot 2023-06-02 at 4.00.34 PM-min.png")](https://elitemacx86.com/attachments/screen-shot-2023-06-02-at-4-00-34-pm-min-png.6003/)  
Once the download is completed, you'll see the following screen:  
[![Screen Shot 2023-06-02 at 4.12.31 PM-min.png](https://elitemacx86.com/data/attachments/6/6012-84569e44eacf6a2e45ab7d0fba25c344.jpg "Screen Shot 2023-06-02 at 4.12.31 PM-min.png")](https://elitemacx86.com/attachments/screen-shot-2023-06-02-at-4-12-31-pm-min-png.6004/)  
8\. Quit Terminal. You'll find the macOS Installer in a DMG file in `/Users/yourusername` directory

[![Screen Shot 2023-06-02 at 4.56.52 PM-min.png](https://elitemacx86.com/data/attachments/6/6013-d29d091bbe792592c25590e9ff4be674.jpg "Screen Shot 2023-06-02 at 4.56.52 PM-min.png")](https://elitemacx86.com/attachments/screen-shot-2023-06-02-at-4-56-52-pm-min-png.6005/)  
9\. Mount the .DMG file by double-clicking it.  
[![Screen Shot 2023-06-02 at 4.13.07 PM-min.png](https://elitemacx86.com/data/attachments/6/6014-110575f6ecd7e9c26024027a804d28f7.jpg "Screen Shot 2023-06-02 at 4.13.07 PM-min.png")](https://elitemacx86.com/attachments/screen-shot-2023-06-02-at-4-13-07-pm-min-png.6006/)  
10\. Once mounted, copy the `Install macOS.app` to the `/Applications` folder of your Mac.

[![Screenshot 2023-06-02 at 3.03.46 AM-min.png](https://elitemacx86.com/data/attachments/6/6017-d52c8ce65e474c2104da17450dba9886.jpg "Screenshot 2023-06-02 at 3.03.46 AM-min.png")](https://elitemacx86.com/attachments/screenshot-2023-06-02-at-3-03-46-am-min-png.6009/)  
Once copied, Install macOS.app will appear in your Applications folder:  
[![Screen Shot 2023-06-02 at 4.34.08 PM-min.png](https://elitemacx86.com/data/attachments/6/6015-bbbea294e426330ccf3fa30e6b616f31.jpg "Screen Shot 2023-06-02 at 4.34.08 PM-min.png")](https://elitemacx86.com/attachments/screen-shot-2023-06-02-at-4-34-08-pm-min-png.6007/)  
11\. Verify the size of the downloaded installer by getting the info of the Installer. Usually, this should be near about 12GiB approx in size. This is required as sometimes the installer gets downloaded as an incomplete installer which can prevent booting the macOS/OS X installer.  
[![Screenshot 2023-06-02 at 12.25.29 PM-min.png](https://elitemacx86.com/data/attachments/6/6008-cd6c55f1eb2bd529906cd5c2f08977db.jpg "Screenshot 2023-06-02 at 12.25.29 PM-min.png")](https://elitemacx86.com/attachments/screenshot-2023-06-02-at-12-25-29-pm-min-png.6000/)  
(e). Method #5: Using gibMacOS  
1\. Install Command Line Tools, if you haven't already.  
2\. Open the Terminal and execute the following commands:

```
git clone https://github.com/corpnewt/gibMacOS

#Move to the directory
cd gibMacOS

#Run gibMacOS
sudo /.gibMacOS.command
```


![Screenshot 2024-06-10 at 7.01.25 PM-min.png](https://elitemacx86.com/attachments/screenshot-2024-06-10-at-7-01-25-pm-min-png.7769/ "Screenshot 2024-06-10 at 7.01.25 PM-min.png")

  
3\. When prompted for a password, enter your password. You'll get a list of available products for download:  

![Screenshot 2024-06-10 at 7.01.36 PM-min.png](https://elitemacx86.com/attachments/screenshot-2024-06-10-at-7-01-36-pm-min-png.7770/ "Screenshot 2024-06-10 at 7.01.36 PM-min.png")

  
2\. Type M and press the enter key to change the Max-OS version.  

![Screenshot 2024-06-10 at 7.01.46 PM-min.png](https://elitemacx86.com/attachments/screenshot-2024-06-10-at-7-01-46-pm-min-png.7771/ "Screenshot 2024-06-10 at 7.01.46 PM-min.png")

  
3\. When prompted, type 15 and press enter key, which is for macOS Sequoia.  

![Screenshot 2024-06-10 at 7.02.07 PM-min.png](https://elitemacx86.com/attachments/screenshot-2024-06-10-at-7-02-07-pm-min-png.7772/ "Screenshot 2024-06-10 at 7.02.07 PM-min.png")

  
Once changed, you'll see something similar:  

![Screenshot 2024-06-10 at 7.02.17 PM-min.png](https://elitemacx86.com/attachments/screenshot-2024-06-10-at-7-02-17-pm-min-png.7773/ "Screenshot 2024-06-10 at 7.02.17 PM-min.png")

4\. Type C and press the enter key to change the Catalog.

![Screenshot 2024-06-10 at 7.02.22 PM-min.png](https://elitemacx86.com/attachments/screenshot-2024-06-10-at-7-02-22-pm-min-png.7774/ "Screenshot 2024-06-10 at 7.02.22 PM-min.png")

  
5\. When prompted, type 4 and press the enter key, which is for the Developer seed.  

![Screenshot 2024-06-10 at 7.02.31 PM-min.png](https://elitemacx86.com/attachments/screenshot-2024-06-10-at-7-02-31-pm-min-png.7775/ "Screenshot 2024-06-10 at 7.02.31 PM-min.png")

  
Once changed, you'll see something similar:  

![Screenshot 2024-06-10 at 7.02.39 PM-min.png](https://elitemacx86.com/attachments/screenshot-2024-06-10-at-7-02-39-pm-min-png.7776/ "Screenshot 2024-06-10 at 7.02.39 PM-min.png")

  
6\. Type 1 and press enter key, which is for macOS Sequoia.  

![Screenshot 2024-06-10 at 7.03.05 PM-min.png](https://elitemacx86.com/attachments/screenshot-2024-06-10-at-7-03-05-pm-min-png.7777/ "Screenshot 2024-06-10 at 7.03.05 PM-min.png")

  
The script will start downloading the files.  

![Screenshot 2024-06-10 at 7.03.18 PM-min.png](https://elitemacx86.com/attachments/screenshot-2024-06-10-at-7-03-18-pm-min-png.7778/ "Screenshot 2024-06-10 at 7.03.18 PM-min.png")

  
Once the download is completed, you'll see the following screen:  

![Screenshot 2024-06-10 at 7.11.08 PM-min.png](https://elitemacx86.com/attachments/screenshot-2024-06-10-at-7-11-08-pm-min-png.7779/ "Screenshot 2024-06-10 at 7.11.08 PM-min.png")

  
Press enter key and you'll get back to the main screen:  

![Screenshot 2024-06-10 at 7.11.56 PM-min.png](https://elitemacx86.com/attachments/screenshot-2024-06-10-at-7-11-56-pm-min-png.7780/ "Screenshot 2024-06-10 at 7.11.56 PM-min.png")

  
7\. Once downloaded, quit Terminal using Q.  

![Screenshot 2024-06-10 at 7.12.02 PM-min.png](https://elitemacx86.com/attachments/screenshot-2024-06-10-at-7-12-02-pm-min-png.7781/ "Screenshot 2024-06-10 at 7.12.02 PM-min.png")

  

![Screenshot 2024-06-10 at 7.12.06 PM-min.png](https://elitemacx86.com/attachments/screenshot-2024-06-10-at-7-12-06-pm-min-png.7782/ "Screenshot 2024-06-10 at 7.12.06 PM-min.png")

  
The downloads can be found under the following directory:  

![Screenshot 2024-06-10 at 7.11.43 PM-min.png](https://elitemacx86.com/attachments/screenshot-2024-06-10-at-7-11-43-pm-min-png.7783/ "Screenshot 2024-06-10 at 7.11.43 PM-min.png")

  
1\. Open the InstallAssistant.pkg file using double click which you downloaded in step #2.  

![Screenshot 2024-06-10 at 12.49.40 PM-min.png](https://elitemacx86.com/attachments/screenshot-2024-06-10-at-12-49-40-pm-min-png.7761/ "Screenshot 2024-06-10 at 12.49.40 PM-min.png")

2\. Install the package file. When prompted, enter your password and proceed with the installation.

![Screenshot 2024-06-10 at 12.49.43 PM-min.png](https://elitemacx86.com/attachments/screenshot-2024-06-10-at-12-49-43-pm-min-png.7762/ "Screenshot 2024-06-10 at 12.49.43 PM-min.png")

![Screenshot 2024-06-10 at 12.50.33 PM-min.png](https://elitemacx86.com/attachments/screenshot-2024-06-10-at-12-50-33-pm-min-png.7763/ "Screenshot 2024-06-10 at 12.50.33 PM-min.png")

Once the package file is installed, the installer will prompt you to either delete or keep the package installer. Select one as per your choice.

![Screenshot 2024-06-10 at 12.50.40 PM-min.png](https://elitemacx86.com/attachments/screenshot-2024-06-10-at-12-50-40-pm-min-png.7764/ "Screenshot 2024-06-10 at 12.50.40 PM-min.png")

After installing the package file, Install macOS.app will appear under the Applications folder:

![Screenshot 2024-06-10 at 12.51.30 PM-min.png](https://elitemacx86.com/attachments/screenshot-2024-06-10-at-12-51-30-pm-min-png.7765/ "Screenshot 2024-06-10 at 12.51.30 PM-min.png")

  
Verify the size of the downloaded installer by getting the installer's info. Usually, this should be near about 12GiB approx in size. This is required as sometimes the installer gets downloaded as an incomplete installer which can prevent booting the macOS/OS X installer.

**QUICK INFO: Booting macOS Big Sur 11.3 and newer without mapping of the USB Ports will result in boot loop due to the broken XhciPort Limit. Therefore, it is recommended to install 11.2.3 or map your USB Ports first using Windows before installing macOS. Note that if you have already [**mapped your USB Ports**](https://elitemacx86.com/threads/how-to-map-your-usb-ports-on-macos.581/), you can boot macOS 11.3 and newer without such issues. See** [**Mapping USB Ports**](https://elitemacx86.com/threads/how-to-map-your-usb-ports-on-macos.581/) **for more information.**

**2\. Downloading macOS (10.7-10.12)**

Using this method, you can download a complete installer, directly from Apple. However, this method is limited and will allow you to download OS X 10.7 to macOS 10.12 only. Follow the steps below to download OS X 10.7 to macOS Sierra (10.12). For macOS High Sierra (10.13) and later, see Downloading Modern OS. Follow the steps below to download macOS Sierra and prior.

> **QUICK INFO:  
> OS X 10.9 (Mavericks) isn't available with this method. If you're looking for OS X 10.9, see Online Method for more information.**

  
1\. Depending on the macOS version you want to install, download the required version by opening the link below. This will download a .DMG file.

```
macOS Sierra (10.12)
http://updates-http.cdn-apple.com/2019/cert/061-39476-20191023-48f365f4-0015-4c41-9f44-39d3d2aca067/InstallOS.dmg

OS X El Capitan (10.11)
http://updates-http.cdn-apple.com/2019/cert/061-41424-20191024-218af9ec-cf50-4516-9011-228c78eda3d2/InstallMacOSX.dmg

OS X Yosemite (10.10)
http://updates-http.cdn-apple.com/2019/cert/061-41343-20191023-02465f92-3ab5-4c92-bfe2-b725447a070d/InstallMacOSX.dmg

OS X Mountain Lion (10.8)
https://updates.cdn-apple.com/2021/macos/031-0627-20210614-90D11F33-1A65-42DD-BBEA-E1D9F43A6B3F/InstallMacOSX.dmg

OS X Lion (10.7)
https://updates.cdn-apple.com/2021/macos/041-7683-20210614-E610947E-C7CE-46EB-8860-D26D71F0D3EA/InstallMacOSX.dmg
```


  
2\. Once the .DMG file is downloaded, double-click the .DMG file to open it and you'll see a `.pkg` file within.  
3\. Double-click the .pkg file and this will automatically move the macOS/OS X installer into your /Applications folder upon installing .pkg file. However, if you're on a new macOS/OS X version than the .pkg file you're installing, you'll receive the following error:

[![Screen Shot 2022-10-26 at 1.37.37 AM-min.png](https://elitemacx86.com/data/attachments/5/5199-46bbac484a905a46d02a400b166375cb.jpg "Screen Shot 2022-10-26 at 1.37.37 AM-min.png")](https://elitemacx86.com/attachments/screen-shot-2022-10-26-at-1-37-37-am-min-png.5191/)

This is because the package checks for system compatibility, even if you're creating a Bootable USB for a different target computer. To fix this issue, you'll need to manually extract the installer from the .pkg file. Follow the steps below to extract the installer.

**Extracting the Installer**

**(a). Method #1: Using App Method**

1\. Download the required macOS/OS X version using the guide above.  
2\. Download and install Pacifist.  
3\. Mount the .DMG file  
4\. Open the .pkg file using Pacifist and you'll see the following.

From the top, click on Package Resources, just next to Archive Contents and you'll see something similar to the following

Expand the InstallOS/Instal OS X Contents and you'll see something similar to the following

Right-click on InstallESD.dmg and select Extract to Custom Location and you'll see something like below

When prompted, select the location for storing the .dmg file and click on Choose

When prompted, click on Extract to begin the extraction process.

When prompted, enter your password and click on OK button.

Pacifist will start extracting the ESD file to your chosen location.

For both the Offline Methods described above, you can use the `createinstallmedia` command to create a Bootable USB.

Verify the size of the downloaded installer by getting the info of the Installer. Usually, this should be near about 8GiB approx in size. This is required as sometimes the installer gets downloaded as an incomplete installer which can prevent booting the macOS/OS X installer.

**3\. Hybrid: Downloading OS X Lion (10.7.5) and Later**

Using this Hybrid method, you can download a variety of installers, including Beta installers. However, by default, Mist will not include Beta installer downloads, unless you explicitly allow it to download such. Using this method, you can download the current macOS installer as well as Legacy macOS installers such as macOS Sierra and prior (except for Mavericks (10.9) and Snow Leopard (10.6)) in one click. Moreover, unlike OCLP, Mist is also capable of downloading the macOS installers and has the ability to create the bootable USB for OS X Yosemite (10.10.X) and later. Follow the steps below to download OS X Yosemite (10.10.x) and later.

1\. Download Mist  
2\. Once the .DMG is downloaded, double-click the .DMG file to open it.  
3\. Move the Mist to your /Applications folder  
4\. Open Mist.app to launch the Application

[![Screen Shot 2023-09-30 at 10.30.44 PM-min.png](https://elitemacx86.com/data/attachments/6/6559-63bc7ce9f4eacf10791bc1503964aee9.jpg "Screen Shot 2023-09-30 at 10.30.44 PM-min.png")](https://elitemacx86.com/attachments/screen-shot-2023-09-30-at-10-30-44-pm-min-png.6551/)  
5\. From the Menu bar, select Mist and click on Install Privileged Helper Tool. When prompted, enter your password and click on Install Helper.

 [![Screen Shot 2023-09-30 at 10.31.07 PM-min.png](https://elitemacx86.com/data/attachments/6/6570-c617071ce23df535d1f7e1f891cead25.jpg "Screen Shot 2023-09-30 at 10.31.07 PM-min.png")](https://elitemacx86.com/attachments/screen-shot-2023-09-30-at-10-31-07-pm-min-png.6562/)[![Screen Shot 2023-09-30 at 10.31.14 PM-min.png](https://elitemacx86.com/data/attachments/6/6571-f0991dd79369f413d7d9beee98da1a3b.jpg "Screen Shot 2023-09-30 at 10.31.14 PM-min.png")](https://elitemacx86.com/attachments/screen-shot-2023-09-30-at-10-31-14-pm-min-png.6563/)  
You can also install the Privileged Helper Tool from the Mist>Preferences  
[![Screen Shot 2023-10-01 at 4.04.56 PM-min.png](https://elitemacx86.com/data/attachments/6/6565-8ce8679e304d1b2acc5b26bd3ecd8f3d.jpg "Screen Shot 2023-10-01 at 4.04.56 PM-min.png")](https://elitemacx86.com/attachments/screen-shot-2023-10-01-at-4-04-56-pm-min-png.6557/)

6\. From the Menu bar, select Mist>Preferences and ensure the Privileged Helper Tool is installed.

[![Screen Shot 2023-09-30 at 10.32.07 PM-min.png](https://elitemacx86.com/data/attachments/6/6566-432035fe4c087753c8b3c9ea309061b8.jpg "Screen Shot 2023-09-30 at 10.32.07 PM-min.png")](https://elitemacx86.com/attachments/screen-shot-2023-09-30-at-10-32-07-pm-min-png.6558/)  
7\. In the same window, click on Check now to ensure you're using the latest version of the Mist. If there's an update available, install the update and check for the updates again to ensure you're running the latest version.

[![Screen Shot 2023-09-30 at 10.32.11 PM-min.png](https://elitemacx86.com/data/attachments/6/6567-07a56d74cff459254022fafda16f8ba0.jpg "Screen Shot 2023-09-30 at 10.32.11 PM-min.png")](https://elitemacx86.com/attachments/screen-shot-2023-09-30-at-10-32-11-pm-min-png.6559/)  
8\. Click on OK and close the Preferences and you'll be back at the app.

By default, Mist will not include Beta installer downloads, unless you explicitly allow it to download such. This feature can be useful for downloading a version of macOS that is still in the Beta stage and has not been publicly released yet. Users who do not wish to download Beta Installer can ignore this step. Follow the steps below to download macOS Beta Installer.

1\. Open Mist>Preferences and go to the Installers tab.  
[![Screen Shot 2023-10-01 at 1.01.35 AM-min.png](https://elitemacx86.com/data/attachments/6/6580-0eb0dd12d3248008e7a3a09db1a63438.jpg "Screen Shot 2023-10-01 at 1.01.35 AM-min.png")](https://elitemacx86.com/attachments/screen-shot-2023-10-01-at-1-01-35-am-min-png.6572/)  
2\. In the Software Update Catalogs section, select the type of Catalog i.e. Customer, Developer, or Public. You can either select them individually or can select all of them.

[![Screen Shot 2023-10-01 at 1.01.55 AM-min.png](https://elitemacx86.com/data/attachments/6/6581-93ae3d71c7462e88f9d09eb73ce53687.jpg "Screen Shot 2023-10-01 at 1.01.55 AM-min.png")](https://elitemacx86.com/attachments/screen-shot-2023-10-01-at-1-01-55-am-min-png.6573/)  
3\. Close Preferences and you'll be back in the App. From the bottom, select Include Betas and then click on the Refresh button at the top, left to the Search menu and Mist will rescan the catalogs.

 [![Screen Shot 2023-10-01 at 1.02.05 AM-min.png](https://elitemacx86.com/data/attachments/6/6582-3d26fcd4eef513709c7ccd4f11812eeb.jpg "Screen Shot 2023-10-01 at 1.02.05 AM-min.png")](https://elitemacx86.com/attachments/screen-shot-2023-10-01-at-1-02-05-am-min-png.6574/)[![Screen Shot 2023-10-01 at 1.02.09 AM-min.png](https://elitemacx86.com/data/attachments/6/6583-b81d2f1a2516c8a9f9813048366c39a1.jpg "Screen Shot 2023-10-01 at 1.02.09 AM-min.png")](https://elitemacx86.com/attachments/screen-shot-2023-10-01-at-1-02-09-am-min-png.6575/)  
Now, you should be able to see the Beta Installers in the download list.

[![Screen Shot 2023-10-01 at 1.02.19 AM-min.png](https://elitemacx86.com/data/attachments/6/6584-2265ecd9fbbec954327ad4c6553e4300.jpg "Screen Shot 2023-10-01 at 1.02.19 AM-min.png")](https://elitemacx86.com/attachments/screen-shot-2023-10-01-at-1-02-19-am-min-png.6576/)  
4\. Continue from the step #10

  
9\. By default, the Mist will land on the Firmware download. Click on the Installers tab.

 [![Screen Shot 2023-09-30 at 10.30.55 PM-min.png](https://elitemacx86.com/data/attachments/6/6568-121ee11a525e7535c12f03dee6defcef.jpg "Screen Shot 2023-09-30 at 10.30.55 PM-min.png")](https://elitemacx86.com/attachments/screen-shot-2023-09-30-at-10-30-55-pm-min-png.6560/)[![Screen Shot 2023-09-30 at 10.33.02 PM-min.png](https://elitemacx86.com/data/attachments/6/6569-09677629a673b037047ff550168fa3d0.jpg "Screen Shot 2023-09-30 at 10.33.02 PM-min.png")](https://elitemacx86.com/attachments/screen-shot-2023-09-30-at-10-33-02-pm-min-png.6561/)  
10\. Depending on the macOS version you want to install, click on the download button right next to the macOS version.  
[![Screen Shot 2023-09-30 at 10.33.02 PM-min.png](https://elitemacx86.com/data/attachments/6/6569-09677629a673b037047ff550168fa3d0.jpg "Screen Shot 2023-09-30 at 10.33.02 PM-min.png")](https://elitemacx86.com/attachments/screen-shot-2023-09-30-at-10-33-02-pm-min-png.6561/)  
When downloading a legacy version of macOS, i.e. macOS Sierra and prior, you may get this warning. Simply click on Continue to proceed with the download.  
[![Screen Shot 2023-10-01 at 1.02.40 AM-min.png](https://elitemacx86.com/data/attachments/6/6585-9f98bc77fee97122623dbf0ea3c4dbad.jpg "Screen Shot 2023-10-01 at 1.02.40 AM-min.png")](https://elitemacx86.com/attachments/screen-shot-2023-10-01-at-1-02-40-am-min-png.6577/)  
NOTE: If the Privileged Helper Tool is not installed, you'll get the following error when you click on the Download button.

[![Screen Shot 2023-10-01 at 9.17.25 PM-min.png](https://elitemacx86.com/data/attachments/6/6577-2227e8f57aa33b6502778d08bcaddf91.jpg "Screen Shot 2023-10-01 at 9.17.25 PM-min.png")](https://elitemacx86.com/attachments/screen-shot-2023-10-01-at-9-17-25-pm-min-png.6569/)  
11\. When prompted, select the desired location to download the macOS installer and click on Save  
[![Screen Shot 2023-09-30 at 10.34.20 PM-min.png](https://elitemacx86.com/data/attachments/6/6572-8f0abc74d2b357788d970c47ce49c263.jpg "Screen Shot 2023-09-30 at 10.34.20 PM-min.png")](https://elitemacx86.com/attachments/screen-shot-2023-09-30-at-10-34-20-pm-min-png.6564/)  
Once you click on Save, the Mist will start downloading the requested installer.  
 [![Screen Shot 2023-09-30 at 10.34.40 PM-min.png](https://elitemacx86.com/data/attachments/6/6573-43a61f482ab9d8772e4deb4e807267c5.jpg "Screen Shot 2023-09-30 at 10.34.40 PM-min.png")](https://elitemacx86.com/attachments/screen-shot-2023-09-30-at-10-34-40-pm-min-png.6565/)[![Screen Shot 2023-09-30 at 10.35.06 PM-min.png](https://elitemacx86.com/data/attachments/6/6574-b1e1bc00f289f08417d24210f5c83850.jpg "Screen Shot 2023-09-30 at 10.35.06 PM-min.png")](https://elitemacx86.com/attachments/screen-shot-2023-09-30-at-10-35-06-pm-min-png.6566/)  
Once the installer is downloaded, the Mist will save the installer in the particular location you chose previously and will also perform a cleanup.  
[![Screen Shot 2023-09-30 at 11.01.28 PM-min.png](https://elitemacx86.com/data/attachments/6/6575-b5037fe719107d9c329f5717a9bc8a62.jpg "Screen Shot 2023-09-30 at 11.01.28 PM-min.png")](https://elitemacx86.com/attachments/screen-shot-2023-09-30-at-11-01-28-pm-min-png.6567/)  
Once tasks are completed, you'll see the tasks in green and you'll find the macOS Installer in the directory you chose earlier.  
 [![Screen Shot 2023-09-30 at 11.01.58 PM-min.png](https://elitemacx86.com/data/attachments/6/6576-861d301444ec7aa5bb623b7dd866da73.jpg "Screen Shot 2023-09-30 at 11.01.58 PM-min.png")](https://elitemacx86.com/attachments/screen-shot-2023-09-30-at-11-01-58-pm-min-png.6568/)[![Screen Shot 2023-10-01 at 8.48.04 AM-min.png](https://elitemacx86.com/data/attachments/6/6579-ebe70643cdf433d1f4760f3734d2e559.jpg "Screen Shot 2023-10-01 at 8.48.04 AM-min.png")](https://elitemacx86.com/attachments/screen-shot-2023-10-01-at-8-48-04-am-min-png.6571/)  
12\. Click on Close and Quit Mist.

**4\. Preparing Installer**

Once you have obtained the desired copy of macOS Installer, you can prepare the installer. To prepare the installer, we'll use `createinstallmedia` command.

OpenCore and the macOS/OS X Installer are placed on separate partitions on the Bootable USB. There are two possible options for the USB partitions which are listed below. Depending on your choice, choose one of the methods to create the Bootable USB.



* Method: MBR
  * Notes: MBR with a FAT32 partition for OpenCore.Separate HFS+J partition for the macOS/OS X installer.EFI Partition is always mounted automatically when the USB is plugged in.
* Method: GPT
  * Notes: GPT with a single HFS+J partition for the macOS/OS X installer.Hidden EFI partition (automatically created).Need to mount EFI Partition every time you want to access the ESP.


We recommend using MBR with two partitions as most of the computers can boot from it (including Legacy), and it is convenient that the EFI partition will mount automatically when the USB is inserted. This can be very helpful in case if you want to make any changes to the contents of the EFI Partition.

**(a). Method #1: MBR**

> **QUICK INFO:  
> Be careful with `diskutil` commands as you can lose data without a mechanism for recovery if you repartition the wrong disk.  
> If you're using OS X EL Capitan or prior, Disk Utility cannot be used for MBR partitioning.**

  
1\. Insert your USB Flash Drive  
2\. Open the Terminal and execute the following commands  
  
In our case, the output is

As you can see, the USB Flash drive is

To partition,

```
diskutil partitionDisk /dev/disk1 2 MBR FAT32 "OpenCore EFI" 200Mi HFS+J "macOS Installer" R
```


  
You'll see the following as an output of the operation:

3\. Quit Terminal

**(b). Method #2: GPT**

1\. Insert your USB Flash Drive  
2\. Open Disk Utility. The Disk Utility is located at `/Applications/Utilities`.  
3\. Click on `View` and then select Show All Devices  
[![Screenshot 2023-06-01 at 10.35.30 AM-min.png](https://elitemacx86.com/data/attachments/6/6036-9b6922e9f11263ef499c56a401d83cab.jpg "Screenshot 2023-06-01 at 10.35.30 AM-min.png")](https://elitemacx86.com/attachments/screenshot-2023-06-01-at-10-35-30-am-min-png.6028/)  
4\. Select your target USB Flash Drive in the left pane and click on Erase button, at the top and a popup will appear. Use the following parameters to erase your drive.

**Name**: USB  
**Format**: Mac OS Extended (Journaled)  
**Scheme**: GUID Partition Map  
[![Screenshot 2023-05-19 at 7.35.38 AM-min.png](https://elitemacx86.com/data/attachments/5/5868-c2ed6180ca265f01e5d87aa92197bc74.jpg "Screenshot 2023-05-19 at 7.35.38 AM-min.png")](https://elitemacx86.com/attachments/screenshot-2023-05-19-at-7-35-38-am-min-png.5860/)  
**NOTE:**  
Do NOT use any other format other than Mac OS Extended (Journaled) to erase the USB.

5\. When done, click on Done and close Disk Utility. You can click on Show Details to verify if the USB has been erased with HFS+ format.  
[![Screenshot 2023-05-19 at 7.36.03 AM-min.png](https://elitemacx86.com/data/attachments/5/5869-10ae6339a87b83402935ee5fc8198d05.jpg "Screenshot 2023-05-19 at 7.36.03 AM-min.png")](https://elitemacx86.com/attachments/screenshot-2023-05-19-at-7-36-03-am-min-png.5861/)

6\. Launch Terminal, located at `/Applications/Utilities`.  
7\. Depending on the macOS/OS X version you want to install, execute the appropriate `createinstallmedia` command in the Terminal.

```
macOS Sonoma (14.x):
sudo /Applications/Install\ macOS\ Sonoma.app/Contents/Resources/createinstallmedia --volume /Volumes/USB

macOS Ventura:
sudo /Applications/Install\ macOS\ Ventura.app/Contents/Resources/createinstallmedia --volume /Volumes/USB

macOS Monterey:
sudo /Applications/Install\ macOS\ Monterey.app/Contents/Resources/createinstallmedia --volume /Volumes/USB

macOS Big Sur:
sudo /Applications/Install\ macOS\ Big\ Sur.app/Contents/Resources/createinstallmedia --volume /Volumes/USB

macOS Catalina:
sudo /Applications/Install\ macOS\ Catalina.app/Contents/Resources/createinstallmedia --volume /Volumes/USB

macOS Mojave:
sudo /Applications/Install\ macOS\ Mojave.app/Contents/Resources/createinstallmedia --volume /Volumes/USB

macOS High Sierra:
sudo /Applications/Install\ macOS\ High\ Sierra.app/Contents/Resources/createinstallmedia --volume /Volumes/USB

macOS Sierra:
sudo /Applications/Install\ macOS\ Sierra.app/Contents/Resources/createinstallmedia --volume /Volumes/USB --applicationpath /Applications/Install\ macOS\ Sierra.app

OS X El Capitan:
sudo /Applications/Install\ OS\ X\ El\ Capitan.app/Contents/Resources/createinstallmedia --volume /Volumes/USB --applicationpath /Applications/Install\ OS\ X\ El\ Capitan.app

OX X Yosemite:
sudo /Applications/Install\ OS\ X\ Yosemite.app/Contents/Resources/createinstallmedia --volume /Volumes/USB --applicationpath /Applications/Install\ OS\ X\ Yosemite.app

OS X Mavericks:
sudo /Applications/Install\ OS\ X\ Mavericks.app/Contents/Resources/createinstallmedia --volume /Volumes/USB --applicationpath /Applications/Install\ OS\ X\ Mavericks.app --nointeraction
```


  
8\. When prompted, enter your password.  
[![Screenshot 2023-06-02 at 9.27.37 AM-min.png](https://elitemacx86.com/data/attachments/6/6037-394a243097fd0b1006d12316ba3152a8.jpg "Screenshot 2023-06-02 at 9.27.37 AM-min.png")](https://elitemacx86.com/attachments/screenshot-2023-06-02-at-9-27-37-am-min-png.6029/)  
NOTE: Sometimes, after you enter your password, the createinstallmedia will not start the process. It's due to the USB being in a locked state. In such cases, a workaround is to remove and replug the USB Flash Drive or just reformat it as shown in the above steps.

9\. Press (`Y`) to confirm and then press enter key and it will start erasing the disk and will create macOS Bootable USB.  
 [![Screenshot 2023-06-02 at 9.27.47 AM-min.png](https://elitemacx86.com/data/attachments/6/6038-4219aab7c584d171231f43e787912ff1.jpg "Screenshot 2023-06-02 at 9.27.47 AM-min.png")](https://elitemacx86.com/attachments/screenshot-2023-06-02-at-9-27-47-am-min-png.6030/)[![Screenshot 2023-06-02 at 9.27.53 AM-min.png](https://elitemacx86.com/data/attachments/6/6039-66fb549d2b93b947da4f0f8a975aacda.jpg "Screenshot 2023-06-02 at 9.27.53 AM-min.png")](https://elitemacx86.com/attachments/screenshot-2023-06-02-at-9-27-53-am-min-png.6031/)  
If you're on Big Sur or later, the Terminal may prompt for access. When prompted, click on OK to allow access.

![Screenshot 2023-06-05 at 1.31.30 PM-min.png](https://elitemacx86.com/attachments/screenshot-2023-06-05-at-1-31-30%E2%80%AFpm-min-png.6160/ "Screenshot 2023-06-05 at 1.31.30 PM-min.png")

  
Once the process is completed, you'll see the following screen:  
[![Screenshot 2023-06-02 at 9.45.35 AM-min.png](https://elitemacx86.com/data/attachments/6/6040-75fbdf7d004a3dffdbc12135aa49e56d.jpg "Screenshot 2023-06-02 at 9.45.35 AM-min.png")](https://elitemacx86.com/attachments/screenshot-2023-06-02-at-9-45-35-am-min-png.6032/)  
10\. Quit Terminal. The USB Flash Drive will be renamed itself as per the OS X/macOS Installer

**QUICK NOTE:**  

*   macOS/OS X Installer must exist in the Applications folder.
*   Createinstallmedia is only supported by OS X Mavericks and newer. For OS X 10.4-10.8, see Legacy OS X for more information.

**(c). Method #3: Using OCLP**

Using OCLP, you cannot only download the macOS Installer but can also create the Bootable USB in a single click. However, this method is limited to macOS High Sierra (10.13.x) and later only. If you want to install macOS Sierra (10.12.x) or prior, please use. Follow the steps below to create a macOS Bootable USB using OCLP.

1\. Insert your USB Flash Drive  
2\. Download OpenCore Legacy Patcher, if you haven't already  
3\. Open OCLP and you'll see something similar to the screenshot attached below.  
[![Screen Shot 2023-09-26 at 10.11.14 PM-min.png](https://elitemacx86.com/data/attachments/6/6526-fe52e7552027d138d6b33cabdcdb64c5.jpg "Screen Shot 2023-09-26 at 10.11.14 PM-min.png")](https://elitemacx86.com/attachments/screen-shot-2023-09-26-at-10-11-14-pm-min-png.6518/)  
4\. Click on Use existing macOS Installer. Depending on the macOS/OS X version you want to install, select the appropriate installer from the drop-down list when prompted.  
 [![Screen Shot 2023-09-26 at 10.11.18 PM-min.png](https://elitemacx86.com/data/attachments/6/6527-fb2ddd759be173f639c7860806479481.jpg "Screen Shot 2023-09-26 at 10.11.18 PM-min.png")](https://elitemacx86.com/attachments/screen-shot-2023-09-26-at-10-11-18-pm-min-png.6519/)[![Screen Shot 2023-09-27 at 12.37.53 AM-min.png](https://elitemacx86.com/data/attachments/6/6541-7754f18f568b48d94cebdea168e8433c.jpg "Screen Shot 2023-09-27 at 12.37.53 AM-min.png")](https://elitemacx86.com/attachments/screen-shot-2023-09-27-at-12-37-53-am-min-png.6533/)  
NOTE: OCLP will find all the installers automatically if it exists in the `/Applications` directory. If you do not have an existing installer, you can download one using OCLP. See Method #3 for more information.

5\. When prompted, select the appropriate USB Flash Drive from the drop-down list for creating the Bootable USB.

[![Screen Shot 2023-09-27 at 12.38.09 AM-min.png](https://elitemacx86.com/data/attachments/6/6542-a03f087cae15b5abb3eb4572283f8046.jpg "Screen Shot 2023-09-27 at 12.38.09 AM-min.png")](https://elitemacx86.com/attachments/screen-shot-2023-09-27-at-12-38-09-am-min-png.6534/)  
6\. OCLP will now ask for a confirmation to erase the target Flash Drive. Click on Yes to proceed with the erasure of the USB Flash Drive. When prompted, enter your password and click on OK.  
 [![Screen Shot 2023-09-27 at 12.38.13 AM-min.png](https://elitemacx86.com/data/attachments/6/6543-019da2d5fbc55312b2041c530871b108.jpg "Screen Shot 2023-09-27 at 12.38.13 AM-min.png")](https://elitemacx86.com/attachments/screen-shot-2023-09-27-at-12-38-13-am-min-png.6535/)[![Screen Shot 2023-09-27 at 12.38.23 AM-min.png](https://elitemacx86.com/data/attachments/6/6544-1464410251c097bff09f535dc0d3afbe.jpg "Screen Shot 2023-09-27 at 12.38.23 AM-min.png")](https://elitemacx86.com/attachments/screen-shot-2023-09-27-at-12-38-23-am-min-png.6536/)  
Now OCLP will create the macOS Bootable USB. Depending on the USB Flash Drive quality and size, it may take a while.  
[![Screen Shot 2023-09-27 at 1.12.05 AM-min.png](https://elitemacx86.com/data/attachments/6/6545-646b297ce48c8b0d6b5e9e90f250951a.jpg "Screen Shot 2023-09-27 at 1.12.05 AM-min.png")](https://elitemacx86.com/attachments/screen-shot-2023-09-27-at-1-12-05-am-min-png.6537/)  
Once OCLP is completed, it will verify the installer's integrity.  
[![Screen Shot 2023-09-27 at 10.29.02 PM-min.png](https://elitemacx86.com/data/attachments/6/6546-23f2d3a46a6314d10d7b75ea4ce8b6a2.jpg "Screen Shot 2023-09-27 at 10.29.02 PM-min.png")](https://elitemacx86.com/attachments/screen-shot-2023-09-27-at-10-29-02-pm-min-png.6538/)  
7\. Once the process is completed, you'll see the following screen. Click on OK and then quit OCLP.

[![Screen Shot 2023-09-27 at 1.06.17 AM-min.png](https://elitemacx86.com/data/attachments/6/6547-30e53763955f0eb66b8ae41edbefe8d9.jpg "Screen Shot 2023-09-27 at 1.06.17 AM-min.png")](https://elitemacx86.com/attachments/screen-shot-2023-09-27-at-1-06-17-am-min-png.6539/)  
**(d). Method #4: Using Mist**

Unlike OCLP, you cannot only download the macOS Installer but can also create the Bootable USB in a single click using Mist. However, this method is limited to OS X Yosemite (10.10.X) and later only. If you want to install OS X Mountain Lion (10.8.5) or prior, please use . Follow the steps below to create a macOS Bootable USB using Mist.

1\. Insert your USB Flash Drive  
2\. Follow Step #1-#8 in the Hybrid section  
3\. Depending on the OS X/macOS version you want to install, click on the disk icon right next to the OS X/macOS version.

When downloading a legacy version of macOS, i.e. macOS Sierra and prior, you may get this warning. Simply click on Continue to proceed with the download.

When prompted, click on Open Disk Utility.  
Click on View and then select Show All Devices

![Screenshot 2023-06-01 at 10.35.30 AM-min.png](https://elitemacx86.com/attachments/screenshot-2023-06-01-at-10-35-30-am-min-png.6028/ "Screenshot 2023-06-01 at 10.35.30 AM-min.png")

4\. Select your target USB Flash Drive in the left pane and click on Erase button, at the top and a popup will appear. Use the following parameters to erase your drive.

**Name**: USB  
**Format**: Mac OS Extended (Journaled)  
**Scheme**: GUID Partition Map

![Screenshot 2023-05-19 at 7.35.38 AM-min.png](https://elitemacx86.com/attachments/screenshot-2023-05-19-at-7-35-38-am-min-png.5860/ "Screenshot 2023-05-19 at 7.35.38 AM-min.png")

**NOTE:**  
Do NOT use any other format other than Mac OS Extended (Journaled) to erase the USB.

5\. When done, click on Done and close Disk Utility. You can click on Show Details to verify if the USB has been erased with HFS+ format.

![Screenshot 2023-05-19 at 7.36.03 AM-min.png](https://elitemacx86.com/attachments/screenshot-2023-05-19-at-7-36-03-am-min-png.5861/ "Screenshot 2023-05-19 at 7.36.03 AM-min.png")

  
Click on the refresh button right next to the volume list and then select the target USB Flash Drive from the drop-down list and click on Select.

Once you click on Select, the Mist will start downloading the requested installer.

Once the installer is downloaded, the Mist will save the installer in a temporary location and then it will create the Bootable USB, and will also perform a cleanup. Depending on the USB Flash Drive quality and size, it may take a while.

Once tasks are completed, you'll see the tasks in green and you'll find the Bootable USB in the Finder.

Click on Close and Quit Mist.

**II. Online Method**

* * *

  
**1\. Downloading macOS (10.7-13.4)**

Using this method, you can download from OS X Lion 10.7 to macOS Ventura 13.4. However, these are the recovery image and therefore requires an internet connection to download the full installer during the time of installation. You'll need to have the exact Recovery image of the target OS you want to install. To download the recovery image, follow the steps below.

1\. Within the downloaded OpenCorePkg (RELEASE), navigate to the Utilities/macrecovery directory.

[![Screen Shot 2023-06-02 at 5.48.51 PM-min.png](https://elitemacx86.com/data/attachments/6/6019-39076b3a34f92e135a99b1cd9440be3d.jpg "Screen Shot 2023-06-02 at 5.48.51 PM-min.png")](https://elitemacx86.com/attachments/screen-shot-2023-06-02-at-5-48-51-pm-min-png.6011/)  
2\. Open the Terminal and execute the following commands.

```
#Move to the directory
cd Downloads/OpenCore-0.9.2-RELEASE/Utilities/macrecovery
```


  
NOTE: Replace the X with the OpenCore version.

3\. Depending on the macOS version you need (See Recovery Table below), execute the commands. When prompted, enter your password.

**Recovery Table**



* OS Version: OS X Lion
  * Command: python3 ./macrecovery.py -b Mac-C3EC7CD22292981F -m 00000000000F0HM00 download
* OS Version: OS X Mountain Lion
  * Command: python3 ./macrecovery.py -b Mac-7DF2A3B5E5D671ED -m 00000000000F65100 download
* OS Version: OS X Mavericks
  * Command: python3 ./macrecovery.py -b Mac-F60DEB81FF30ACF6 -m 00000000000FNN100 download
* OS Version: OS X Yosemite
  * Command: python3 ./macrecovery.py -b Mac-E43C1C25D4880AD6 -m 00000000000GDVW00 download
* OS Version: OS X El Capitan
  * Command: python3 ./macrecovery.py -b Mac-FFE5EF870D7BA81A -m 00000000000GQRX00 download
* OS Version: macOS Sierra
  * Command: python3 ./macrecovery.py -b Mac-77F17D7DA9285301 -m 00000000000J0DX00 download
* OS Version: macOS High Sierra
  * Command: python3 ./macrecovery.py -b Mac-7BA5B2D9E42DDD94 -m 00000000000J80300 download
* OS Version: macOS Mojave
  * Command: python3 ./macrecovery.py -b Mac-7BA5B2DFE22DDD8C -m 00000000000KXPG00 download
* OS Version: macOS Catalina
  * Command: python3 ./macrecovery.py -b Mac-CFF7D910A743CAAF -m 00000000000PHCD00 download
* OS Version: macOS Big Sur
  * Command: python3 ./macrecovery.py -b Mac-42FD25EABCABB274 -m 00000000000000000 download
* OS Version: macOS Monterey
  * Command: python3 ./macrecovery.py -b Mac-E43C1C25D4880AD6 -m 00000000000000000 download
* OS Version: macOS Ventura
  * Command: python3 ./macrecovery.py -b Mac-7BA5B2D9E42DDD94 -m 00000000000000000 -os latest download


  
[![Screen Shot 2023-06-02 at 5.25.34 PM-min.png](https://elitemacx86.com/data/attachments/6/6025-7fec0f7cc8574b9f7596c0fbdc132d01.jpg "Screen Shot 2023-06-02 at 5.25.34 PM-min.png")](https://elitemacx86.com/attachments/screen-shot-2023-06-02-at-5-25-34-pm-min-png.6017/)  
The script will start downloading the required recovery files:  
[![Screen Shot 2023-06-02 at 5.25.42 PM-min.png](https://elitemacx86.com/data/attachments/6/6026-2956e714be463ecf0f7ef0a25859b66e.jpg "Screen Shot 2023-06-02 at 5.25.42 PM-min.png")](https://elitemacx86.com/attachments/screen-shot-2023-06-02-at-5-25-42-pm-min-png.6018/)  
6\. Once the download is completed, you'll see something like below:  
[![Screen Shot 2023-06-02 at 5.26.01 PM-min.png](https://elitemacx86.com/data/attachments/6/6027-663d0abe6aa94f6fb0f777b1a8f720ca.jpg "Screen Shot 2023-06-02 at 5.26.01 PM-min.png")](https://elitemacx86.com/attachments/screen-shot-2023-06-02-at-5-26-01-pm-min-png.6019/)  
This will create a `com.apple.recovery.boot` directory inside `OpenCore-0.X.X-RELEASE/Utilities/macrecovery` directory.  
[![Screen Shot 2023-06-02 at 5.26.32 PM-min.png](https://elitemacx86.com/data/attachments/6/6030-b578f6fcb0a8985460b649b9b03e3580.jpg "Screen Shot 2023-06-02 at 5.26.32 PM-min.png")](https://elitemacx86.com/attachments/screen-shot-2023-06-02-at-5-26-32-pm-min-png.6022/)  
You can find `BaseSystem.dmg` and `BaseSystem.chunklist` in `OpenCore-0.X.X-RELEASE/Utilities/macrecovery/com.apple.recovery.boot` directory.  
[![Screen Shot 2023-06-02 at 5.26.28 PM-min.png](https://elitemacx86.com/data/attachments/6/6031-b0d1fd143548526c73b0dcf18673c218.jpg "Screen Shot 2023-06-02 at 5.26.28 PM-min.png")](https://elitemacx86.com/attachments/screen-shot-2023-06-02-at-5-26-28-pm-min-png.6023/)  

**NOTE:**  

*   **Depending on the macOS version, the script will either download BaseSystem or RecoveryImage files.**
*   **Booting macOS Big Sur 11.3 and newer without mapping of the USB Ports will result in boot loop due to the broken XhciPort Limit. Therefore, it is recommended to install 11.2.3 or map your USB Ports first using Windows before installing macOS. Note that if you have already [**mapped your USB Ports**](https://elitemacx86.com/threads/how-to-map-your-usb-ports-on-macos.581/), you can boot macOS 11.3 and newer without such issues. See** [**Mapping USB Ports**](https://elitemacx86.com/threads/how-to-map-your-usb-ports-on-macos.581/) **for more information.**

  
**2\. Preparing Installer**

Once you have the Recovery image, you can create the installer. To prepare the installer, follow the steps below

1\. Insert your USB Flash Drive (less than 64GB ).  
2\. Open Disk Utility. The Disk Utility is located at `/Applications/Utilities/Disk Utility`  
3\. Click on `View` and then select `Show All Devices`.  
[![Screenshot 2023-06-01 at 10.35.30 AM-min.png](https://elitemacx86.com/data/attachments/6/6036-9b6922e9f11263ef499c56a401d83cab.jpg "Screenshot 2023-06-01 at 10.35.30 AM-min.png")](https://elitemacx86.com/attachments/screenshot-2023-06-01-at-10-35-30-am-min-png.6028/)  
4\. Select your target USB Flash Drive in the left pane and click on Erase button at the top and a popup will appear.  
5\. Use the following parameters to erase your drive.

**Name**: EFI  
**Format**: MS-DOS (FAT)  
**Scheme**: Master Boot Record

[![Screenshot 2023-05-19 at 7.36.27 AM-min.png](https://elitemacx86.com/data/attachments/5/5870-d11899f36813baa07a978db4168e35ca.jpg "Screenshot 2023-05-19 at 7.36.27 AM-min.png")](https://elitemacx86.com/attachments/screenshot-2023-05-19-at-7-36-27-am-min-png.5862/)  
6\. When done, click on Done and close Disk Utility.

[![Screenshot 2023-05-19 at 7.36.37 AM-min.png](https://elitemacx86.com/data/attachments/5/5871-7d792903f689897f1cc354d0c7844535.jpg "Screenshot 2023-05-19 at 7.36.37 AM-min.png")](https://elitemacx86.com/attachments/screenshot-2023-05-19-at-7-36-37-am-min-png.5863/)

7\. Copy the `com.apple.recovery.boot` folder downloaded in the above step to your USB Flash Drive in Finder. Please ensure that the directory contains `BaseSystem.chunklist` and `BaseSystem.dmg` files respectively.

**For Windows users**

* * *

  
  
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

**For Linux users**

* * *

  
  
**STEP 1: Downloading macOS**  
You'll need to have the exact Recovery image of the target OS you want to install. To download the recovery image, follow the steps below.

1\. Download OpenCore Pkg from the downloads section of this forum.  
2\. Extract the downloaded file  
3\. Move into the OpenCore-0.X.X-RELEASE/Utilities directory  
4\. Right click on macreceovery folder and select Copy

[![Screenshot from 2022-07-17 21-31-09-min.png](https://elitemacx86.com/data/attachments/4/4774-c4a4532ae614a1c60cf3c72987a188b7.jpg "Screenshot from 2022-07-17 21-31-09-min.png")](https://elitemacx86.com/attachments/screenshot-from-2022-07-17-21-31-09-min-png.4766/)  
5\. Open Terminal

[![Screenshot from 2022-07-17 21-32-11-min.png](https://elitemacx86.com/data/attachments/4/4775-66af1a9e9103512d0914bbecde95fcbd.jpg "Screenshot from 2022-07-17 21-32-11-min.png")](https://elitemacx86.com/attachments/screenshot-from-2022-07-17-21-32-11-min-png.4767/)

[![Screenshot from 2022-07-17 21-32-39-min.png](https://elitemacx86.com/data/attachments/4/4776-f22abe93744ae7451e335bf80d7d3e55.jpg "Screenshot from 2022-07-17 21-32-39-min.png")](https://elitemacx86.com/attachments/screenshot-from-2022-07-17-21-32-39-min-png.4768/)  
6\. Type cd and then paste the path you copied earlier in step 4 and then press enter key. The command would be the following

```
cd /home/tech/Downloads/OpenCore-0.X.X-RELEASE/utilities/macrecovery
```


  
[![Screenshot from 2022-07-17 21-33-56-min.png](https://elitemacx86.com/data/attachments/4/4777-72307478d0ee1a128345e83080406b36.jpg "Screenshot from 2022-07-17 21-33-56-min.png")](https://elitemacx86.com/attachments/screenshot-from-2022-07-17-21-33-56-min-png.4769/)

**NOTE:**  

*   Replace the X with the OpenCore version.
*   Replace Your User Name with your actual username

  
7\. Depending on the macOS version you need (See Recovery Table below), type the command.

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


  
[![Screenshot from 2022-07-17 21-34-53-min.png](https://elitemacx86.com/data/attachments/4/4778-0d60b3b47c33c8e32af392518b46a468.jpg "Screenshot from 2022-07-17 21-34-53-min.png")](https://elitemacx86.com/attachments/screenshot-from-2022-07-17-21-34-53-min-png.4770/)  
8\. Once the download is completed, you'll see something like below

[![Screenshot from 2022-07-17 23-09-16-min.png](https://elitemacx86.com/data/attachments/4/4779-cb516281b04b70a89c006e6684929b32.jpg "Screenshot from 2022-07-17 23-09-16-min.png")](https://elitemacx86.com/attachments/screenshot-from-2022-07-17-23-09-16-min-png.4771/)  
You can find `BaseSystem.dmg` and `BaseSystem.chunklist` in `OpenCore-0.X.X-RELEASE/Utilities/macrecovery` directory.

[![Screenshot from 2022-07-17 23-11-19-min.png](https://elitemacx86.com/data/attachments/4/4781-1d0acd97cf2d8ba573f94146dabded38.jpg "Screenshot from 2022-07-17 23-11-19-min.png")](https://elitemacx86.com/attachments/screenshot-from-2022-07-17-23-11-19-min-png.4773/)

**NOTE:**  

*   Depending on the macOS version, the script will either download BaseSystem or RecoveryImage files.

  
**STEP 2: Preparing Installer**  
Once you have the Recovery image, you can create the installer. To prepare the installer, follow the steps below

1\. Insert your USB Flash Drive (less than 64GB) into your Ubuntu Computer.  
2\. Open Disks  
[![Screenshot from 2022-07-18 03-57-51-min.png](https://elitemacx86.com/data/attachments/4/4782-82fb7d9134469c350c4509ac0ae375bb.jpg "Screenshot from 2022-07-18 03-57-51-min.png")](https://elitemacx86.com/attachments/screenshot-from-2022-07-18-03-57-51-min-png.4774/)  
**For Unallocated Disks**

1\. For USB Drives which is unallocated, click on the Settings button and then click on Next

[![Screenshot from 2022-07-18 03-58-01-min.png](https://elitemacx86.com/data/attachments/4/4783-a7ebb08e44344338132d76e556ea57fd.jpg "Screenshot from 2022-07-18 03-58-01-min.png")](https://elitemacx86.com/attachments/screenshot-from-2022-07-18-03-58-01-min-png.4775/)

2\. Under Volume Name, type EFI  
3\. Under Type, select For use with all systems and devices (FAT) and click on Create  
[![Screenshot from 2022-07-18 03-58-15-min.png](https://elitemacx86.com/data/attachments/4/4784-c3a8e6390d7ba81fe6c0f847484e07b5.jpg "Screenshot from 2022-07-18 03-58-15-min.png")](https://elitemacx86.com/attachments/screenshot-from-2022-07-18-03-58-15-min-png.4776/)  
4\. Once erased, you'll see the created partition in Disks  
[![Screenshot from 2022-07-18 03-58-56-min.png](https://elitemacx86.com/data/attachments/4/4785-98296748055e8f2f5be0abfb1c318217.jpg "Screenshot from 2022-07-18 03-58-56-min.png")](https://elitemacx86.com/attachments/screenshot-from-2022-07-18-03-58-56-min-png.4777/)  
5\. Click on the play button to mount the USB Flash Drive  
[![Screenshot from 2022-07-18 04-02-22-min.png](https://elitemacx86.com/data/attachments/4/4789-5ae9da88abc4aa943955601962ac5f00.jpg "Screenshot from 2022-07-18 04-02-22-min.png")](https://elitemacx86.com/attachments/screenshot-from-2022-07-18-04-02-22-min-png.4781/)

**For USB with existing Data**

1\. For USB Drives which is unallocated, click on the Settings button and then click on the Format button

[![Screenshot from 2022-07-18 04-00-56-min.png](https://elitemacx86.com/data/attachments/4/4787-6e8797eb0d24f5177e2d12dd2bfd9690.jpg "Screenshot from 2022-07-18 04-00-56-min.png")](https://elitemacx86.com/attachments/screenshot-from-2022-07-18-04-00-56-min-png.4779/)  
2\. Click on Format

[![Screenshot from 2022-07-18 04-01-16-min.png](https://elitemacx86.com/data/attachments/4/4788-c13a44cd7a519d32ebec7f6f372c53c0.jpg "Screenshot from 2022-07-18 04-01-16-min.png")](https://elitemacx86.com/attachments/screenshot-from-2022-07-18-04-01-16-min-png.4780/)

3\. Once erased, you'll see the created partition in Disks

[![Screenshot from 2022-07-18 03-58-56-min.png](https://elitemacx86.com/data/attachments/4/4785-98296748055e8f2f5be0abfb1c318217.jpg "Screenshot from 2022-07-18 03-58-56-min.png")](https://elitemacx86.com/attachments/screenshot-from-2022-07-18-03-58-56-min-png.4777/)  
4\. Click on the play button to mount the USB Flash Drive  
[![Screenshot from 2022-07-18 04-02-22-min.png](https://elitemacx86.com/data/attachments/4/4789-5ae9da88abc4aa943955601962ac5f00.jpg "Screenshot from 2022-07-18 04-02-22-min.png")](https://elitemacx86.com/attachments/screenshot-from-2022-07-18-04-02-22-min-png.4781/)  
Now open your USB Flash Drive in Explorer  
Create a folder named `com.apple.recovery.boot` in the root of the USB Flash Drive  
Copy `BaseSystem.dmg` and `BaseSystem.chunklist` downloaded above into `com.apple.recovery.boot` directory.

Last edited: Jun 13, 2024

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

*   [](https://elitemacx86.com/threads/how-to-boot-macos-installer-on-desktops-and-laptops-using-opencore-uefi-legacy-opencore-install-guide.950/post-7588)
*   [#3](https://elitemacx86.com/threads/how-to-boot-macos-installer-on-desktops-and-laptops-using-opencore-uefi-legacy-opencore-install-guide.950/post-7588)

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

Legacy picture

Last edited: Jul 14, 2023

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

*   [](https://elitemacx86.com/threads/how-to-boot-macos-installer-on-desktops-and-laptops-using-opencore-uefi-legacy-opencore-install-guide.950/post-7589)
*   [#4](https://elitemacx86.com/threads/how-to-boot-macos-installer-on-desktops-and-laptops-using-opencore-uefi-legacy-opencore-install-guide.950/post-7589)

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

**I. Intel Desktops**



* CPU Code Name: Penryn
  * EC: SSDT-EC.aml
  * AWAC: -
  * IMEI: -
  * NVRAM: -
  * USB: -
  * CPU: -
* CPU Code Name: Lynfield, Clarkdale
  * EC: SSDT-EC.aml
  * AWAC: -
  * IMEI: -
  * NVRAM: -
  * USB: -
  * CPU: -
* CPU Code Name: Sandy Bridge
  * EC: SSDT-EC.aml
  * AWAC: -
  * IMEI: SSDT-IMEI
  * NVRAM: -
  * USB: -
  * CPU: CPU-PM.aml (Post-install task)
* CPU Code Name: Ivy Bridge
  * EC: SSDT-EC.aml
  * AWAC: -
  * IMEI: SSDT-IMEI
  * NVRAM: -
  * USB: -
  * CPU: CPU-PM.aml (Post-install task)
* CPU Code Name: Haswell
  * EC: SSDT-EC.aml
  * AWAC: -
  * IMEI: -
  * NVRAM: -
  * USB: -
  * CPU: SSDT-PLUG.aml
* CPU Code Name: Broadwell
  * EC: SSDT-EC.aml
  * AWAC: -
  * IMEI: -
  * NVRAM: -
  * USB: -
  * CPU: SSDT-PLUG.aml
* CPU Code Name: Skylake
  * EC: SSDT-EC-USBX.aml
  * AWAC: -
  * IMEI: -
  * NVRAM: -
  * USB: -
  * CPU: SSDT-PLUG.aml
* CPU Code Name: Kaby Lake
  * EC: SSDT-EC-USBX.aml
  * AWAC: -
  * IMEI: -
  * NVRAM: -
  * USB: -
  * CPU: SSDT-PLUG.aml
* CPU Code Name: Coffee Lake
  * EC: SSDT-EC-USBX.aml
  * AWAC: SSDT-AWAC-DISABLE.aml
  * IMEI: -
  * NVRAM: SSDT-PMC.aml
  * USB: -
  * CPU: SSDT-PLUG.aml
* CPU Code Name: Comet Lake
  * EC: SSDT-EC-USBX.aml
  * AWAC: SSDT-AWAC-DISABLE.aml
  * IMEI: -
  * NVRAM: -
  * USB: SSDT-RHUB-Reset.aml
  * CPU: SSDT-PLUG.aml
* CPU Code Name: Rocket Lake
  * EC: SSDT-EC-USBX.aml
  * AWAC: SSDT-AWAC-DISABLE.aml
  * IMEI: -
  * NVRAM: -
  * USB: -
  * CPU: SSDT-PLUG-ALT.aml
* CPU Code Name: Alder Lake
  * EC: SSDT-EC-USBX.aml
  * AWAC: SSDT-AWAC-DISABLE.aml
  * IMEI: -
  * NVRAM: -
  * USB: -
  * CPU: SSDT-PLUG-ALT.aml
* CPU Code Name: Raptor Lake
  * EC: SSDT-EC-USBX.aml
  * AWAC: SSDT-AWAC-DISABLE.aml
  * IMEI: -
  * NVRAM: -
  * USB: -
  * CPU: SSDT-PLUG-ALT.aml


**II. AMD Desktops**


|CPU Code Name                             |EC                      |AWAC|IMEI|NVRAM|USB|CPU          |
|------------------------------------------|------------------------|----|----|-----|---|-------------|
|Bulldozer/Piledriver/Steamroller/Excavator|SSDT-EC-USBX-DESKTOP.aml|-   |-   |-    |-  |-            |
|Jaguar/Puma                               |SSDT-EC-USBX-DESKTOP.aml|-   |-   |-    |-  |-            |
|Zen/Zen+/Zen2                             |SSDT-EC-USBX-DESKTOP.aml|-   |-   |-    |-  |SSDT-CPUR.aml|
|Zen3/Zen3+/Zen4                           |SSDT-EC-USBX-DESKTOP.aml|-   |-   |-    |-  |SSDT-CPUR.aml|


**II. Intel HEDT**


|CPU Code Name       |EC              |RTC                |PCI         |CPU            |
|--------------------|----------------|-------------------|------------|---------------|
|Nehalem and Westmere|SSDT-EC.aml     |-                  |-           |SSDT-CPU-PM.aml|
|Sandy Bridge-E      |SSDT-EC.aml     |-                  |SSDT-UNC.aml|SSDT-CPU-PM.aml|
|Ivy Bridge-E        |SSDT-EC.aml     |-                  |SSDT-UNC.aml|SSDT-CPU-PM.aml|
|Haswell-E           |SSDT-EC-USBX.aml|SSDT-RTC0-RANGE.aml|SSDT-UNC.aml|SSDT-PLUG.aml  |
|Broadwell-E         |SSDT-EC-USBX.aml|SSDT-RTC0-RANGE.aml|SSDT-UNC.aml|SSDT-PLUG.aml  |
|Skylake-X/W         |SSDT-EC-USBX.aml|SSDT-RTC0-RANGE.aml|-           |SSDT-PLUG.aml  |


**III. Intel Laptops**



* CPU Code Name: Clarksfield, Arrandale
  * EC: SSDT-EC.aml
  * AWAC: -
  * IMEI: -
  * NVRAM: -
  * USB: -
  * CPU: -
  * Backlight: SSDT-PNLF.aml
* CPU Code Name: Sandy Bridge
  * EC: SSDT-EC.aml
  * AWAC: -
  * IMEI: SSDT-IMEI
  * NVRAM: -
  * USB: -
  * CPU: CPU-PM.aml
  * Backlight: SSDT-PNLF.aml
* CPU Code Name: Ivy Bridge
  * EC: SSDT-EC.aml
  * AWAC: -
  * IMEI: SSDT-IMEI
  * NVRAM: -
  * USB: -
  * CPU: CPU-PM.aml
  * Backlight: SSDT-PNLF.aml
* CPU Code Name: Haswell
  * EC: SSDT-EC.aml
  * AWAC: -
  * IMEI: 
  * NVRAM: -
  * USB: -
  * CPU: SSDT-PLUG.aml
  * Backlight: SSDT-PNLF.aml
* CPU Code Name: Broadwell
  * EC: SSDT-EC.aml
  * AWAC: -
  * IMEI: 
  * NVRAM: -
  * USB: -
  * CPU: SSDT-PLUG.aml
  * Backlight: SSDT-PNLF.aml
* CPU Code Name: Skylake
  * EC: SSDT-EC-USBX.aml
  * AWAC: -
  * IMEI: 
  * NVRAM: -
  * USB: -
  * CPU: SSDT-PLUG.aml
  * Backlight: SSDT-PNLF.aml
* CPU Code Name: Kaby Lake/Kaby Lake-R/Amber Lake
  * EC: SSDT-EC-USBX.aml
  * AWAC: -
  * IMEI: 
  * NVRAM: -
  * USB: -
  * CPU: SSDT-PLUG.aml
  * Backlight: SSDT-PNLF.aml
* CPU Code Name: Coffee Lake/Whiskey Lake
  * EC: SSDT-EC-USBX.aml
  * AWAC: SSDT-AWAC-DISABLE.aml
  * IMEI: 
  * NVRAM: SSDT-PMC.aml
  * USB: -
  * CPU: SSDT-PLUG.aml
  * Backlight: SSDT-PNLF.aml
* CPU Code Name: Comet Lake
  * EC: SSDT-EC-USBX.aml
  * AWAC: SSDT-AWAC-DISABLE.aml
  * IMEI: 
  * NVRAM: -
  * USB: -
  * CPU: SSDT-PLUG.aml
  * Backlight: SSDT-PNLF.aml
* CPU Code Name: Ice Lake
  * EC: SSDT-EC-USBX.aml
  * AWAC: SSDT-AWAC-DISABLE.aml
  * IMEI: 
  * NVRAM: -
  * USB: SSDT-RHUB-Reset.aml
  * CPU: SSDT-PLUG.aml
  * Backlight: SSDT-PNLF.aml


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
