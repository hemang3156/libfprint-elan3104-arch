# Arch Linux Driver for ELAN 3104 Fingerprint Sensor

This repository provides an Arch Linux `PKGBUILD` to get the unsupported ELAN fingerprint sensor (Hardware ID: `04f3:3104`) working on Arch-based distributions. 

It automatically downloads `libfprint`, applies the experimental community patches from `goodix-fp-linux-dev` and `r4nd3l`, fixes OpenCV 5 build compatibility issues, and installs it safely via pacman.

## How to Install

Open your terminal and run the following commands:

```bash
git clone https://github.com/YOUR-USERNAME/libfprint-elan3104-arch.git
cd libfprint-elan3104-arch
makepkg -si
```

## How to Use & Enroll

Once installed, you can enroll your fingerprint by running:
```bash
fprintd-enroll
```

**Important Physical Quirk:** Even though the sensor on ASUS/Vivobook laptops looks like a square "touch" sensor, this experimental driver uses image-stitching logic. **Do not just press your finger flat.** Instead, place your finger at the top of the sensor and **slowly swipe it down** across the square to avoid "swipe too short" errors.
