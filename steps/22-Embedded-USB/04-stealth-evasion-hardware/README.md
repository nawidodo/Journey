# 22-04 · Stealth, Detection, and the Hardware Path

**Week:** W33 · **Track:** N · **Prev:** [`../03-esp32-wifi-c2`](../03-esp32-wifi-c2/README.md) · **Next:** [`../05-capstone-omg-clone`](../05-capstone-omg-clone/README.md)

## Objective
Two halves: (1) think like the defender your device just attacked — how is keystroke injection caught, and what does evasion mean *physically*; (2) the real-hardware path from dev board to inline cable board.

## Tasks
- [ ] Detection view: HID report cadence vs human typing, VID/PID fingerprints, device-type logs (Windows Device Manager/Event Log, Sysmon EID 6/13), EDR USB-device policies — write the Sigma/YARA that catches your own 22-03 device (invert Track M)
- [ ] Evasion (defensive note): USB VID/PID spoofing, report pacing, humanized timing — document limits: physical inspection always wins
- [ ] USB-C hardware: VBUS/GND/D+/D− + CC pin pull-ups for host role; inline pass-through wiring (data passes *through* the cable while the MCU taps it or fully emulates the device)
- [ ] Power design: 3.3V LDO from VBUS, quiescent current, inrush — the cable must not brown-out the victim machine
- [ ] KiCad (stretch): schematic + 2-layer inline board, SMD footprints, fab (JLCPCB), test with a multimeter/scope

## Resources
- USB-C spec / CC negotiation (usb.org, Type-C spec); "USB Made Simple" style guides
- Track M detection tools (Sigma, Sysmon) inverted; BadUSB detection writeups
- O.MG teardown/analysis posts (if available) — the physical design to learn from

## Exit Criteria
- [ ] Detection rule set that flags your device + a documented evasion/limits table — `labs/` + `notes/`
- [ ] Hardware feasibility doc: BOM, power budget, inline-board plan (KiCad board only if stretch) — `notes/`

## Links
- [USB Type-C spec (usb.org)](https://www.usb.org/usb-type-c)
- [Sysmon (Windows)](https://learn.microsoft.com/en-us/sysinternals/downloads/sysmon)
- [KiCad](https://www.kicad.org/)
