## Flashing firmware on the Razer Blade 15 Advanced 2019 RZ09-0301x

On Windows, you may use the official [AFUWINGUIx64.exe tool](https://github.com/jlempen/Razer-Blade-15-Advanced-2019-RZ09-0301x-OpenCore/tree/main/Tools/AfuWin64) provided by the [UEFI BIOS manufacturer AMI](https://www.ami.com/bios-uefi-utilities/) to flash a firmware file. However, as of 2023, the latest Windows versions seem to kernel panic when launching the tool due to driver verification issues.

It is therefore recommended to use the official [AFUEfix64.exe tool](https://github.com/jlempen/Razer-Blade-15-Advanced-2019-RZ09-0301x-OpenCore/tree/main/Tools/AfuEfi64) to flash the firmware directly in an EFI Shell environment by following the instructions below.

### Preparation
* Copy the firmware file you want to flash and most importantly a copy of your ORIGINAL (stock) firmware file to the root of a FAT32 formatted USB stick.
* Copy the `AfuEfix64.efi` executable to the USB stick as well.

### Enter the EFI Shell
While booting you computer with OpenCore, press SPACE in the picker and select `EFI Shell`.

### Flashing the firmware
* Plug in your AC adapter and make sure your battery is at least 50% charged.
* First of all, if any step fails while flashing, DO NOT REBOOT!
* Type `map -r` to display a list of all the hardware, including internal and external drives.
* Find the `AfuEfix64.efi` executable by typing `fs0:` followed by `ls`, which lists all the files. If you copied the file into a subfolder, you'll need to navigate to this subfolder first with e.g. `cd “Program Files”`.
Once you found the drive containing `AfuEfix64.efi`, take note of the index of the drive.
* Find your firmware file using the same procedure and take note of the index of the drive.
* Verify the firmware file using the `/D` option by typing:
  `fs[index of your AfuEfix64.efi drive]:`. Example: `fs1:`
  `AfuEfix64.efi fs[index of your firmware drive]:\[your firmware name] /D`
  Example: `AfuEfix64.efi fs4:\unlocked.rom /D`
* If the checksum and the other tests are OK, you are ready to flash the firmware. Type:
  `fs[index of your AfuEfix64.efi drive]:`. Example: `fs1:`
  `AfuEfix64.efi fs[index of your firmware drive]:\[your firmware name] /P /B`
  Example: `AfuEfix64.efi fs4:\unlocked.rom /P /B`
* Now make sure all the tasks completed successfully. If not, DO NOT REBOOT!!!
  Repeat the entire procedure with the modified firmware file or with the original, stock firmware file.
* Once all the tastks completed successfully, type `exit` to return to the OpenCore picker and restart your computer.

Now you should see the unlocked settings in the UEFI BIOS.
