# Ryzentosh on AMD Ryzen 9 7900 + MSI X670E Tomahawk WiFi

> MacOS version : Sonoma 14.7.2
>
> SMBIOS : iMac20,2
>
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
Check how many cores have your CPU and apply the patch *(from Dortania Guide)* then modify the number of cores in all four lines in `kernel> patch`. You just need to add the number in `HEX` format:

- `B8000000 0000 => B8 <core count> 0000 0000` 
- `BA000000 0000 => BA <core count> 0000 0000` 
- `BA000000 0090 => BA <core count> 0000 0090` 
- `BA000000 00 => BA <core count> 0000 00` 

Check SMBIOS settings and edit ROM value in **PlatformInfo**. Compare your file with my configuration file - which options are true or false /checked or unchecked/ and do the proper action. Also, consult with your notes from Dortania Guide.

When you are ready - reboot your computer, cross your finger, took a sip of beer, and boot from your USB drive.
If you find any troubles, you have to google for solutions - check the forums, links are below. If everything goes well you should see the macOS recovery window. Proceed with disk formatting, grab a beer, and then start installation of the Mac OS. The last part takes three beers.

**Cheers!**

## Add virtualization support - VirtualBox

**Notes**
Thank you for people that discover it, I copied it from [here](https://macos86.it/topic/6535-guidevirtualbox-on-sonoma-and-amd-hackintosh/)

The key program is Virtualbox 6.1.50 and related USB 3 extensions, where the code to support AMD-V/SVM are still present. **VirtualBox 7** is not ok because it only works with VT-X/VT-D owned only by Intel CPUs now, nobody created a wrapper yet.

1. In config.plist (see image) inside your boot EFI, Add the numbers csr-active-config = 03080000 (see image)  into the NVRAM section of config.plist (to workaround security & Privacy Oracle Box kext sign problems with this partial SIP disable).
2.  Add AMFIPass.kext details (see image) into kernel section of config.plist.  Save config.plist back to the EFI /OC folder.
3. Download version 1.40 of AMFIPass.kext and copy it over onto your EFI drive's Kext folder.
4. Ensure you can reset NVRAM in Opencore when rebooting
5. Reboot, and hit space, number to reset your NVRAM.
6. Now it should boot with your SIP disabled and use AMFIpass to allow the box kernel thru.
7. Download & install Vbox v.6.1.48 or 6.1.50
8. During the installation process it should ask you to sign / allow the Oracle Virtual Box / ALLOW it inside your 'System Settings' Security & privacy section.
9. After install and the kernel of box has been signed it will ask to reboot. Go ahead and reboot.
10. After reboot, locate and download/install Oracle_VM_VirtualBox_Extension_Pack-6.1.50.vbox-extpack (if using ver 6.1.50)
11. Now grab a ISO from Microsoft and install your guest OS - in my case it was Windows 10 32 bit iso
12. Go thru the motions of installing the guest OS like you normally would.
13. Locate and download VBoxGuestAdditions_6.1.50.iso or VBoxGuestAdditions_6.1.48.iso and install them from a guest ISO storage drive.This will provide a number of addition hardware features and functionality.
14. Finally, fine tune your guest OS for display USB pointing devices etc...
15. If you get problems running virtual box - quit Vbox completely (stop any guest VMs running first if you can) and then rerun it by running it from within Applications folder and not as an alias as sometimes aliases get corrupted or lose sight of the software.
16. I tried then resetting SIP to Enabled and disabling AMFIPass kext inside config plist Kernel section, reboot, resetting NVRAM and booting up to Sonoma but the Kernel errors reappeared and I found they were no longer 'Allowed' by the OS. Subsequently, virtual box guest VM's failed to start. 😞
17. So went back to partially Disabling SIP (csr-active-config = 03080000) but didn't require AMFIPass to ON now that Ive installed vBox. 


## Invaluable tools & links:
- Dortania Guide - [link-guide](https://dortania.github.io/OpenCore-Install-Guide/)
- OCAuxiliaryTools - [link-OCAT](https://github.com/ic005k/OCAuxiliaryTools)
- ProperTree - [link-ProperTree](https://github.com/corpnewt/ProperTree)
- GenSMBIOS - [link-genSMBIOS](https://github.com/corpnewt/GenSMBIOS)
- USBToolBox - [link-USBToolBox](https://github.com/USBToolBox/tool)
- [AMD-OSX forum](https://forum.amd-osx.com/)
- [macOS86 forum](https://macos86.it/)