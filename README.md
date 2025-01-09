![OpenCore logo](https://github.com/acidanthera/OpenCorePkg/raw/master/Docs/Logos/OpenCore_with_text_Small.png)

# Razer-Blade-15-Advanced-2019-RZ09-0301x-OpenCore
macOS on the Razer Blade 15 Advanced (2019) RZ09-0301x thanks to [Acidanthera's OpenCore bootloader](https://github.com/acidanthera/OpenCorePkg).

The Razer Blade 15 Advanced (2019) RZ09-0301x is an almost perfect Hackintosh laptop. Its design is reminiscent of the trusty old 2006 17-Inch MacBook Pro once you remove all the ugly Intel, Razer and NVIDIA stickers from the palmrest. It sounds great with its 2 large speakers, the huge trackpad supports all the native gestures and feels like an Apple trackpad, the keyboard with its sexy RGB lighting is quite serviceable, the Razer Blade sleeps and wakes quickly and the battery holds around five hours under normal load.

> [!TIP]
> I recommend installing `macOS 13 Ventura` rather than the newer `macOS 14 Sonoma` or `macOS 15 Sequoia`. The Intel Wireless chips work almost perfectly with Apple's iServices and Continuity features on Ventura while those features are partially broken at the moment on newer versions of macOS. Moreover, `macOS 13 Ventura` is the last version of macOS to natively support Broadcom Wireless chips.

> [!IMPORTANT]
> For macOS to be able to boot on the Razer Blade, the `Secure Boot` option _**must be disabled**_ in the UEFI BIOS.

## Disclaimer
This repository is neither a howto nor an installation manual. Using these files requires at least basic knowledge of [Acidanthera's OpenCore bootloader](https://github.com/acidanthera/OpenCorePkg), ACPI, UEFI and the art of hackintoshing in general. I recommend reading the excellent [Dortania's OpenCore Install Guide](https://dortania.github.io/OpenCore-Install-Guide), as well as all its linked resources.

This EFI folder is based on [tylernguyen's archived repository](https://github.com/tylernguyen/razer15-hackintosh). The SSDTs required to reliably disable the dGPU even after standby/sleep were borrowed from [doubleyoustew](https://github.com/doubleyoustew/Razer-Blade-15/tree/master/Src/Hotpatches/dGPU) and the framebuffer patches needed to enable the 240Hz refresh rate were discovered by [reddit user sublime4116](https://www.reddit.com/r/hackintosh/comments/vz2lfq/comment/j223m0u/). 

## Recommendations
I recommend completely erasing the device's SSD by creating a new GPT partition table before attempting to install macOS, as it makes the installation process much easier. You may use any Linux live ISO with a partitioning tool such as `GParted` or `KPartition` to erase the SSD.

Furthermore, this EFI will only work with the modified firmware found in the [UEFI Firmware folder](https://github.com/jlempen/Razer-Blade-15-Advanced-2019-RZ09-0301x-OpenCore/tree/main/UEFI%20Firmware). Instructions on how to flash the modified firmware may be found [in this readme](https://github.com/jlempen/Razer-Blade-15-Advanced-2019-RZ09-0301x-OpenCore/blob/main/UEFI%20Firmware/flashing_firmware.md). macOS will only boot successfully after flashing the modified firmware and changing the required settings as described in [UEFI BIOS Configuration](https://github.com/jlempen/Razer-Blade-15-Advanced-2019-RZ09-0301x-OpenCore#uefi-bios-configuration).

Please be aware that all `PlatformInfo` and `SMBIOS` information was removed from the OpenCore `config.plist` files. Users will therefore need to generate their own `PlatformInfo` with [CorpNewt's GenSMBIOS tool](https://github.com/corpnewt/GenSMBIOS) before attempting to boot a Razer Blade 15 Advanced (2019) RZ09-0301x with this repository's EFI folder.

`AirportItlwm-Ventura.kext`, `AirportItlwm-Sonoma140.kext` and `AirportItlwm-Sonoma144.kext` from the [OpenIntelWireless repo](https://github.com/OpenIntelWireless/itlwm) are required to enable the Wifi chip. This EFI will dynamically load the appropriate kext for macOS Ventura or Sonoma depending on the running kernel. No need to manually replace the kext file when updating your version of macOS. As the Intel Wifi chip does not yet work with the `AirportItlwm.kext` in macOS Sequoia, you'll need to use the `Itlwm.kext` and its companion app [HeliPort](https://github.com/OpenIntelWireless/HeliPort/releases) to connect to a Wifi network. You'll find the latest stable `HeliPort.dmg` in the [Tools folder](https://github.com/jlempen/Surface-Laptop-3-OpenCore/blob/main/Tools/HeliPort.dmg) of this repo. This EFI will dynamically load the `Itlwm.kext` instead of `AirportItlwm.kext` when you boot into macOS Sequoia.

This EFI folder also contains all the kexts needed to enable various Broadcom-based M.2 wireless cards. If you replaced the Intel wireless card with a Broadcom-based card such as the Dell DW1560, Dell DW1830, Dell DW1820A, Lenovo Lite-On WCBN802B, AzureWave AW-CB162NF, Lite-On WCBN808B or Lenovo Foxconn T77H649, simply disable all the Intel wireless and Bluetooth kexts and enable the Broadcom kexts instead. To do so, simply follow [my instructions below](Razer-Blade-15-Advanced-2019-RZ09-0301x-OpenCore#enabling-native-hidpi-display-settings-in-macos).

This repository uses the unofficial OpenCore_NO_ACPI_Build fork of OpenCore by [btwise](https://gitee.com/btwise/OpenCore_NO_ACPI), wich is not endorsed by Acidanthera (the dev team behind OpenCore). The main (and only) difference between this fork and the official OpenCore version is that it allows to prevent ACPI injection (e.g. patches, tables, boot parameters) into other OSes besides macOS.

Windows and Linux should be detected automagically by the OpenCore boot loader even when installed after macOS.

## Software Specifications
| Software         | Version                            |
| ---------------- | ---------------------------------- |
| Target OS        | Apple macOS 13 Ventura, 14 Sonoma and 15 Sequoia |
| OpenCore         | [MOD-OC v1.0.3](https://github.com/wjz304/OpenCore_NO_ACPI_Build/releases/download/1.0.3_20b758b/OpenCore-Mod-1.0.3-RELEASE.zip) |
| SMBIOS           | MacBookPro16,1 |
| UEFI Firmware    | [Modified firmware v1.05](https://github.com/jlempen/Razer-Blade-15-Advanced-2019-RZ09-0301x-OpenCore/tree/main/UEFI%20Firmware) |
| SSD format       | APFS file system, GPT partition table |

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

## What needs some more work
- [ ] Thunderbolt support and hotplug

## What will probably never work
- [ ] NVIDIA GeForce RTX 2070 Max-Q dGPU (disabled with an SSDT)
- [ ] HDMI and Mini DisplayPort outputs (hardwired to the NVIDIA dGPU)
- [ ] Windows Hello IR camera

## Modified UEFI Firmware
In order to take advantage of better CPU power management and graphics acceleration, there are a few settings that need to be unlocked and configured in the UEFI BIOS. The most convenient way to achieve this is to flash the modded firmware found in the [UEFI Firmware folder](https://github.com/jlempen/Razer-Blade-15-Advanced-2019-RZ09-0301x-OpenCore/tree/main/UEFI%20Firmware) and change the required settings in the UEFI BIOS as described below. Instructions on how to flash the modded firmware may be found [in this readme](https://github.com/jlempen/Razer-Blade-15-Advanced-2019-RZ09-0301x-OpenCore/blob/main/UEFI%20Firmware/flashing_firmware.md).

If you prefer modifying your own firmware, you may find very thorough instructions in [stonevil's excellent Razer Blade 15 Advanced early 2019 repository](https://github.com/stonevil/Razer_Blade_Advanced_early_2019_Hackintosh/#bios-unlock).

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

## Enabling native HiDPI display settings in macOS
To enable native HiDPI settings in the Display Preferences of macOS, download and run the [one-key-hidpi](https://github.com/jlempen/one-key-hidpi) script and select the option `(1) 1920x1080 Display`.

## Replacing the Intel wireless and Bluetooth M.2 card with a Broadcom-based card
Disable the following kexts in the `config.plist` file:
```
itlwm.kext
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

## Configuring the RGB color effects of the keyboard
The RGB color effects of the keyboard may be managed and configured conveniently with the excellent [Razer macOS](https://github.com/1kc/razer-macos) utility.

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

## Related repositories
* https://github.com/tylernguyen/razer15-hackintosh
* https://github.com/stonevil/Razer_Blade_Advanced_early_2019_Hackintosh
* https://github.com/doubleyoustew/Razer-Blade-15
* https://github.com/wu-hongjun/Hackintosh-Razer-Blade-RZ09-0301
