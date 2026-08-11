# 22-05 · Capstone: O.MG-Cable Clone 🚩 M23

**Week:** W34 · **Track:** N · **Prev:** [`../04-stealth-evasion-hardware`](../04-stealth-evasion-hardware/README.md)

## Objective
Ship a working keystroke-injection cable-class device, end to end: plug in → auto-run payload → report over WiFi → manage from a phone. This is the M23 checkpoint — re-derive 22-01→22-04 with the notes closed before starting.

## Tasks
- [ ] Full chain on one device: DuckyScript engine + HID injection + WiFi AP + web UI + exfil + auto-run on plug
- [ ] Three realistic payloads (credential prompt, reverse shell, persistence) tested against a clean lab machine
- [ ] Defender round-trip: run your own 22-04 detections against the capstone device; document what lands and what doesn't
- [ ] Hardware presentation: dev-board build photos/video; inline-board plan or prototype (stretch)
- [ ] Writeup: architecture diagram (descriptors ↔ engine ↔ WiFi ↔ exfil), limits, detection guidance — `notes/`
- [ ] **Stretch:** KiCad inline USB-C PCB fabricated and working inside a real cable housing

## Resources
- Your 22-01…22-04 outputs; O.MG docs/teardowns as the feature benchmark
- Track F host-side notes, Track E C2 patterns, Track M detection rules

## Exit Criteria
- [ ] End-to-end demo video: plug → auto-run → exfil → UI — `labs/`
- [ ] Writeup with architecture + detection section — `notes/`

## Links
- [O.MG Cable (Hak5)](https://shop.hak5.org/products/omg-cable)
- [KiCad](https://www.kicad.org/)
- [JLCPCB](https://jlcpcb.com/)
