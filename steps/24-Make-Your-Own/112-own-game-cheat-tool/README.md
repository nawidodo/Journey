# 24-112 · Own game cheat tool — scan/refine/write, RE muscle on your own game (stretch)

**Week:** W46+ opt (stretch) · **Track:** P · **Prev:** [`../111-own-metal-compute`](../111-own-metal-compute/README.md) · **Pairs:** 21-10, 15-07, 24-16, 24-103

## Objective
The classic RE playground: find and modify values in a live process — build GameCheat-lite — process attach (ptrace/Darwin or proc_pidaccess on your own machine, own processes only), memory scan (read process pages via `mach_vm_read`/`/proc/.../mem`), value scan/refine (search exact/range/unknown, the narrowing loop), and write (mach_vm_write with the page-protection dance — 24-103 APIs, 24-16 page discipline). Target: your OWN test game (a 24-63 clone or any self-compiled binary) with a known score/health counter; the harvest: the classic 15-07/SIGTRAP workflow — find the pointer chain (pairs 24-82 breakpoint thinking), and the detection writeup (what anti-cheat looks for — the EDR analogy to 21-06, why games rootkit themselves).

## Tasks
- [ ] Attach: open own process, read/write memory (mach_vm on macOS; /proc/.../mem on Linux — cross-platform table), handle permissions
- [ ] Scan: value scan (eq/range/unknown), refine loop, filter by type; the search state machine (21-10 scanner DNA)
- [ ] Write: targeted poke + re-protect; the "infinite ammo" write/refresh loop on your own game
- [ ] Pointer chain: find address that points to the value (pairs 15-07 watchpoints); survive the value moving between runs — the base+offset dance (24-97/Mach-O sections)
- [ ] Detection writeup: what a real anti-cheat watches (page access, writes, timing — 21-06 detection thinking), ethical line (own processes/games only) — `notes/`

## Resources
- Game-hacking tutorials (cheat-engine/lief docs — technique, not targets); code from 21-10 (scanner) + 24-103 (mach_vm) — the actual reuse cases

## Exit Criteria
- [ ] Scan-refine-write loop edits your own game's counter live — `labs/` + `code/`
- [ ] Pointer-chain + detection writeup — `notes/`

## Links
- [LIEF (binary hacking library)](https://lief-project.github.io/)
- [Cheat Engine](https://cheatengine.org/)