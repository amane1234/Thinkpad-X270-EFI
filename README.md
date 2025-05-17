# **ThinkPad X270 OpenCore EFI for macOS**

EFI for Lenovo ThinkPad X270 using OpenCore, compatible with macOS.
For the latest version, download from the [Releases](https://github.com/amane1234/Thinkpad-X270-EFI/releases) section instead of using the source folder.

---

## *Laptop Specifications*

| **Component**     | **Details**           |
| ----------------- | --------------------- |
| **CPU**           | Intel i5-6300U        |
| **iGPU**          | Intel HD Graphics 520 |
| **SSD**           | Samsung 870 EVO       |
| **Wireless**      | Intel AC-8260ngw      |
| **SMBIOS**        | MacBookPro 13,1       |
| **Bootloader**    | OpenCore 1.0.3        |
| **macOS Version** | macOS Ventura         |

---

## *Intel Wi-Fi Support*

To enable Wi-Fi, install either **itlwm.kext** or **airportitlwm.kext** — not both.
This EFI uses **itlwm**, so you'll need **[HeliPort](https://github.com/OpenIntelWireless/HeliPort)** for Wi-Fi management.

---

## *Known Issues*

* **Touchpad Gestures**: 3-finger gestures are unstable due to poor hardware; no fix available.
* **CMOS Reset after waking up from Hibernation**: Fix with:

  * `rtcfx_exclude=0x80-0xAB` in boot-args with `RTCMemoryFixup.kext`
  *  If the issue persists, manually identify the faulty RTC region: [RTC Reset Fix Guide](https://dortania.github.io/OpenCore-Post-Install/misc/rtc.html#finding-our-bad-rtc-region)

---

## *SMBIOS Configuration*

Generate or modify SMBIOS with [GenSMBIOS](https://github.com/corpnewt/GenSMBIOS).

---

## *BIOS Settings for ThinkPad X270*

**Disable**:

* Secure Boot
* CSM (Compatibility Support Module)

---

## *(Experimental)* Enable Hibernation (S4)

1. Set `ThirdpartyDrives=true` and `Hibernatemode=NVRAM` in `config.plist`
2. Run in Terminal:

   ```
   sudo pmset -a hibernatemode 3
   sudo pmset -a standby 1
   ```
3. If it still doesn’t work, add:

   * `Hibernationfixup.kext` with `hbfx-ahbm=129`
   * `RTCMemoryFixup.kext` with `rtcfx_exclude=80-AB`
   * Enable `DiscardHibernatemap` at `config.plist`

---

## *Touchpad Configuration (VoodooSMBus)*

Configure touchpad settings (e.g., TrackpointDeadzone, PalmRejectionWidth) via SSDT:
[VoodooSMBus Configuration](https://github.com/VoodooSMBus/VoodooRMI?tab=readme-ov-file#configuration)

---
