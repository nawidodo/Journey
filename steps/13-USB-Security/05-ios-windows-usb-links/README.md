# 13-05 · iOS + Windows USB Links

**Week:** W29–30 · **Track:** F · **Prev:** [`../04-usb-fuzzing-device-emulation`](../04-usb-fuzzing-device-emulation/README.md) · **Next:** [`../06-capstone-usb-attack`](../06-capstone-usb-attack/README.md)

## Objective
Your USB knowledge pays off — the bootrom over USB (checkm8), and the Windows USB driver surface.

## Tasks
- [ ] checkm8: bootrom USB (DFU) bug — the original Apple USB exploitation path, link to Phase 7
- [ ] USB on iOS: iBoot USB, lockdownd/muxd surface
- [ ] Windows USB stack: KMDF/WDF USB drivers, IRPs, USBView/USBPcap — link to Track D (Windows kernel)
- [ ] Read one real USB exploit writeup end-to-end (Linux, Windows, or checkm8)

## Resources
- checkm8 writeups (axi0mX, Habr); "checkm8 in depth" blog posts
- Microsoft USB driver docs; USBView; USBPcap
- Your Phase 4 RE skills (Ghidra on a USB driver)

## Exit Criteria
- [ ] 1-page checkm8 root cause (USB → bootrom bug) — `notes/`
- [ ] 1 Windows USB bug class mapped to your Track D plan — `notes/`