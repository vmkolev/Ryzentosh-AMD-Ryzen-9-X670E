# Ryzentosh on AMD Ryzen 9 7900 + MSI X670E Tomahawk WiFi

> MacOS version : Sonoma 14.7.2
> SMBIOS : iMac20,2
> Motherboard Bios version : 7E12v1F


### Specification
|  Component  | Model                               |
| ------------ |-------------------------------------|
| CPU  | AMD Ryzen 9 7900                   |
| Motherboard | MSI X670E Tomahawk Wifi          |
| RAM  | Kingston FURY Beast Kit 32GB (2x16GB) DDR5 6000MHz     |
|  GPU  | XFX Radeon RX480 |
| Ethernet  | Realtek RTL8125                     |
| Bluetooth  | ASUS BT-500                     |

### ACPI
- SSDT-CPUR.aml
- SSDT-EC-USBX-DESKTOP.aml
- SSDT-I225V.aml
- SSDT-PLUG_RYZEN.aml


### Kext
- AirportItlwm.kext
- AMDRyzenCPUPowerManagement.kext
- AppleALC.kext
- AppleIGC.kext
- AppleIntelI210Ethernet.kext
- AppleMCEReporterDisabler.kext
- BlueToolFixup.kext
- IntelBluetoothFirmware.kext
- IntelBTPatcher.kext
- Lilu.kext
- NVMeFix.kext
- RadeonSensor.kext
- RestrictEvents.kext
- SMCAMDProcessor.kext
- SMCRadeonGPU.kext
- USBMap.kext
- USBToolBox.kext
- VirtualSMC.kext
- WhateverGreen.kext
- LucyRTL8125Ethernet.kext

### BIOS settings
After reset BIOS to "default optimized" I've changed the following settings:
- iGPU - Disabled  
- IOMMU - Auto
- 4G - Enabled
- Legacy USB - Enabled 

### Not Tested
- USB ports - I have not test all ports

### Not Working
- WiFi 6: MT7922 is not supported

## Installation
Okay, if you want macOS on your computer, then you need to grab the following things: a USB drive at least 16Gb, fast Internet, a large stack of beer, and a lot of patience.

First step is put all beer in the fridge and then download OpenCore [package](https://github.com/acidanthera/opencorepkg/releases).
There are a lot of videos on YouTube how to generate a new `config.plist`. The simpliest way is to copy the Sample.plist file from `..\OpenCore-1.0.3-RELEASE\Docs` in `..\OpenCore-1.0.3-RELEASE\X64\EFI\OC\` and change the name.

The next step is to download recovery image of mac os you want. You could use `macrecovery` tool located at OpenCore folder:
```sh 
..\OpenCore-1.0.3-RELEASE\Utilities\macrecovery
```
There is a catch - you need Python3 to run it. The script check's if it is installed or not. 
The links are located in the file `recovery_urls.txt` in the same folder.
For example if you want mac OS Sonoma the command should looks like:
```sh
macrecovery.py -b Mac-827FAC58A8FDFA22 -m 00000000000000000 download
```
Meanwhile you could prepare your USB drive or take a beer. If you are using Windows download Rufus and format your USB drive with following settings:
- Boot selection -> not bootable
- File system -> FAT32

When it is formatted you can copy `EFI` folder from `..\OpenCore-1.0.3-RELEASE\X64` onto your USB drive.
Once you have the recovery image on your computer copy folder named: `com.apple.recovery.boot` also onto your USB drive.

Next, take another beer and start collecting all needed information and configuration about your system:
- USB ports - use USBToolBox to create USBMap.kext */in some guides says that you need to map only 15 ports - personally I never check if it is true :) /*
- GenSMBIOS - generate information about your mac model - the value of your ROM should be your real MAC address in HEX format. Otherwise, some features in macOS will not appear or fail */according to some guides/*.
- Download all kext files for your system - read the [Dortania Guide](https://dortania.github.io/OpenCore-Install-Guide/) carefully and take notes what you need. *It will take three or more beers time.*

Now, put all your notes in `config.plist` There are more than one way to do but **never use text editor**. Both tools `OCAuxiliaryTools` and `ProperTree` are good choices. Check on YouTube how to use each of them. Personally I started with ProperTree but then I switched to OCAuxiliaryTools because I can update the kext files easily and check for misconfigurations.

If your choice is OCAT, when you start it it will check and add all files in OC, Driver, and Kext folders in config.plist in proper order. Save it and close it then open it again. It will recheck the configuration.
If you follow the instructions of other used tools all information about SMBIOS and USB ports should be already saved in config.plist file.

Grab a beer and go to next step - edit **Kernel**
Check how many cores have your CPU and apply the patch *(from Dortania Guide)* then modify the number of cores in all four lines in `kernel> patch`. You just need to add the number in `HEX` format `BX <phys core count> 0000 0000`.
Check SMBIOS settings and edit ROM value in **PlatformInfo**. Compare your file with my configuration file - which options are true or false /checked or unchecked/ and do the proper action. Also, consult with your notes from Dortania Guide.

When you are ready - reboot your computer, cross your finger, took a sip of beer, and boot from your USB drive.
If you find any troubles, you have to google for solutions - check the forums, links are below. If everything goes well you should see the macOS recovery window. Proceed with disk formatting, grab a beer, and then start installation of the Mac OS. The last part takes three beers.

**Cheers!**

## Invaluable tools & links:
- Dortania Guide - [link-guide](https://dortania.github.io/OpenCore-Install-Guide/)
- OCAuxiliaryTools - [link-OCAT](https://github.com/ic005k/OCAuxiliaryTools)
- ProperTree - [link-ProperTree](https://github.com/corpnewt/ProperTree)
- GenSMBIOS - [link-genSMBIOS](https://github.com/corpnewt/GenSMBIOS)
- USBToolBox - [link-USBToolBox](https://github.com/USBToolBox/tool)
- [AMD-OSX forum](https://forum.amd-osx.com/)
- [macOS86 forum](https://macos86.it/)