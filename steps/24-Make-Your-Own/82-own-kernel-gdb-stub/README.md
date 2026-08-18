# 24-82 · Own kernel gdb stub — remote debug protocol for your 24-01 OS (stretch)

**Week:** W46+ opt (stretch) · **Track:** P · **Prev:** [`../81-own-unikernel`](../81-own-unikernel/README.md) · **Next:** [`../83-own-backup-tool`](../83-own-backup-tool/README.md) · **Pairs:** 24-01, 15-07, 02-11

## Objective
Your 24-01 kernel panics and you print; real kernels stop at breakpoints. Build a gdb stub: implement the remote serial protocol (RSP — `g`, `m`, `M`, `c`, `s`, breakpoints) inside your kernel, wire a serial port (QEMU `-serial`), and debug your kernel from host gdb: single-step kernel code, read/write kernel memory, set software breakpoints by rewriting instructions — pairs 15-07 skills applied at the lowest layer. The payoff: every later kernel lab (01-xv6, 05, 24-10) gets a real debugger, and you'll never print-debug a kernel again.

## Tasks
- [ ] RSP: the packet framing (`$...#checksum`), `+/-` ack, core commands (g/G regs, m/M mem, c continue, s step, Z/z breakpoints, k kill)
- [ ] Integration: interrupt-driven serial (24-01 driver), a panic→breakpoint entry, exception hook (int3 → stub)
- [ ] Breakpoints: software (int3 patch + restore) + hardware debug registers; step-over/into semantics
- [ ] Host side: gdb `target remote :port` connects (the oracle); set a breakpoint in your kernel's timer ISR and observe the scheduling state
- [ ] Writeup: RSP security (the "debugger as attack surface" note — pairs 19 hooking), stub-in-the-wild (kgdb/jtag) — `notes/`

## Resources
- GDB RSP spec (the manual); kgdb docs (peer); your 24-01/15-07 code

## Exit Criteria
- [ ] gdb attaches to your kernel, single-steps + inspects memory — `labs/` + `code/`
- [ ] RSP + attack-surface notes — `notes/`

## Links
- [GDB Remote Serial Protocol](https://sourceware.org/gdb/current/onlinedocs/gdb.html/Remote-Protocol.html)
- [QEMU gdbstub](https://wiki.qemu.org/Documentation/TCG)