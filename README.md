# Ryzentosh Sonoma on AMD Ryzen 9 7900 + MSI X670E Tomahawk WiFi

#### MacOS version : Sonoma 14.7.2
#### SMBIOS : iMac20,2
#### Motherboard Bios version : 7E12v1F


## Specification
|  Component  | Model                               |
| ------------ |-------------------------------------|
| CPU  | AMD Ryzen 9 7900                   |
| Motherboard | MSI X670E Tomahawk Wifi          |
| RAM  | Kingston FURY Beast Kit 32GB (2x16GB) DDR5 6000MHz     |
|  GPU  | XFX Radeon RX480 |
| Ethernet  | Realtek RTL8125                     |
| Bluetooth  | ASUS BT-500                     |

## ACPI
- SSDT-CPUR.aml
- SSDT-EC-USBX-DESKTOP.aml
- SSDT-I225V.aml
- SSDT-PLUG_RYZEN.aml


## Kext
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

## BIOS settings
After reset BIOS to "default optimized" I've changed the following settings:
- iGPU - Disabled  
- IOMMU - Auto
- 4G - Enabled
- Legacy USB - Enabled 

## Not Tested
- USB ports - I have not test all ports

## Not Working
- WiFi 6: MT7922 is not supported

## Invaluable tools & links:
- Dortania Guide - [link-guide](https://dortania.github.io/OpenCore-Install-Guide/)
- OCAuxiliaryTools - [link-OCAT](https://github.com/ic005k/OCAuxiliaryTools)
- ProperTree - [link-ProperTree](https://github.com/corpnewt/ProperTree)
- GenSMBIOS - [link-genSMBIOS](https://github.com/corpnewt/GenSMBIOS)
- USBToolBox - [link-USBToolBox](https://github.com/USBToolBox/tool)
- [AMD-OSX forum](https://forum.amd-osx.com/)
- [macOS86 forum](https://macos86.it/)