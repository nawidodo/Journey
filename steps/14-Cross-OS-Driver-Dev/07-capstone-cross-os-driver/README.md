# 14-07 · Capstone: One Driver on All Three OSes 🚩 M15

**Week:** W38–41 · **Track:** G · **Prev:** [`../06-usb-device-driver-app`](../06-usb-device-driver-app/README.md)

## Objective
A single driver idea shipped on Linux, Windows, and macOS — plus a deliberate bug documented from the defender's side (hand-off to Track D). The default target is now **real hardware**: the USB device you built in 14-06.

## Tasks
- [ ] Pick one: (a) virtual control device (config + status), (b) USB gadget-class driver, (c) virtio/emulated-device driver, or **(d) your 14-06 real USB device — recommended** (you already have firmware + macOS userspace driver + app)
- [ ] Implement on Linux (14-01/02), Windows KMDF (14-03), macOS dext (14-04)
- [ ] For (d): same userspace client ported as libusb (Linux) + WinUSB (Windows) + IOKit (macOS), optionally wrapping the device in a KMDF/dext instead of raw userspace
- [ ] One userspace client (ioctl / DeviceIoControl / IOConnectCallMethod) driving all three
- [ ] Plant one bug class in one port; document detection (KASAN / WinDbg / analyst) — Track D practice target
- [ ] Full writeup comparing the three driver models

## Resources
- Your steps 01–05 outputs; WDK, Apple DriverKit, kernel docs
- Your 14-06 device + firmware (option d)

## Exit Criteria
- [ ] **M15:** working driver trio + shared client — `code/`
- [ ] Comparison + bug-report writeup — `notes/`