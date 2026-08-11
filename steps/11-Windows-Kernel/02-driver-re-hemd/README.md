# 11-02 · Driver Reverse Engineering + HEVD

**Week:** W31–33 · **Track:** D · **Prev:** [`../01-windows-internals-debugger`](../01-windows-internals-debugger/README.md) · **Next:** [`../03-pool-overflow-token-stealing`](../03-pool-overflow-token-stealing/README.md)

## Objective
Your lab target: HackSys Extreme Vulnerable Driver (HEVD) — the Windows "pwn.college". Reverse it, find each bug, understand the IOCTL dispatch.

## Tasks
- [ ] Build HEVD (WDK), load in WinDbg guest
- [ ] Ghidra/IDA: map `DeviceIoControl` → dispatch table → vulnerable routines
- [ ] Identify all bug classes in HEVD: stack overflow, pool overflow, UAF, type confusion, race, arbitrary write
- [ ] WinDbg: set breakpoints, confirm each bug fires; `!pool`, heap tracking on

## Resources
- HEVD (HackSysTeam/HEVD)
- *Practical Reverse Engineering* (Dang/Mani/…); x64dbg + Ghidra
- Your Journey RE phase (recommended) — Ghidra fluency transfers

## Exit Criteria
- [ ] Find-and-confirm ≥3 HEVD bugs from source + disassembly — `labs/`
- [ ] IOCTL + dispatch map drawn — `notes/`

## Links
- [HEVD (HackSysTeam)](https://github.com/hacksysteam/HackSysExtremeVulnerableDriver)
- [HEVD solutions](https://github.com/hacksysteam/HEVD-Solutions)
