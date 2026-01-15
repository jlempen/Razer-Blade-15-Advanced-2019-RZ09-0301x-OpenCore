![OpenCore logo](https://github.com/acidanthera/OpenCorePkg/raw/master/Docs/Logos/OpenCore_with_text_Small.png)

# Razer-Blade-15-Advanced-2019-RZ09-0301x-OpenCore
macOS on the Razer Blade 15 Advanced (2019) RZ09-0301x thanks to [Acidanthera's OpenCore bootloader](https://github.com/acidanthera/OpenCorePkg).

> [!WARNING]
> ### Installing or upgrading to macOS Tahoe
> 
> When  <ins>**upgrading**</ins> your existing installation to macOS Tahoe, you <ins>**MUST**</ins> log out of your `Apple Account` (iCloud) before proceeding with the upgrade. Furthermore make sure you <ins>**DESELECT**</ins> the option to enable `FileVault` disk encryption in the installer and do not sign into your `Apple Account` (iCloud) until the installer is done and you reach the desktop. Failing to do so will eventually encrypt your disk and prevent you from unlocking your disk with your password on restart.
>
> If `FileVault` disk encryption is enabled in your existing installation of macOS and you wish to keep it enabled in macOS Tahoe, then [carefully read the section below](https://github.com/jlempen/Razer-Blade-15-Advanced-2019-RZ09-0301x-OpenCore/tree/main#fixing-filevault-when-upgrading-to-macos-tahoe) before proceeding with the upgrade.
>
> When doing a <ins>**clean install**</ins> of macOS Tahoe, make sure you <ins>**DESELECT**</ins> the option to enable `FileVault` disk encryption in the installer and do not sign into your `Apple Account` (iCloud) until the installer is done and you reach the desktop. Failing to do so will eventually encrypt your disk and prevent you from unlocking your disk with your password on restart.
>
> If you wish to enable `FileVault` disk encryption in macOS Tahoe, [carefully read the section below](https://github.com/jlempen/Razer-Blade-15-Advanced-2019-RZ09-0301x-OpenCore/tree/main#fixing-filevault-when-upgrading-to-macos-tahoe).

## Latest News
* (20260114) Added the `apfs_aligned.efi` driver to fix `FileVault` when upgrading to macOS Tahoe ([see section below](https://github.com/jlempen/Razer-Blade-15-Advanced-2019-RZ09-0301x-OpenCore/tree/main#fixing-filevault-when-upgrading-to-macos-tahoe)).
* (20260114) Added resources and instructions to enable `AirportItlwm.kext` on macOS Sequoia and Tahoe ([see section below](https://github.com/jlempen/Razer-Blade-15-Advanced-2019-RZ09-0301x-OpenCore/tree/main?tab=readme-ov-file#enabling-the-intel-wireless-card-in-macos-sequoia-and-tahoe)).
* (20260114) Fixing audio in macOS Tahoe ([see section below](https://github.com/jlempen/Razer-Blade-15-Advanced-2019-RZ09-0301x-OpenCore/tree/main#fixing-audio-on-macos-tahoe)).
* (20260114) With the stuff merged today, macOS Tahoe 26.2 now installs and runs quite nicely.

## Software Specifications
| Software         | Version                            |
| ---------------- | ---------------------------------- |
| Target OS        | Apple macOS 13 Ventura, 14 Sonoma and 15 Sequoia |
| OpenCore         | [MOD-OC v1.0.7](https://github.com/wjz304/OpenCore_NO_ACPI_Build/releases/download/1.0.7_ffbd7f5/OpenCore-Mod-1.0.7-RELEASE.zip) |
| SMBIOS           | MacBookPro16,1 |
| UEFI Firmware    | [Modified firmware v1.05](https://github.com/jlempen/Razer-Blade-15-Advanced-2019-RZ09-0301x-OpenCore/tree/main/UEFI%20Firmware) |
| SSD format       | APFS file system, GPT partition table |

## Abstract
The Razer Blade 15 Advanced (2019) RZ09-0301x is a perfect Hackintosh laptop. Its design is reminiscent of the trusty old 2006 17-Inch MacBook Pro once you remove all the ugly Intel, Razer and NVIDIA stickers from the palmrest. It sounds great with its 2 large speakers, the huge trackpad supports all the native gestures and feels like an Apple trackpad, the keyboard with its sexy RGB lighting is quite serviceable, the Razer Blade sleeps and wakes quickly and the battery holds around five hours under normal load.

> [!IMPORTANT]
> For macOS to be able to boot on the Razer Blade, the `Secure Boot` option _**must be disabled**_ in the UEFI BIOS.

## Disclaimer
This repository is neither a howto nor an installation manual. Using these files requires at least basic knowledge of [Acidanthera's OpenCore bootloader](https://github.com/acidanthera/OpenCorePkg), ACPI, UEFI and the art of hackintoshing in general. I recommend reading the excellent [Dortania's OpenCore Install Guide](https://dortania.github.io/OpenCore-Install-Guide), as well as all its linked resources.

This EFI folder is based on [tylernguyen's archived repository](https://github.com/tylernguyen/razer15-hackintosh). The SSDTs required to reliably disable the dGPU even after standby/sleep were borrowed from [doubleyoustew](https://github.com/doubleyoustew/Razer-Blade-15/tree/master/Src/Hotpatches/dGPU) and the framebuffer patches needed to enable the 240Hz refresh rate were discovered by [reddit user sublime4116](https://www.reddit.com/r/hackintosh/comments/vz2lfq/comment/j223m0u/). 

## Recommendations
I recommend completely erasing the device's SSD by creating a new GPT partition table before attempting to install macOS, as it makes the installation process much easier. You may use any Linux live ISO with a partitioning tool such as `GParted` or `KPartition` to erase the SSD.

Furthermore, this EFI will only work with the modified firmware found in the [UEFI Firmware folder](https://github.com/jlempen/Razer-Blade-15-Advanced-2019-RZ09-0301x-OpenCore/tree/main/UEFI%20Firmware). Instructions on how to flash the modified firmware may be found [in this readme](https://github.com/jlempen/Razer-Blade-15-Advanced-2019-RZ09-0301x-OpenCore/blob/main/UEFI%20Firmware/flashing_firmware.md). macOS will only boot successfully after flashing the modified firmware and changing the required settings as described in [UEFI BIOS Configuration](https://github.com/jlempen/Razer-Blade-15-Advanced-2019-RZ09-0301x-OpenCore#uefi-bios-configuration).

Please be aware that all `PlatformInfo` and `SMBIOS` information was removed from the OpenCore `config.plist` files. Users will therefore need to generate their own `PlatformInfo` with [CorpNewt's GenSMBIOS tool](https://github.com/corpnewt/GenSMBIOS) before attempting to boot a Razer Blade 15 Advanced (2019) RZ09-0301x with this repository's EFI folder.

`AirportItlwm-Ventura.kext`, `AirportItlwm-Sonoma140.kext`, `AirportItlwm-Sonoma144.kext` and `AirportItlwm-Sequoia-Tahoe.kext` from the [OpenIntelWireless repo](https://github.com/OpenIntelWireless/itlwm) are drivers required to enable the Wifi chip. This EFI will dynamically load the appropriate kext for macOS Ventura, Sonoma, Sequoia or Tahoe depending on the running kernel. No need to manually replace the kext file when updating your version of macOS. 

In macOS Sequoia and Tahoe, you'll need to apply root patches with [Laobamac's OCLP-Mod Patcher](https://github.com/laobamac/OCLP-Mod/releases) once the OS is up and running in order to enable the Intel Wifi chip. [Head over to the instructions.](https://github.com/jlempen/Razer-Blade-15-Advanced-2019-RZ09-0301x-OpenCore/tree/main?tab=readme-ov-file#enabling-the-intel-wireless-card-in-macos-sequoia-and-tahoe).

the `Itlwm.kext` driver and its companion app [HeliPort](https://github.com/OpenIntelWireless/HeliPort/releases) are included but disabled in this EFI for those who prefer to connect to their Wifi network this way. You'll find the latest stable `HeliPort.dmg` in the [Tools folder](https://github.com/jlempen/Razer-Blade-15-Advanced-2019-RZ09-0301x-OpenCore/blob/main/Tools/HeliPort.dmg) of this repo.

This EFI folder also contains all the kexts needed to enable various Broadcom-based M.2 wireless cards. If you replaced the Intel wireless card with a Broadcom-based card such as the Dell DW1560, Dell DW1830, Dell DW1820A, Lenovo Lite-On WCBN802B, AzureWave AW-CB162NF, Lite-On WCBN808B or Lenovo Foxconn T77H649, simply disable all the Intel wireless and Bluetooth kexts and enable the Broadcom kexts instead. To do so, simply follow [my instructions below](https://github.com/jlempen/Razer-Blade-15-Advanced-2019-RZ09-0301x-OpenCore#replacing-the-intel-wireless-and-bluetooth-m2-card-with-a-broadcom-based-card).

This repository uses the unofficial [OpenCore_NO_ACPI_Build fork of OpenCore by btwise](https://gitee.com/btwise/OpenCore_NO_ACPI), wich is not endorsed by Acidanthera (the dev team behind OpenCore). The main (and only) difference between this fork and the official OpenCore version is that it allows to prevent ACPI injection (e.g. patches, tables, boot parameters) into other OSes besides macOS.

Windows and Linux should be detected automagically by the OpenCore boot loader even when installed after macOS.

<details>
  <summary>Computer Specifications</summary>
	
## Computer Specifications
| Device           | Hardware                           |
| ---------------- | ---------------------------------- |
| CPU              | Intel Core i7-9750H 6-Core (2.6 - 4.5 GHz, Coffee Lake Refresh) |
| Chipset          | Mobile Intel HM370 |
| iGPU             | Intel UHD Graphics 630 |
| dGPU             | NVIDIA GeForce RTX 2070 Max-Q with 8 GB of GDDR6 VRAM (disabled) |
| RAM              | Up to 64 GB dual-channel DDR4 2667 MHz (replaceable) |
| Audio            | Realtek ALC 298 |
| Wifi + Bluetooth | Intel Wireless AX200 and Bluetooth 5 (replaceable) |
| Storage          | Up to 2 TB SSD M.2 2280 NVMe PCIe 3.0 x4 |
| 1 x USB Type-C   | Does not support Power Delivery |
| 3 x USB-A 3.0    | USB 3.2 Gen 2 |
| Camera           | 1 MPix 720p HD camera |
| Keyboard / Trackpad | Per key RGB keyboard and precision glass touchpad |
| Display          | 240 Hz, 15.6 Inch Full HD, 1920 x 1080 matte screen |
</details>

<details>
  <summary>What works</summary>

## What works
- [x] CPU power management (`SSDT-PLUG-_SB.PR00.aml`)
- [x] CPU SpeedStep (`CPUFriend.kext`, `CPUFriendDataProvider.kext`)
- [x] iGPU with full acceleration (`WhateverGreen.kext` with patches)
- [x] SSD drive (`NVMeFix.kext`)
- [x] Sleep/hibernate and wake (`SSDT-PTSWAK.aml`, `SSDT-DDGPU.aml`, `SSDT-RTEC.aml`, `HibernationFixup.kext`)
- [x] USB-C port with hotplug (`SSDT-EC-USBX-LAPTOP.aml`, `USBMap.kext`)
- [x] USB 3.0 ports with hotplug (`SSDT-EC-USBX-LAPTOP.aml`, `USBMap.kext`)
- [x] WLAN (`AirportItlwm.kext`)
- [x] Bluetooth (`IntelBluetoothFirmware.kext`, `IntelBTPatcher.kext`, `BlueToolFixup.kext`)
- [x] Camera
- [x] Internal speakers, microphone and Combojack (`AppleALC.kext`, `alcid=3`, `SSDT-ALC298.aml`, `SSDT-ALC298a.aml`)
- [x] Keyboard with working brightness, volume and mute keys (internal USB header)
- [x] Configurable RGB color effects for the keyboard
- [x] Trackpad with native multi-touch gestures (`SSDT-I2CxConf.aml`, `SSDT-OCGPI0-GPHD.aml`, `SSDT-OCI2C-TPXX-RAZER15.aml`, `SSDT-XOSI.aml`, `VoodooI2C.kext`, `VoodooI2CHID.kext`)
- [x] Battery percentage and cycle count (`SSDT-BATT-razer15-late2019.aml`, `VirtualSMC.kext`, `SMCBatteryManager.kext`)
</details>

<details>
  <summary>What needs some more work</summary>
	
## What needs some more work
- [ ] Thunderbolt support and hotplug
</details>

<details>
  <summary>What will probably never work</summary>
	
## What will probably never work
- [ ] NVIDIA GeForce RTX 2070 Max-Q dGPU (disabled with an SSDT)
- [ ] HDMI and Mini DisplayPort outputs (hardwired to the NVIDIA dGPU)
- [ ] External Display output via USB-C (hardwired to the NVIDIA dGPU)
- [ ] Windows Hello IR camera
</details>

<details>
  <summary>Modified UEFI Firmware</summary>
	
## Modified UEFI Firmware
In order to take advantage of better CPU power management and graphics acceleration, there are a few settings that need to be unlocked and configured in the UEFI BIOS. The most convenient way to achieve this is to flash the modded firmware found in the [UEFI Firmware folder](https://github.com/jlempen/Razer-Blade-15-Advanced-2019-RZ09-0301x-OpenCore/tree/main/UEFI%20Firmware) and change the required settings in the UEFI BIOS as described below. Instructions on how to flash the modded firmware may be found [in this readme](https://github.com/jlempen/Razer-Blade-15-Advanced-2019-RZ09-0301x-OpenCore/blob/main/UEFI%20Firmware/flashing_firmware.md).

If you prefer modifying your own firmware, you may find very thorough instructions in [stonevil's excellent Razer Blade 15 Advanced early 2019 repository](https://github.com/stonevil/Razer_Blade_Advanced_early_2019_Hackintosh/#bios-unlock).
</details>

<details>
  <summary>UEFI BIOS Configuration</summary>
	
## UEFI BIOS Configuration
* Reboot computer
* Repeatedly press the ``F1`` or ``DEL`` key to enter the UEFI BIOS configuration menu
* In the UEFI BIOS navigate to the menu
	* ``Advanced``
		* ``Power & Performance``
			* ``CPU - Power Management Control``
				* ``CPU Lock Configuration``
					* Disable ``CFG Lock``
					* Disable ``Overclocking Lock``
	* ``Advanced``
		* ``Overclocking Performance Menu``
			* Disable ``XTU Interface``
	* ``Advanced``
		* ``Thunderbolt(TM) Configuration``
			* Switch ``Security Level`` to ``No Security``
	* ``Chipset``
		* ``System Agent (SA) Configuration``
			* ``Graphics Configuration``
				* Set ``DVMT Pre-Allocated`` to ``64``
				* Set ``DVMT Total Gfx Mem`` to ``MAX``
	* ``Chipset``
		* ``System Agent (SA) Configuration``
			* Disable ``VT-d``
	* ``Chipset``
		* ``SATA And RST Configuration``
			* Check ``SATA Mode Selection`` set to ``AHCI``
	* ``Security``
		* Set ``Secure Boot`` to ``Disabled``
	* ``Boot``
		* Set ``Fast Boot`` to ``Disabled``
		* ``CSM Configuration``
			* Set ``CSM Support`` to ``Disabled``
	* ``Save and Exit``
		* Hit ``Save Changes``
		* Hit ``Save Changes and Reset``
</details>

<details>
  <summary>Enabling native HiDPI display settings in macOS</summary>
	
## Enabling native HiDPI display settings in macOS
To enable native HiDPI settings in the Display Preferences of macOS, download and run the [one-key-hidpi](https://github.com/jlempen/one-key-hidpi) script and select the option `(1) 1920x1080 Display`.
</details>

<details>
  <summary>Replacing the Intel wireless and Bluetooth M.2 card with a Broadcom-based card</summary>
	
## Replacing the Intel wireless and Bluetooth M.2 card with a Broadcom-based card
Disable the following kexts in the `config.plist` file:
```
itlwm.kext
AirportItlwm-Sequoia-Sonoma.kext
AirportItlwm-Sonoma144.kext
AirportItlwm-Sonoma140.kext
AirportItlwm-Ventura.kext
IntelBluetoothFirmware.kext
IntelBTPatcher.kext
```
Enable the following kexts in the `config.plist` file:

```
AirportBrcmFixup.kext
AirportBrcmFixup.kext/Contents/PlugIns/AirPortBrcmNIC_Injector.kext
BrcmFirmwareData.kext
BrcmPatchRAM3.kext
```
To enable Broadcom wireless support in macOS Sonoma and Sequoia, enable the following kexts as well:

```
AMFIPass.kext
IOSkywalkFamily.kext
IO80211FamilyLegacy.kext
IO80211FamilyLegacy.kext/Contents/PlugIns/AirPortBrcmNIC.kext
```

Then head over to `Kernel -> Block` in your `config.plist` file and enable the `com.apple.iokit.IOSkywalkFamily` item.

For the Broadcom wireless card to work in macOS Sonoma and Sequoia, you'll also need to download and install the latest version of the [OpenCore Legacy Patcher](https://github.com/dortania/OpenCore-Legacy-Patcher/releases). Then launch the patcher and run the `Post-install Root Patch` for modern wireless and reboot when instructed to do so. Your Broadcom wireless card should work now!

> [!IMPORTANT]
> This last step needs to be repeated after every macOS update!
</details>

<details>
  <summary>Enabling the Intel Wireless Card in macOS Sequoia and Tahoe</summary>
	
## Enabling the Intel Wireless Card in macOS Sequoia and Tahoe

### Using the AirportItlwm.kext driver and root patching
It is now possible to have Intel WiFi with native Airport features on macOS Sequoia and Tahoe by enabling the `AirportItlwm.kext` driver for macOS Ventura provided by [the OpenIntelWireless project](https://openintelwireless.github.io/). 

`AirportItlwm.kext` uses Apple's IO80211Family. It provides certain Airport features but lacks stability compared with `itlwm.kext` due to the ambiguity of reverse engineering.

To enable the `AirportItlwm.kext` driver, download and install the latest release of [Laobamac's OCLP-Mod Patcher](https://github.com/laobamac/OCLP-Mod/releases).

Launch the OCLP-Mod Patcher, this may take a few seconds. As of January 2026, the tool is only available in Chinese.

Now click on the upper right button to select the Root Patching option:
<img width="712" height="443" alt="Screenshot 2026-01-10 at 00 43 16" src="https://github.com/user-attachments/assets/3af5464a-836b-4f71-a0fc-8ac8bd4af304" />

Then click on the highlighted button or press Enter to start the patching process:
<img width="712" height="443" alt="Screenshot 2026-01-10 at 00 46 51" src="https://github.com/user-attachments/assets/843687df-655f-47ea-bfc8-5b3f06681756" />

Once the patching is done, click on the highlighted button or press Enter to close the tool and restart your computer. Your Intel wireless card should be working now.

> [!IMPORTANT]
> You'll need to repeat the above steps after every macOS update!

### Using the Itlwm.kext driver
`itlwm.kext` uses Apple's IOEthernet rather than IO80211. It is purely based on open-source resources, provides a stabler and faster performance, and the ability to unload on Kernels that use prelined kernel.

To enable Intel WiFi with the `itlwm.kext` driver, open the `Kernel -> Add` tab in your `config.plist` file.

Enable the following kext:
```
itlwm.kext
```

Then disable the following kexts:

```
IOSkywalkFamily.kext
IO80211FamilyLegacy.kext
IO80211FamilyLegacy.kext/Contents/PlugIns/AirPortBrcmNIC.kext
AMFIPass.kext
AirportItlwm-Sequoia-Tahoe.kext
```

Now head over to the `Kernel -> Block` tab and disable the `com.apple.iokit.IOSkywalkFamily` item.

Then head over to the `NVRAM` tab, select the `7C436110-AB2A-4BBB-A880-FE41995C9F82` UUID and remove the `-amfipassbeta` argument from the `boot-args` key.

Save and close the `config.plist` file and reboot your computer.

Download and install the latest `HeliPort` Intel WiFi client for `itlwm` from [the OpenIntelWireless project](https://openintelwireless.github.io/HeliPort/#download).

Add the `HeliPort` client to your login items and hide the macOS WiFi icon from the Menu Bar.
</details>

<details>
  <summary>Fixing audio on macOS Tahoe</summary>
	
## Fixing audio on macOS Tahoe

### By root patching
As Apple removed the `AppleHDA.kext` from macOS Tahoe, [Acidanthera's AppleALC.kext](https://github.com/acidanthera/AppleALC) won't work on macOS Tahoe out of the box and audio is broken. To fix this, simply run [Laobamac's OCLP-Mod Patcher](https://github.com/laobamac/OCLP-Mod/releases) a second time once you have [fixed the Intel wireless card](https://github.com/jlempen/Razer-Blade-15-Advanced-2019-RZ09-0301x-OpenCore/tree/main?tab=readme-ov-file#enabling-the-intel-wireless-card-in-macos-sequoia-and-tahoe) and Internet is working again. The reason for this is that the OCLP-Mod Patcher needs to download the Kernel Development Kit from Apple's servers in order to reinstall the `AppleHDA.kext` on your macOS Tahoe partition and to do so, it needs a working Internet connection. 

### By installing the VoodooHDA audio driver
Another way to get back the internal speakers and microphone on macOS Tahoe is to install [SergeySlice's VoodooHDA audio driver](https://github.com/CloverHackyColor/VoodooHDA) with [chris1111's convenient VoodooHDA-Tahoe installer](https://github.com/chris1111/VoodooHDA-Tahoe). This has the added benefit of fixing the loud click you hear from your speakers when playing sound for the first time after waking your SL3 from hibernation.

Before installing the VoodooHDA driver, you need to disable the `AppleALC.kext` driver in your `config.plist` file under `Kernel -> Add` and reboot your computer.

Then [grab the latest installer](https://github.com/jlempen/Razer-Blade-15-Advanced-2019-RZ09-0301x-OpenCore/blob/main/Tools/VoodooHDA-Tahoe.pkg) from the Tools folder in my repository, launch the installer and follow the instructions.

Once you're back in macOS Tahoe after a reboot, head over to `System Settings -> Sound -> Output & Input` and select the `Output` tab, then select `Speaker (Analog)` as your sound output device.
</details>

<details>
  <summary>Fixing FileVault when upgrading to macOS Tahoe</summary>
	
## Fixing FileVault when upgrading to macOS Tahoe
If `FileVault` disk encryption is enabled during the upgrade or install of macOS Tahoe, the `FileVault` login window will reject your password and the encrypted disk will remain locked after the first reboot. This is due to Tahoe's APFS filesystem driver not supporting the software `FileVault` disk encryption created with previous versions of macOS.

There are two solutions to this rather annoying issue:

### Enabling the older APFS driver from macOS Sequoia
Open the `UEFI -> Drivers` tab in your `config.plist` file and enable the `apfs_aligned.efi` driver.

Then head over to the `UEFI -> APFS` tab and disable the `EnableJumpstart` option.

Save and close the `config.plist` file and restart your computer. The `FileVault` login window should now accept your password and unlock the disk.

### Turning off FileVault in the macOS Recovery Console
Follow the nice instructions [on Jac Timms' website](https://www.ichi.co.uk/blog/removing-file-vault-from-internet-recovery-console).
</details>

<details>
  <summary>Fixing the Caps Lock LED</summary>
	
## Fixing the Caps Lock LED
The Caps Lock LED doesn't work out of the box. To fix it, download and install [Karabiner-Elements](https://karabiner-elements.pqrs.org). Then open the app's Settings and head over to the `Devices` tab and look for your keyboard. Enable the `Manipulate caps lock LED` option and you're done. Make sure Karabiner-Elements starts on system startup.
</details>

<details>
  <summary>Configuring the RGB color effects of the keyboard</summary>
	
## Configuring the RGB color effects of the keyboard
The RGB color effects of the keyboard may be managed and configured conveniently with the excellent [Razer macOS](https://github.com/stickoking/razer-macos) utility.
</details>

<details>
  <summary>Undervolting to reduce heat and improve performance</summary>
	
## Undervolting to reduce heat and improve performance
There are two undervolting tools available for macOS:
* [Volta](https://volta.garymathews.com)
* [VoltageShift](https://github.com/sicreative/VoltageShift)

The `VoltageShift.kext` is already included and enabled in this repository's `Kexts` folder. To be able to launch the `voltageshift` command line tool from anywhere, copy the [voltageshift executable](https://github.com/jlempen/Razer-Blade-15-Advanced-2019-RZ09-0301x-OpenCore/blob/main/Tools/VoltageShift/voltageshift) from the [Tools folder](https://github.com/jlempen/Razer-Blade-15-Advanced-2019-RZ09-0301x-OpenCore/tree/main/Tools) to your `/usr/local/bin` folder by entering `sudo cp voltageshift /usr/local/bin/` in the macOS terminal after navigating to the folder where you downloaded the `voltageshift` executable file.

Please refer to the instructions found in the [VoltageShift repository](https://github.com/sicreative/VoltageShift), as well as to the excellent howto found in [zearp's repository](https://github.com/zearp/Nucintosh#undervolting).

After running `voltageshift info`, it appears that Razer undervolted the Razer Blade 15 Advanced 2019 RZ09-0301x CPU and CPU cache by -102mV right from the factory! But perhaps it is possible to undervolt the machine even more. Every computer is different in this regard.

### Undervolting in the UEFI BIOS
While undervolting in macOS with `VoltageShift` works well, it is also possible to undervolt the Razer Blade directly in the UEFI BIOS. Undervolting in this way will permanently enable the voltage offsets to the computer itself and affect every operating system running on the computer.

The instructions below were borrowed from [stonevil's repository](https://github.com/stonevil/Razer_Blade_Advanced_early_2019_Hackintosh/#undervolting).

Our modified AMI BIOS provides a lot of different tools for undervolting and overclocking. The most interesting and easy to use are:
* ``Processor``
	* ``Core Voltage Offset``
* ``GT``
	* ``GT Voltage Offset``
	* ``GTU Voltage Offset``
* ``Uncore``
	* ``Uncore Voltage Offset``

`Processor` is obviously the CPU, `GT` and `GTU` represent the internal graphics card and `Uncore` is the CPU cache.

To apply an undervolting configuration:
* Reboot the computer
* Repeatedly press the ``F1`` or ``DEL`` key to enter the UEFI BIOS setup
* In the UEFI BIOS setup, navigate to the menu
	* ``Advanced``
		* ``Processor``
			* Set ``Core Voltage Offset`` to 100
			* Set ``Offset Prefix`` to ``-`` (!)
		* ``GT``
			* Set ``GT Voltage Offset`` to 100
			* Set ``Offset Prefix`` to ``-`` (!)
			* Set ``GTU Voltage Offset`` to 100
			* Set ``Offset Prefix`` to ``-`` (!)
		* ``Uncore``
			* Set ``Uncore Voltage Offset`` to 60
			* Set ``Offset Prefix`` to ``-`` (!)
		* ``Memory``
			* Set ``Memory Profile`` to the best profile for the installed memory. Usually it is something like ``XMP profile 1``
	* ``Save and Exit``
		* Hit ``Save Changes``
		* Hit ``Save Changes and Reset``
 
I would advise to test the values first in macOS with the `VoltageShift` tool and only set them in the UEFI BIOS when you found stable values.

* Boot into macOS or Windows
* Download and install the [Prime95](https://www.mersenne.org/download/) application
* Run ``Torture Test...`` from the ``Options`` menu for at least one hour
* If the system works reliably, repeat all the steps and incrementally increase the offsets by -5. It is recommended to use the same offsets for ``Processor`` and ``GT/GTU``. Repeat the ``Torture Test...``. If the system becomes unstable, freezes or reboots, revert back to the previous stable configuration.

| Setting | Start offset | Recommended step | My stable offset |
| ---: | ---: | ---: | ---: |
| ``Processor Core Voltage Offset`` | -100 mV | -5 mV | -140 mV |
| ``GT Core Voltage Offset`` | -100 mV | -5 mV | -140 mV |
| ``GTU Core Voltage Offset`` | -100 mV | -5 mV | -140 mV |
| ``Uncore Voltage Offset`` | -60 mV | -5 mV | -120 mV |

CPU limitations can be very different from one machine to another, so do not use my configuration blindly.
</details>

<details>
  <summary>Fixing broken Apple Messages and FaceTime on macOS Sonoma</summary>
	
## Fixing broken Apple Messages and FaceTime on macOS Sonoma
To fix issues with Apple Messages and FaceTime related to the [Intel Wireless driver](https://github.com/OpenIntelWireless/itlwm) on macOS Sonoma, disable all `AirportItlwm-***.kext` entries under `Kernel -> Add` in your `config.plist` file and use the [itlwm_v2.3.0_stable.kext.zip](https://github.com/OpenIntelWireless/itlwm/releases/download/v2.3.0/itlwm_v2.3.0_stable.kext.zip) and its companion app [HeliPort](https://github.com/OpenIntelWireless/HeliPort/releases/download/v1.5.0/HeliPort.dmg) instead.

The latest version 2.3.0 of `itlwm.kext` is already included in the Kext folder and `config.plist` file.
</details>

<details>
  <summary>Related repositories</summary>
	
## Related repositories
* https://github.com/tylernguyen/razer15-hackintosh
* https://github.com/stonevil/Razer_Blade_Advanced_early_2019_Hackintosh
* https://github.com/doubleyoustew/Razer-Blade-15
* https://github.com/wu-hongjun/Hackintosh-Razer-Blade-RZ09-0301
</details>
