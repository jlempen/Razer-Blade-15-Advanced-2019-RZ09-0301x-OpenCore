![OpenCore logo](https://github.com/acidanthera/OpenCorePkg/raw/master/Docs/Logos/OpenCore_with_text_Small.png)

# Razer-Blade-15-Advanced-2019-RZ09-0301x-OpenCore
macOS on the Razer Blade 15 Advanced (2019) RZ09-0301x thanks to [Acidanthera's OpenCore bootloader](https://github.com/acidanthera/OpenCorePkg).

The Razer Blade 15 Advanced (2019) RZ09-0301x is an almost perfect Hackintosh laptop. Its design is reminiscent of the trusty old 2006 17-Inch MacBook Pro once you remove all the ugly Intel, Razer and NVIDIA stickers from the palmrest. It sounds great with its 2 large speakers, the huge trackpad supports all the native gestures and feels like an Apple trackpad, the keyboard with its sexy RGB lighting is quite serviceable, the Razer Blade sleeps and wakes quickly and the battery holds around five hours under normal load.

## Disclaimer
This repository is neither a howto nor an installation manual. Using these files requires at least basic knowledge of [Acidanthera's OpenCore bootloader](https://github.com/acidanthera/OpenCorePkg), ACPI, UEFI and the art of hackintoshing in general. I recommend reading the excellent [Dortania's OpenCore Install Guide](https://dortania.github.io/OpenCore-Install-Guide), as well as all its linked resources.

This EFI folder is based on [tylernguyen's archived repository](https://github.com/tylernguyen/razer15-hackintosh). The SSDTs required to reliably disable the dGPU even after standby/sleep were borrowed from [doubleyoustew](https://github.com/doubleyoustew/Razer-Blade-15/tree/master/Src/Hotpatches/dGPU) and the framebuffer patches needed to enable the 240Hz refresh rate were discovered by [reddit user sublime4116](https://www.reddit.com/r/hackintosh/comments/vz2lfq/comment/j223m0u/). 

## Recommendations
I recommend completely erasing the device's SSD by creating a new GPT partition table before attempting to install macOS, as it makes the installation process much easier. You may use any Linux live ISO with a partitioning tool such as `GParted` or `KPartition` to erase the SSD.

In order to boot macOS on the Razer Blade, the `Secure Boot` option needs to be _**disabled**_ in the BIOS.

Furthermore, this EFI will only work with the modified firmware found in the [UEFI Firmware folder](https://github.com/jlempen/Razer-Blade-15-Advanced-2019-RZ09-0301x-OpenCore/tree/main/UEFI%20Firmware). Instructions on how to flash the modified firmware may be found [in this readme](https://github.com/jlempen/Razer-Blade-15-Advanced-2019-RZ09-0301x-OpenCore/tree/main/UEFI%20Firmware). macOS will only boot successfully after flashing the modified firmware and changing the required settings as described in [UEFI BIOS Configuration](https://github.com/jlempen/Razer-Blade-15-Advanced-2019-RZ09-0301x-OpenCore#uefi-bios-configuration).

Please be aware that all `PlatformInfo` and `SMBIOS` information was removed from the OpenCore `config.plist` files. Users will therefore need to generate their own `PlatformInfo` with [CorpNewt's GenSMBIOS tool](https://github.com/corpnewt/GenSMBIOS) before attempting to boot a Razer Blade 15 Advanced (2019) RZ09-0301x with this repository's EFI folder.

## Software Specifications
| Software         | Version                            |
| ---------------- | ---------------------------------- |
| Target OS        | Apple macOS 13.3 Ventura |
| OpenCore         | [OC v0.9.7](https://github.com/acidanthera/OpenCorePkg/releases/download/0.9.7/OpenCore-0.9.7-RELEASE.zip) |
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
- [x] Trackpad with native multi-touch gestures (`SSDT-I2CxConf.aml`, `SSDT-OCGPI0-GPHD.aml`, `SSDT-OCI2C-TPXX-RAZER15.aml`, `SSDT-XOSI.aml`, `VoodooI2C.kext`, `VoodooI2CHID.kext`)
- [x] Battery percentage and cycle count (`SSDT-BATT-razer15-late2019.aml`, `VirtualSMC.kext`, `SMCBatteryManager.kext`)

## What needs some more work
- [ ] Thunderbolt support and hotplug

## What will probably never work
- [ ] NVIDIA GeForce RTX 2070 Max-Q dGPU (disabled with an SSDT)
- [ ] HDMI and Mini DisplayPort outputs (hardwired to the NVIDIA dGPU)
- [ ] Windows Hello IR camera

## Modified UEFI Firmware
In order to take advantage of better CPU power management and graphics acceleration, there are a few settings that need to be unlocked and configured in the UEFI BIOS. The most convenient way to achieve this is to flash the modded firmware found in the [UEFI Firmware folder](https://github.com/jlempen/Razer-Blade-15-Advanced-2019-RZ09-0301x-OpenCore/tree/main/UEFI%20Firmware) and change the required settings in the UEFI BIOS as described below. Instructions on how to flash the modded firmware may be found [in this readme](https://github.com/jlempen/Razer-Blade-15-Advanced-2019-RZ09-0301x-OpenCore/tree/main/UEFI%20Firmware).

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

## Related repositories
* https://github.com/tylernguyen/razer15-hackintosh
* https://github.com/stonevil/Razer_Blade_Advanced_early_2019_Hackintosh
* https://github.com/doubleyoustew/Razer-Blade-15
* https://github.com/wu-hongjun/Hackintosh-Razer-Blade-RZ09-0301
