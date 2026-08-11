# 13-04 · USB Fuzzing + Device Emulation

**Week:** W27–29 · **Track:** F · **Prev:** [`../03-usb-attack-surface`](../03-usb-attack-surface/README.md) · **Next:** [`../05-ios-windows-usb-links`](../05-ios-windows-usb-links/README.md)

## Objective
Act as the malicious device — emulate and fuzz a host stack, trigger a real driver bug in a VM.

## Tasks
- [ ] Linux gadget framework (functionfs) or Facedancer/GreatFET — present as an arbitrary USB device
- [ ] **Mass-storage gadget**: expose a raw disk image as a fake "USB drive" (configfs `usb_f_mass_storage` on a gadget-capable board) — Windows mounts it as a removable drive
- [ ] Spoof VID/PID/serial; compare what Windows records on mount: `USBSTOR` registry keys, SetupAPI logs, PnP events (the "fool the analyst" layer — real detection trusts these, not enumeration)
- [ ] Build a malicious HID device: keystroke injection over gadget
- [ ] Fuzz a USB driver in QEMU VM: syzkaller USB fuzzer (or `usbfuzzer`) against a parsing-heavy driver
- [ ] Reproduce a descriptor-parsing crash with your own gadget (start from a known CVE trigger)
- [ ] Defense mirror: write detection for your own gadget (Sysmon/Sigma for USB, macOS unified logs)

## Resources
- Linux kernel: `Documentation/usb/gadget_*`, functionfs examples, `usb_f_mass_storage` configfs docs
- Raspberry Pi Zero gadget-mode tutorials (dwc2/configfs); Facedancer/GreatFET
- syzkaller docs (USB); `usbfuzzer`
- Windows USBSTOR/SetupAPI forensics (SANS posters, Velociraptor USB artifacts)
- QEMU USB passthrough; your Dojo VM

## Exit Criteria
- [ ] Malicious-HID demo or fuzz-crash repro — `labs/`
- [ ] Detection rules for your gadget — `labs/`