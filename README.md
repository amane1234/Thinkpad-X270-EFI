# **ThinkPad X270 EFI - OpenCore for macOS Ventura**

EFI for Lenovo ThinkPad X270 with OpenCore bootloader, compatible with macOS Ventura.

---

## _Laptop Specifications_

| **Component**      | **Details**                       |
| ------------------ | --------------------------------- |
| **CPU**            | Intel i5-6300U                    |
| **iGPU**           | Intel HD Graphics 520             |
| **SSD**            | Samsung 870 EVO                   |
| **Wireless**       | Intel AC-8260                     |
| **SMBIOS**         | MacBookPro 14,1                   |
| **Bootloader**     | OpenCore 0.9.8                    |
| **macOS Version**  | macOS Ventura                     |

---

## _Intel Wi-Fi Support_

For Intel Wi-Fi functionality, you need to manually install either the **itlwm.kext** or **airportitlwm.kext** (but **do not load both kexts at the same time**).

This EFI configuration uses **itlwm**, so you must download **HeliPort.dmg** to manage the Wi-Fi connection.

### Required Kexts:
- **itlwm / airportitlwm**: [GitHub - itlwm](https://github.com/OpenIntelWireless/itlwm)
- **HeliPort** (Wi-Fi management tool): [GitHub - HeliPort](https://github.com/OpenIntelWireless/HeliPort)

---

## _System Functionality & Known Issues_

### **Working Features**:
- **Full system functionality**: Everything works as expected, except Apple-specific features like **Sidecar** and **AirDrop**, which require an Apple Airport Card.
  
- **Wi-Fi**: Works well with the **itlwm** kext and HeliPort.

### **Suggested Fixes**:
- **Wi-Fi Replacement**: If you want to use **Sidecar** or **AirDrop**, you may swap your current Wi-Fi card with a Mac-supported card, such as the **BCM94360** or **BCM94350**. These cards are supported out of the box on macOS, and you can use the **OpenCore Legacy Patcher (OCLP)** to apply the necessary patches.

### **Known Bugs**:
- **Touchpad Gestures**: The 3-finger gestures are quite buggy and may not work consistently.
- **Touchpad Behavior**: The touchpad only works under polling mode, which might cause responsiveness issues.
- **Power Management & Sleep**: Due to the inability to unlock **CFG-Lock** on the X270, there are several bugs related to **AppleCpuPmCfgLock**, particularly with power management and sleep functions.

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

## _Touchpad Gestures Fixes_

The **ThinkPad X270** uses a **Synaptics SMBus** touchpad, and achieving good touchpad functionality on macOS can require some trial and error with different kexts. Below are the methods that work best for this model:

### **Recommended Kexts**:
1. **VoodooPS2 + VoodooSMBus + VoodooRMI (SMBUS plugin)**:  
   This combination provides the best touchpad gestures and responsiveness, but **touchpad buttons may not work**.

2. **VoodooPS2 + VoodooGPIO.kext**:  
   This setup results in **mediocre gesture functionality**, but it allows the **touchpad buttons** to function.

### Additional Fixes:
- **SSDT-ThinkPad_ClickPad.aml**:  
   This SSDT file helps improve touchpad functionality. You can experiment with its parameters or disable it to adjust responsiveness.

### Download Links:
- **VoodooSMBus and VoodooRMI**: [VoodooSMBus GitHub](https://github.com/VoodooSMBus/VoodooRMI/releases)
- **VoodooGPIO.kext**: [VoodooGPIO GitHub](https://github.com/RehabMan/VoodooGPIO)

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

- **Enable S3 Sleep**: S4 sleep (hibernation) has significant issues on this system. Stick with S3 sleep mode for better stability.

---
