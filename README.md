# 5700X 6900XT B450 Ryzentosh
## Overview
Hi, this is the EFI file that is currently (mostly) working for me on *macOS 13.3.1*. This is the version i am using with this EFI.

I have stumbled across multiple repos containing several EFIs which somehow led me to my first (mostly) working hackintosh so i hope this will be able to somehow help someone with similar hardware by sharing what worked for me. 

There are two ways of using this repo:
- The easy way : download my config.plist and open it using [OpencoreAuxilaryTools](https://github.com/ic005k/OCAuxiliaryTools). This should set all settings correctly and download ACPIs, Kexts and Drivers automatically. From then you would just verify everything and generate your own serialnumber using the tools "generate" feature and you should be able to just get going.
- The hard way of gathering files and settings by yourself and following the Opencore guide. 

I would recommend you doing your own research and building your EFI by yourself according to the Dortania guide. When you're done and it does not work, come back compare it with mine see check for differences and issues i had.

## Hardware
- Ryzen 7 5700X
- Asus TUF RX 6900XT
- MSI B450 Gaming Plus
- 4x8GB HyperX Fury RAM

## What works and what doesn't?
### Working
- iServices:
    - Facetime
    - Messages
    - Find my
    - iCloud
- CPU Power management, Fan control
- Audio over USB headset
### Not working
- Audio: Audio should work after patching it with AppleALC, but i use a USB headset so i did not want to mess with that. 
- Sleep: Sleep is a gamble. Sometimes it works and sometimes the PC crashes / turns off after entering sleep state.
- Airdrop, Handoff (Wifi and Bluetooth): I dont want to choke my graphics card but if you really need Wifi or Bluetooth buy one of the PCI-E cards mentioned on the [wireless buyer's guide](https://dortania.github.io/Wireless-Buyers-Guide/types-of-wireless-card/pcie.html)
- Discord only works in browser as the desktop version uses some libraries that only work on Intel (at least thats what i understood). Read more about this issue and how to fix it [here](https://www.macos86.it/topic/5489-tutorial-for-patching-binaries-for-amd-hackintosh-compatibility/)

# The EFI itself
## Gathering EFI Files

### Drivers
Download the latest Opencore Release, delete everything in /EFI/OC/Drivers except for:
- OpenCanopy.efi
- OpenHfsPlus.efi
- OpenRuntime.efi

### ACPI
- [SSDT-EC-USBX-DESKTOP.aml](https://github.com/dortania/Getting-Started-With-ACPI/blob/master/extra-files/compiled/SSDT-EC-USBX-DESKTOP.aml) 

### Kexts
Now this is the fun part. 

For 5700X, 6900XT
- [Lilu.kext](https://github.com/acidanthera/Lilu/releases)
- [VirtualSMC.kext](https://github.com/acidanthera/VirtualSMC/releases)
- From [SMCAMDProcessor](https://github.com/trulyspinach/SMCAMDProcessor) you will need both SMCAMDProcessor.kext and AMDRyzenCPUPowerManagement.kext
- [AppleMCEReporterDisable.kext](https://github.com/acidanthera/bugtracker/files/3703498/AppleMCEReporterDisabler.kext.zip)
- [RadeonSensor.kext](https://github.com/aluveitie/RadeonSensor)

For B450:
- [USBToolBox.kext](https://github.com/USBToolBox/kext)
- Make sure that if you use the USBToolBox.kext above, you also map your USBs using [USBToolBox Tool](https://github.com/USBToolBox/tool)
- [RealtekRTL8111.kext](https://github.com/Mieze/RTL8111_driver_for_OS_X/releases)

This should be all you need.

## Issues

### iServices not working?
Take a look at [this part of the guide](https://dortania.github.io/OpenCore-Post-Install/universal/iservices.html#fixing-en0).

### Blackscreen after lots of verbose?
Try removing the Whatevergreen kext, that fixed it for me since the 6900XT is natively supported. (God bless the guy who told me this in the AMD OSX discord)

### Some apps (mostly apps containing math related stuff) crash?
This might be the issue with the Intel libraries which was mentioned [here](https://www.macos86.it/topic/5489-tutorial-for-patching-binaries-for-amd-hackintosh-compatibility/). I had this problem with discord and it is possible to patch it but in my case i'll just open discord in the browser.

