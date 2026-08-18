# 24-103 · Own macOS process monitor — sysctl + libproc, no /proc here (stretch)

**Week:** W46+ opt (stretch) · **Track:** P · **Prev:** [`../102-own-hls-streamer`](../102-own-hls-streamer/README.md) · **Next:** [`../104-own-dylib-injector`](../104-own-dylib-injector/README.md) · **Pairs:** 24-86, 24-30, 21-06, 24-97

## Objective
Your 24-86 htop-lite reads /proc; macOS has none — the data lives in sysctl + libproc. Port it: process listing (proc_listallpids → proc_pidinfo/proc_pidfdinfo), CPU/mem aggregation (host_statistics + proc_pid_rusage, the sampling math from 24-30), and a 24-14 TUI — the first macOS tool in your toolbox. The security payoff is the view: observe your own injection lab (104) and hooking demo (19-04) from the OS side — what a macOS EDR sees, what ppid/entitlement faking hides — pairs the 24-86 visibility writeup with the Apple-specific APIs (task_for_pid, codesigning state via proc_pidinfo PROC_PIDT_SHORTBSDINFO).

## Tasks
- [ ] API: sysctl (kinfo_proc KP_PROC/uniqid), proc_listallpids/proc_pidinfo; the structures and fields (24-97 format discipline applied to kernel ABI — 07 pairs)
- [ ] Metrics: CPU% via delta rusage (24-30 math), resident/phys_footprint, %mem; pid_for_task note (what needs root — 07/task_for_pid)
- [ ] TUI: 24-14 draws process table, sort by CPU/mem, refresh delta, kill via signal button (24-38 input)
- [ ] Cross-check lab: your monitor's numbers vs `top`/`ps` on the same processes (the oracle); run a busy loop + memory-hungry own 24-16 allocator — both tools agree — `labs/`
- [ ] Writeup: macOS process visibility vs Linux (no /proc, the API wall, SIP/TCC limits — 07), what a macOS EDR consumes — `notes/`

## Resources
- libproc + sysctl headers (the manual: `<libproc.h>`, `<sys/sysctl.h>`); top source (peer); your 24-86/24-30/24-14 code

## Exit Criteria
- [ ] Live monitor matches top/ps on own machine — `labs/` + `code/`
- [ ] API/visibility writeup — `notes/`

## Links
- [libproc.h](https://opensource.apple.com/source/Libc/Libc-1439.141.1/include/libproc.h.auto.html)
- [sysctl(3) man](https://www.manpagez.com/man/3/sysctl/)