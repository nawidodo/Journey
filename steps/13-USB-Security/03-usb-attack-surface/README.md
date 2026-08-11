# 13-03 · USB Attack Surface

**Week:** W26–27 · **Track:** F · **Prev:** [`../02-linux-usb-driver-dev`](../02-linux-usb-driver-dev/README.md) · **Next:** [`../04-usb-fuzzing-device-emulation`](../04-usb-fuzzing-device-emulation/README.md)

## Objective
Enumerate the attack classes — what a malicious device or malicious driver can do — and the defense for each.

## Tasks
- [ ] BadUSB / HID spoofing (fake keyboard, keystroke injection); firmware swap
- [ ] Descriptor confusion: malformed/oversized descriptors, descriptor-reparse attacks
- [ ] Device-side vs host-side attacks; usbfs `/dev/bus/usb` access control (Android USB attacks, io_uring/pegasus USB CVEs)
- [ ] Historical cases: Linux USB CVEs (`drivers/usb/`: hub, usbhid, gadget), Project Zero USB writeups
- [ ] For each class, note the defense: driver sanitization, USB quarantine (usbguard), allowlists, macOS/Windows USB restrictions

## Resources
- Project Zero blog (USB entries); CVE lists for `drivers/usb/`
- usbguard docs; BadUSB original research (Nohl); USB HID spec
- Your 13-01/13-02 notes

## Exit Criteria
- [ ] Attack-class matrix (attack → surface → defense) — `notes/`
- [ ] One USB CVE studied in depth (write-up) — `notes/`