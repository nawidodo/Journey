# 13-06 · Capstone: USB Attack + Defense 🚩 M14

**Week:** W30–32 · **Track:** F · **Prev:** [`../05-ios-windows-usb-links`](../05-ios-windows-usb-links/README.md)

## Objective
Ship one USB attack end-to-end with its own detection rules — offensive and defensive artifact in one project.

## Tasks
- [ ] Choose: (a) BadUSB-style HID payload that bypasses a defense you documented, or (b) gadget triggering a real driver bug in a VM
- [ ] Design doc first (in `notes/`): device, target, expected detections
- [ ] Build in isolated VM only — never host, no unauthorized networks
- [ ] Detection pass: write Sysmon/Sigma or macOS-log rules against your own attack; fix ≥1 evasion it catches
- [ ] Full writeup: how it works, how you'd defend (this is the deliverable)

## Resources
- Your steps 01–05 outputs; usbguard, Sysmon/Sigma, USB quarantine docs
- Public USB attack/defense teardowns

## Exit Criteria
- [ ] **M14:** USB attack + own detection rules, full chain in lab — `labs/`
- [ ] Defense-oriented writeup — `notes/`

## Links
- [GreatFET docs](https://greatscottgadgets.com/greatfet/)
- [Facedancer framework](https://github.com/greatscottgadgets/facewhisperer)
