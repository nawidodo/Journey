# 14-04 · macOS DriverKit (dext)

**Week:** W33–35 · **Track:** G · **Prev:** [`../03-windows-kmdf-wdm`](../03-windows-kmdf-wdm/README.md) · **Next:** [`../05-cross-os-patterns-fuzzing`](../05-cross-os-patterns-fuzzing/README.md)

## Objective
Modern macOS drivers. Apple Silicon killed kexts; **DriverKit (dext)** is the only forward path — and it runs in userspace, so it's safe on your own Mac.

## Tasks
- [ ] DriverKit model: dext vs kext (why Apple deprecated kexts); IOService, IOUserClient, IOKit
- [ ] Build a dext skeleton: service match, start/stop, external methods (`IOConnectCallMethod`)
- [ ] Deploy via `systemextensiond`, sign with your Developer ID; optionally classic IOKit kext on an Intel VM for legacy
- [ ] Map the same interface you built in 14-01/14-03: Linux ioctl ⇄ Windows DeviceIoControl ⇄ dext method
- [ ] Note the Phase 7 link: many iOS bugs are IOKit method implementations

## Resources
- Apple DriverKit docs; WWDC DriverKit sessions; Apple dext samples
- IOKit fundamentals (Apple docs, Levin Vol 1–2)

## Exit Criteria
- [ ] dext + client app demo — `code/`
- [ ] "dext vs kext vs ioctl/IOCTL" comparison — `notes/`