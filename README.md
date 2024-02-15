# Thinkpad-X270-EFI
Lenovo Thinkpad X270 opencore EFI for Ventura


## Laptop Spec

| Component        | Details                            |
| ---------------- | ---------------------------------- |
| CPU              | Intel i5 - 6300u                   |
| dGPU             | Intel® HD Graphics 520             |
| SSD              | Samsung 870 evo                    |
| Wi-Fi            | Intel AC-8260                      |
| SmBios           | MacbookPro 14,1                    |
| BootLoader       | OpenCore 0.9.8                     |
| macOS            | Ventura                            |




## Intel WIFI/BT

You need to manually download and install either itlwm.kext or airportitlwm.kext (Do not load both kexts at once) to use intel WIFI,

Since this EFI uses itlwm, you need to download Heliport.dmg

itlwm / Airportitlwm : https://github.com/OpenIntelWireless/itlwm

Heliport : https://github.com/OpenIntelWireless/HeliPort


## Function / Bugs


### Functions:

Every function works except Apple airportcard required ones, such as sidecar and airdrop. 

You may swap your wifi card into natively supported devices BCM94360 or BCM94350 with OCLP patcher to work these functions properly. (If you can remove whitelist by bios modding)



### Bugs:

- 3 Finger gestures of Touchpad are quiet buggy
- Touchpad only work under polling mode
- Thinkpad X270 is impossible to unlock MSR 0xE2 register, therefore there are a lot of "AppleCpuPmCfgLock" related bugs


## SMBIOS

This hackintosh EFI uses MacbookPro 14,1 SMBIOS.

Create your own MacbookPro 14,1 SMBIOS with GenSMBIOS for ventura

or MacbookPro 13,1 for monterey.

https://github.com/corpnewt/GenSMBIOS



## MacOS bootable USB creation

- Read the Dortania guide for creating your USB from Windows or macOS
- [Guide Dortania](https://dortania.github.io/OpenCore-Install-Guide/installer-guide/) - USB creation
- Download EFI and copy it to your USB drive.



## Bios settings


### Disable : 

Secure boot
CSM Support

            
## Trials to fix touchpad gestures.

X270 uses i2c synaptic touchpad, so you may use voodooi2c.kexts with one of three satelite.kexts

This is what I figured out :

- Voodooi2c + VoodooI2CHID : Gives the best touchpad gestures for this X270

- Voodooi2c + VoodooRMI : Although a lot of synaptic machines decided to use VoodooRMI kext due to great functionality, it gives mediocre gesture function performance for this X270.

- Voodooi2c + VoodooI2CSynaptics : Unable to load VoodooI2CSynaptics.kext, thus the worst touchpad gesture and functionality for this X270.

You should try one of these combination to achieve best quality of touchpad gestures.
