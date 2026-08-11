# 14-06 · Real USB Device + Userspace Driver + App

**Week:** W37–38 · **Track:** G · **Prev:** [`../05-cross-os-patterns-fuzzing`](../05-cross-os-patterns-fuzzing/README.md) · **Next:** [`../07-capstone-cross-os-driver`](../07-capstone-cross-os-driver/README.md)

## Objective
Build something real you can hold: an actual USB device (Raspberry Pi Pico, ~$5, breadboard, no soldering), a userspace driver, and a macOS app that controls it. This is the missing **real-hardware** link — 14-02 drove a virtual device, 13-02 emulated one. Key fact that makes this easy: USB is one of the few buses where userspace drivers are first-class — a vendor-class device is fully controllable from an app with **zero kernel code**.

## Hardware
- **Raspberry Pi Pico** (RP2040, ~$5) + breadboard + 2 buttons + 2 LEDs + 10 kΩ pot (~$10 total). TinyUSB (the USB device stack) is built into the Pico SDK. Buy genuine Pico — clones ship with broken USB IDs.
- Alt: **ESP32-S3** (native USB + WiFi) if you want a wireless angle later.
- Class: **vendor-specific (0xFF)**, not HID — HID needs no driver; you want the driver work. Protocol: 4-byte commands (LED bits) on bulk IN, 8-byte reports (2 buttons + 12-bit pot) on bulk OUT.

## Tasks
- [ ] Firmware (C, Pico SDK + TinyUSB): vendor class — control + 1 bulk IN + 1 bulk OUT; debounce, ADC read, command parser
- [ ] Enumerate: device appears in System Information; `ioreg -p IOUSB -l | grep -A5 <vid:pid>` shows your descriptors
- [ ] macOS userspace driver: IOKit `IOUSBHostDevice` — match VID/PID, open, claim interface, bulk transfers (classic `IOUSBDeviceInterface` also fine). No kernel driver involved
- [ ] App: SwiftUI — connect/disconnect, LED toggle + PWM dim, live button + pot meters, ~10 ms poll loop
- [ ] Cross-link: feed button events into your NES emulator as an alternate controller — one device, two consumers (Track B)
- [ ] Linux port: same protocol via libusb — the userspace twin of your 13-02 kernel driver
- [ ] Windows stretch: WinUSB via Zadig in the VM

## Resources
- TinyUSB docs; Pico SDK examples (`hello_world/usb`); USB in a Nutshell (beyondlogic.org)
- Apple `IOUSBLib`/`IOUSBHost` docs; LDD3 ch.18 (from 13-02) for protocol theory
- Track G 07 turns this into the 3-OS capstone target

## Security lens
- This device is now your **own** fuzzing target for 13-04: the firmware command parser is the attack surface you wrote
- 14-04 is the kernel-driver stretch: wrap this device in a DriverKit dext instead of raw IOKit

## Exit Criteria
- [ ] App controls the physical device end-to-end — `code/` (firmware + app + libusb client)
- [ ] "userspace vs kernel driver — when each" decision notes — `notes/`
