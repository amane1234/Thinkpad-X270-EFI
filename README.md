# **Thinkpad-X270-EFI**
Lenovo Thinkpad X270 opencore EFI for Ventura <br><br>


## _Laptop Spec_

| Component        | Details                            |
| ---------------- | ---------------------------------- |
| CPU              | Intel i5 - 6300u                   |
| iGPU             | Intel® HD Graphics 520             |
| SSD              | Samsung 870 evo                    |
| Wireless         | Intel AC-8260                      |
| SmBios           | MacbookPro 14,1                    |
| BootLoader       | OpenCore 0.9.8                     |
| macOS            | Ventura                            |

<br><br>


## _To use Intel WIFI_

You need to manually download and install either itlwm.kext or airportitlwm.kext (Do not load both kexts at once) to use intel WIFI,

Since this EFI uses itlwm, you need to download Heliport.dmg

itlwm / Airportitlwm : https://github.com/OpenIntelWireless/itlwm

Heliport : https://github.com/OpenIntelWireless/HeliPort
<br><br>


## _Functions / Bugs_


### Functions:

Every function works except Apple airportcard required ones, such as sidecar and airdrop. 

You may swap your wifi card into natively supported devices BCM94360 or BCM94350 with OCLP patcher to work these functions properly. (If you can remove whitelist by bios modding)<br>


### Bugs:

- 3 Finger gestures of Touchpad are quiet buggy
- Touchpad only works under polling mode
- Since Thinkpad X270 is impossible to unlock cfg-lock related register, there are a lot of "AppleCpuPmCfgLock" related bugs (Usually power managements and sleep-related-functions)
  <br><br>


## _SMBIOS_

This hackintosh EFI uses MacbookPro 14,1 SMBIOS.

Create your own MacbookPro 14,1 SMBIOS with GenSMBIOS for ventura or MacbookPro 13,1 for monterey.

https://github.com/corpnewt/GenSMBIOS
<br><br>


## _MacOS bootable USB creation_

- Read the Dortania guide for creating your USB from Windows or macOS
- [Guide Dortania](https://dortania.github.io/OpenCore-Install-Guide/installer-guide/) - USB creation
- Download EFI and copy it to your USB drive.
<br><br>


## _Bios settings_


### Enable : 

- Intel VT-d
  <br>

### Disable : 

- Secure boot

- CSM Support
<br><br>
            
## _Trials to fix touchpad gestures with touchpad buttons_

X270 uses SMBUS synaptic touchpad, so you may use voodooSMBUS.kexts with one of satelite.kexts

VoodooRMI : https://github.com/VoodooSMBus/VoodooRMI/releases


This is what I figured out :

- VoodooPS2 + VoodooSMBus + VoodooRMI(SMBUS plugin) : Gives the best touchpad gestures and responsiveness for this X270, but unable to use touchpad buttons.

- VoodooPS2 + VoodooGPIO.kext : Shows mediocre gesture functionality/responsiveness. Able to use both touchpad and touchpad buttons.
  

Plus, "SSDT-ThinkPad_ClickPad.aml" file also contributes touchpad functionality as well.

You may try to adjust parameter of "SSDT-ThinkPad_ClickPad.aml" or simply disable it to achieve better touchpad responsiveness and functionality.
