# Arch Linux Driver for ELAN 3104 Fingerprint Sensor

[![Arch Linux](https://img.shields.io/badge/Arch_Linux-1793D1?style=flat-square&logo=arch-linux&logoColor=white)](https://archlinux.org/)
[![PKGBUILD](https://img.shields.io/badge/Packaging-PKGBUILD-blue?style=flat-square)](PKGBUILD)
[![Hardware ID](https://img.shields.io/badge/Hardware_ID-04f3%3A3104-orange?style=flat-square)](https://linux-hardware.org/?id=usb:04f3-3104)
[![License](https://img.shields.io/badge/License-LGPL-green?style=flat-square)](https://www.gnu.org/licenses/lgpl-3.0.html)

This repository provides an Arch Linux **`PKGBUILD`** package to enable the unsupported **ELAN 3104** fingerprint sensor (Hardware ID: `04f3:3104`, commonly found on ASUS Vivobook and Zenbook series) on Arch Linux and Arch-based distributions (EndeavourOS, Manjaro, etc.).

It pulls `libfprint`, applies the community drivers/patches from `goodix-fp-linux-dev` and `r4nd3l`, patches OpenCV 5 build compatibility issues, and packages it cleanly for system installation via `pacman`.

---

## 📋 Prerequisites

Ensure you have your development tools and `fprintd` installed:

```bash
sudo pacman -S --needed base-devel git fprintd
```

Verify that your system detects the ELAN sensor:

```bash
lsusb | grep -i "04f3:3104"
```

---

## 🚀 Installation

Clone this repository and build the package with `makepkg`:

```bash
git clone https://github.com/hemang3156/libfprint-elan3104-arch.git
cd libfprint-elan3104-arch
makepkg -si
```

Enable and start the `fprintd` systemd service:

```bash
sudo systemctl enable --now fprintd.service
```

---

## 🖐️ Enrollment & Usage

Once installed, enroll your fingerprint using:

```bash
fprintd-enroll
```

> [!IMPORTANT]
> **Physical Sensor Quirk:**  
> Although the physical sensor looks like a stationary touch pad, this driver operates on **image-stitching logic**. **Do not just press and hold your finger flat.** Instead, place the tip of your finger at the top of the sensor and **slowly swipe downward** across the pad to register consistent swipes and avoid `"swipe-too-short"` errors.

To verify your enrolled fingerprint:

```bash
fprintd-verify
```

---

## 🔐 PAM Configuration (Optional)

To enable fingerprint authentication for login or `sudo`:

### For `sudo`:
Add `auth sufficient pam_fprintd.so` as the first auth rule in `/etc/pam.d/sudo`:

```pam
#%PAM-1.0
auth      sufficient   pam_fprintd.so
auth      include      system-auth
account   include      system-auth
session   include      system-auth
```

### For SDDM / GDM / System Login:
Add `auth sufficient pam_fprintd.so` at the top of `/etc/pam.d/system-local-login`.

---

## 🔍 Troubleshooting

- **Service logs:**  
  Monitor real-time daemon logs when enrolling or verifying:
  ```bash
  journalctl -u fprintd.service -f
  ```
- **Device not found:**  
  Ensure udev rules were installed and reload them:
  ```bash
  sudo udevadm control --reload-rules && sudo udevadm trigger
  ```

---

## 🙏 Credits & Acknowledgments
- Upstream driver patch by [r4nd3l (elan-3104-fingerprint-linux)](https://github.com/r4nd3l/elan-3104-fingerprint-linux)
- [goodix-fp-linux-dev libfprint fork](https://github.com/goodix-fp-linux-dev/libfprint)
