# 14-03 · Windows KMDF/WDM Driver

**Week:** W31–33 · **Track:** G · **Prev:** [`../02-hardware-facing-dma-irq-mmio`](../02-hardware-facing-dma-irq-mmio/README.md) · **Next:** [`../04-macos-driverkit-dext`](../04-macos-driverkit-dext/README.md)

## Objective
Write the Windows-side driver — device objects, IRPs, IOCTL dispatch — the write-half of Track D's HEVD bugs.

## Tasks
- [ ] WDK build (VS + WDK or CLI); test-signing on your Win10/11 VM
- [ ] KMDF skeleton: DriverEntry, device add, `EvtIoDeviceControl`, PnP/power callbacks
- [ ] IRP flow: major functions, completion routines; IOCTL dispatch table
- [ ] Debug in WinDbg (`kd>`): breakpoints on your IRP handlers; drive it via `DeviceIoControl` from a client
- [ ] Cross-reference Track D: spot which of your IOCTLs would be exploitable (buffer handling, METHOD_NEITHER)

## Resources
- Microsoft KMDF docs + `kmdfdriver` sample; WDK getting-started; OSDev WDM
- Your Track D VM (WinDbg setup)

## Exit Criteria
- [ ] KMDF driver + client demo — `code/`
- [ ] "IRP → IOCTL" flow notes — `notes/`

## Links
- [WDK/KMDF docs](https://learn.microsoft.com/en-us/windows-hardware/drivers/)
- [Windows driver samples](https://github.com/microsoft/Windows-driver-samples)
