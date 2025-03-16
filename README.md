# **ThinkPad X270 EFI - OpenCore for macOS Ventura**

EFI for Lenovo ThinkPad X270 with OpenCore bootloader, compatible with macOS Ventura.

Since the EFI folder from the source may not be up-to-date, I recommend downloading it from the Releases section instead.

## _Laptop Specifications_

| **Component**      | **Details**                       |
| ------------------ | --------------------------------- |
| **CPU**            | Intel i5-6300U                    |
| **iGPU**           | Intel HD Graphics 520             |
| **SSD**            | Samsung 870 EVO                   |
| **Wireless**       | Intel AC-8260ngw                  |
| **SMBIOS**         | MacBookPro 14,1                   |
| **Bootloader**     | OpenCore 1.0.3                    |
| **macOS Version**  | macOS Ventura                     |

---

## _Intel Wi-Fi Support_

For Intel Wi-Fi functionality, you need to manually install either the **itlwm.kext** or **airportitlwm.kext** (but **do not load both kexts at the same time**).

This EFI configuration uses **itlwm**, so you must download **HeliPort.dmg** to manage the Wi-Fi connection.

**HeliPort** (Wi-Fi management tool): [GitHub - HeliPort](https://github.com/OpenIntelWireless/HeliPort)

---

## _Known Issues_

- **Touchpad Gestures**: The 3-finger gestures are quite buggy and may not work consistently due to poor hardware.
- **Touchpad Behavior**: The touchpad only works under polling mode, which might cause responsiveness issues. Need fix
- **Power Management & Sleep**: Due to the inability to unlock **CFG-Lock** on the X270, there are several bugs related to **AppleCpuPmCfgLock**, particularly with power management and sleep functions. (Some laptops from Lenovo such as T480, T490, does not requre AppleCpuPmCfgLock in order to work, you may try boot without it at first)

---

## _SMBIOS Configuration_

This EFI uses the **MacBookPro 14,1** SMBIOS, which is ideal for Intel Skylake and Kaby Lake CPUs.

### Create Your Own SMBIOS:
To generate or modify your **MacBookPro 14,1** SMBIOS (for Ventura), or switch to **MacBookPro 13,1** (for Monterey), you can use **GenSMBIOS**.

- [GenSMBIOS GitHub Repository](https://github.com/corpnewt/GenSMBIOS)

---

## _Creating a macOS Bootable USB_

Follow the **Dortania** guide to create a bootable macOS USB from either Windows or macOS:

- [Dortania OpenCore Install Guide](https://dortania.github.io/OpenCore-Install-Guide/installer-guide/)

### Steps:
1. **Download the EFI folder** from this guide and copy it to your USB drive.
2. Replace the **SMBIOS** parameters with your own values.
3. Follow the Dortania guide to ensure your USB is correctly prepared for macOS installation.

---

## _BIOS Settings for ThinkPad X270_

To ensure proper compatibility with macOS, adjust the following BIOS settings:

### **Enable**:
- **Intel VT-d**: This is necessary for macOS to run smoothly.

### **Disable**:
- **Secure Boot**: Disable this to avoid issues with OpenCore booting.
- **CSM Support (Compatibility Support Module)**: Disable to ensure that macOS boots in UEFI mode.

---

## _Post-Install Notes_

### Improving Battery Life & Sleep:
- **Power Management**: Given the CFG-Lock limitations, power management may not be optimal, but you can experiment with the following settings to improve sleep behavior and battery life:

  ```bash
  sudo pmset -a standby 0
  sudo pmset -a autopoweroff 0
  sudo pmset -a powernap 0
  sudo pmset -a proximitywake 0
  sudo pmset -a tcpkeepalive 0
  ```
  
### Combojack install: 
- Open the terminal and run the install.sh script from the TOOLS Combojack/ComboJack_Installer.zip directory to enable the headphone jack functionality. After rebooting, the jack should appear as a usable device.
---

## (Experimental) Enabling Hibernation (S4) function

To enable **S4 Hibernation (Write-to-Disk)**, follow these steps:

1. Set `ThirdpartyDrives` to `true` in your `config.plist`. (Required only when using an HDD or SATA SSD)
2. Set `Hibernatemode` to `Auto` in the `config.plist`.
3. Open the terminal and run:  
   ```
   sudo pmset -a hibernatemode 3
   sudo pmset -a standby 1
   ```
4. Change the Standby delay Low/High values as needed *Only if desired.
   ```
   sudo pmset -a standbydelaylow 10800
   sudo pmset -a standbydelayhigh 86400
   ```

If the hibernation function is still not working,

4. Put `Hibernationfixup.kext` to your EFI and add `hbfx-ahbm=37` to your boot-args [Hibernationfixup](https://github.com/acidanthera/HibernationFixup)
5. Put `RTCMemoryFixup.kext` to your EFI and add `rtcfx_exclude=0x80-0xAB,0xB0-0xB4` to your boot-args [RTCMemoryFixup](https://github.com/acidanthera/RTCMemoryFixup)

This will put your laptop into sleep mode (S3) initially, and then transition to hibernation (S4) after a set period of time.
