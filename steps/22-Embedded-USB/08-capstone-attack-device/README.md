# 22-08 · Capstone: plugandpwn-Class Attack-USB Device 🚩 M23

**Week:** W36 · **Track:** N · **Prev:** [`../07-fault-injection-glitching`](../07-fault-injection-glitching/README.md)

## Objective
Ship a working attack-USB device, end to end — the O.MG-cable **and** plugandpwn feature sets on one stick: plug-in auto-run → keystroke injection → exfil (over AP *and* the victim's network) → network pivot → managed from a phone. M23 checkpoint — re-derive 22-01→22-06 with the notes closed before starting.

## Tasks
- [ ] Full chain on one device: DuckyScript engine + HID injection + WiFi AP + web UI + exfil + auto-run on plug
- [ ] Network pivot (22-05) + composite firmware + OTA/self-destruct (22-06) all live in the same image
- [ ] Three realistic payloads (credential prompt, reverse shell, persistence) tested against a clean lab machine
- [ ] Defender round-trip: run your own 22-04 detections against the capstone device; document what lands and what doesn't
- [ ] Hardware presentation: dev-board build photos/video; inline-board plan or prototype (stretch)
- [ ] Writeup: architecture diagram (descriptors ↔ engine ↔ WiFi ↔ pivot ↔ exfil), limits, detection guidance — `notes/`
- [ ] **Stretch:** KiCad inline USB-C PCB fabricated and working inside a real cable housing

## Resources
- Your 22-01…22-06 outputs; plugandpwn + O.MG docs/teardowns as the feature benchmark
- Track F host-side notes, Track E C2 patterns, Track M detection rules

## Exit Criteria
- [ ] End-to-end demo video: plug → auto-run → exfil → UI — `labs/`
- [ ] Writeup with architecture + detection section — `notes/`

## Links
- [plugandpwn](https://plugandpwn.com/)
- [O.MG Cable (Hak5)](https://shop.hak5.org/products/omg-cable)
- [KiCad](https://www.kicad.org/)
- [JLCPCB](https://jlcpcb.com/)
