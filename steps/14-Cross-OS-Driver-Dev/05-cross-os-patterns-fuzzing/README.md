# 14-05 · Cross-OS Patterns + Fuzzing Your Own Drivers

**Week:** W35–37 · **Track:** G · **Prev:** [`../04-macos-driverkit-dext`](../04-macos-driverkit-dext/README.md) · **Next:** [`../06-usb-device-driver-app`](../06-usb-device-driver-app/README.md)

## Objective
The same driver written three ways — and break your own before someone else does.

## Tasks
- [ ] One interface (e.g., "register N-byte config") implemented on all 3 OSes; table the differences (dispatch, buffer validation, refcounts)
- [ ] Linux: syzkaller or a honggfuzz harness against your char driver; fix what it finds
- [ ] Windows: fuzz your IOCTLs (harness + WinDbg dump analysis); check METHOD_NEITHER handling
- [ ] macOS: fuzz dext method arguments — it's userspace, so trivially easy; study IOKit validation patterns
- [ ] Write up which OS's validation model is strongest and why

## Resources
- syzkaller docs; honggfuzz; your 14-01/03/04 outputs
- WinDbg crash-dump analysis (your Track D setup)

## Exit Criteria
- [ ] Fuzz findings + fixes for ≥1 driver — `labs/`
- [ ] Cross-OS comparison table — `notes/`

## Links
- [syzkaller](https://github.com/google/syzkaller)
- [Trail of Bits Fuzzing 101](https://github.com/trailofbits/fuzzing101)
