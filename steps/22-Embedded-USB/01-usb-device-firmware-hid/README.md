# 22-01 · USB Device Firmware + HID Keystroke Injection (pico-ducky)

**Week:** W28–29 · **Track:** N · **Prev:** [`../../13-USB-Security/01-usb-protocol-stack-basics`](../../13-USB-Security/01-usb-protocol-stack-basics/README.md) · **Next:** [`../02-duckyscript-engine`](../02-duckyscript-engine/README.md)

## Objective
Build the core of an O.MG-class cable: a Raspberry Pi Pico that enumerates as a USB keyboard and types anything you tell it. First time you are a *device*, not a *host* — the USB stack you analyzed in Track F, now on the other side.

## Tasks
- [ ] Flash a Pico; toolchain (Pico SDK or Arduino-Pico); blink + serial — the "hello world" of firmware
- [ ] TinyUSB device stack: understand enumeration, descriptors, configurations, interfaces, endpoints
- [ ] Implement HID keyboard: device descriptor + HID report descriptor (8-byte keyboard reports: modifiers + usage IDs)
- [ ] Keystroke injection: send `Hello world` as HID reports; verify in a real OS text field
- [ ] Type a shell payload (e.g. `powershell -enc ...`) and watch it land — the Rubber Ducky primitive
- [ ] Input-protection escape: OS keyboard shortcuts (Alt+F4, Ctrl+Esc) and how they defeat naive injection

## Resources
- dbisu/pico-ducky — the reference project; read it, then write your own
- TinyUSB docs (hid.h, hid_usage tables); USB in a Nutshell; USB HID spec (usb.org)
- Your `13-USB-Security/01` protocol notes, inverted (host→device view)

## Exit Criteria
- [ ] Pico types a full payload into a locked/unlocked OS session — `labs/` (video or transcript)
- [ ] Descriptors dumped with a USB analyzer (USBView/USBPcap) and annotated — `notes/`

## Links
- [pico-ducky](https://github.com/dbisu/pico-ducky)
- [TinyUSB](https://github.com/hathach/tinyusb)
- [USB in a Nutshell](https://www.beyondlogic.org/usbnutshell/usb1.shtml)
- [USB HID Usage Tables (usb.org)](https://www.usb.org/sites/default/files/hut1_22.pdf)
