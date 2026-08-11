# 22-02 · DuckyScript Engine: Payload Compiler + Layouts

**Week:** W30 · **Track:** N · **Prev:** [`../01-usb-device-firmware-hid`](../01-usb-device-firmware-hid/README.md) · **Next:** [`../03-esp32-wifi-c2`](../03-esp32-wifi-c2/README.md)

## Objective
Stop hand-typing byte arrays. Write your own DuckyScript parser → compiler → HID report generator, so any payload is `ducky.bin` away from the device. This is the "language" layer an O.MG cable ships with.

## Tasks
- [ ] DuckyScript language: STRING, DELAY, REPEAT, ALT-SHIFT-TAB, APPLE_*, STRINGLN, GUI, function keys, arrows
- [ ] Parser + IR: script text → token list → command IR (no exec yet; this is a compiler)
- [ ] Scancode mapping: HID usage IDs per key; US vs International layout differences (ISO, JIS) — same usage, different keycaps
- [ ] Emit the binary payload format; load on-device and replay
- [ ] Evil-but-robust: DELAY handling, WAIT_FOR_WINDOW-style primitives, fallback when a key combo doesn't land
- [ ] Round-trip test: compile a payload, decompile your own format back — find your compiler bugs

## Resources
- Hak5 DuckyScript 3.0 docs (official); old v1.0 docs (field-standard payloads)
- HID Usage Tables + keyboard descriptor from 22-01
- Public payload repos (e.g. hausec/ducky, hak5 payloads) — collect, compile, test your engine against them

## Exit Criteria
- [ ] Compiler handles ≥ 20 DuckyScript commands; corpus of public payloads compiles and runs — `code/`
- [ ] Layout table for ≥ 2 keyboard layouts, tested on real hardware — `notes/`

## Links
- [Hak5 DuckyScript docs](https://docs.hak5.org/hak5-usb-rubber-ducky/)
- [USB HID Usage Tables (usb.org)](https://www.usb.org/sites/default/files/hut1_22.pdf)
- [Public Ducky payload corpus (hausec)](https://github.com/hak5/usb-rubber-ducky-payloads)
