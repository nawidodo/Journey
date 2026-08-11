# 11-01 · Windows Internals + Debugger

**Week:** W30–31 · **Track:** D · **Prev:** — · **Next:** [`../02-driver-re-hemd`](../02-driver-re-hemd/README.md)

## Objective
Third kernel, new rules: Windows has no `commit_creds`, has IRQLs, SSDT, KPCR, and pool internals that differ from Linux/XNU.

## Tasks
- [ ] Kernel mode vs user mode; IRQL levels; dispatch routines (MJ_*); DriverObject/DeviceObject
- [ ] Setup: WinDbg + two VMs (host debugging guest), kernel symbols, `!analyze -v`, `!pool`, `!process`
- [ ] Read: `ntoskrnl.exe` exports, SSDT, `_KPCR/_KPRCB`; `_EPROCESS` → token location
- [ ] Write a trivial WDM/UMDF driver in WDK; load it; `dbgprint` from kernel to WinDbg
- [ ] Userland privesc ladder (Sektor7 course, 20 techniques): token manipulation, service misconfigs, COM hijack — non-admin → SYSTEM

## Resources
- *Windows Internals* (Russinovich) pt.1 — the book
- OSR / "What the heck is WDK" setup guides; WinDbg docs
- Sektor7 RTO Privilege Escalation in Windows (20 techniques)
- Reuse: your Linux module + xv6 concepts — this is the deltas-only step

## Exit Criteria
- [ ] Debugger session: break, walk `_EPROCESS` list, find system token — `labs/`
- [ ] Diagram: IRQL / dispatch / pool in `notes/`
