# 13-01 · USB Protocol + Host Stack Basics

**Week:** W24–25 · **Track:** F · **Prev:** — · **Next:** [`../02-linux-usb-driver-dev`](../02-linux-usb-driver-dev/README.md)

## Objective
Enumerate a real USB device end-to-end from wire capture — the substrate every USB attack is built on.

## Tasks
- [ ] USB model: host/target, speeds, packets (SYNC/PID/ADDR/ENDP/CRC), transfer types (control/bulk/interrupt/isochronous)
- [ ] Descriptors: device/configuration/interface/endpoint; HID report descriptor — `lsusb -v` on your Mac
- [ ] Enumeration: GET_DESCRIPTOR dance, SET_CONFIGURATION, driver binding
- [ ] Linux host stack: usbcore, HCD (xHCI), URB lifecycle — read `drivers/usb/core/*`
- [ ] Capture enumeration with usbmon + Wireshark; trace one device you own

## Resources
- USB in a Nutshell (beyondlogic.org); USB Made Simple; USB 2.0 spec ch.5/9
- Linux kernel: `Documentation/usb/*`, `drivers/usb/core/hub.c`
- Wireshark USB capture, usbmon; `lsusb`, `usbhid-dump`

## Exit Criteria
- [ ] Annotated enumeration capture — `labs/`
- [ ] 1-page "USB model from SYNC to URB" — `notes/`

## Links
- [USB in a Nutshell](https://www.beyondlogic.org/usbnutshell/usb1.shtml)
- [USB spec (usb.org)](https://www.usb.org/documents)
