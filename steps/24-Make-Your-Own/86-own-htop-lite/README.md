# 24-86 · Own htop-lite — /proc TUI monitor, the sysadmin's mirror (stretch)

**Week:** W46+ opt (stretch) · **Track:** P · **Prev:** [`../85-own-config-manager`](../85-own-config-manager/README.md) · **Next:** [`../87-own-linker`](../87-own-linker/README.md) · **Pairs:** 24-14, 21-06, 24-30

## Objective
Every process you hunt is visible in /proc: build the tool that surfaces it — htop-lite: /proc parsing (stat, status, smaps, fd), process/CPU/mem aggregation (the sampling math — pairs 24-30), and a live TUI (your 24-14 ANSI engine: redraw loop, keyboard, panels). The security tie-in is the payoff: watch a running Linux kernel module or your own 12-06 implant from /proc — what an admin sees, what EDRs (21-06) and rootkits (18-02) hide by faking.

## Tasks
- [ ] /proc: parse stat/status/smaps/fd per PID (fields you can name), sort by cpu/mem, the sampling-window math (24-30)
- [ ] TUI: 24-14 draws the table; refresh timer, header graphs (CPU/mem), sort/scroll keys (24-38 input)
- [ ] Details: per-process view (threads, open fds, environments — pairs 21-10), search/filter
- [ ] Lab: run your own 24-16 memory-heavy test + a busy loop — htop-lite numbers match /proc's raw values (the cross-check) — `labs/`
- [ ] Writeup: what /proc hides (rootkit fd/comm faking — pairs 18-02), the EDR-vs-admin visibility gap — `notes/`

## Resources
- proc(5) man (the manual); htop source (peer); your 24-14/24-30/21-06 code

## Exit Criteria
- [ ] Live process monitor matches raw /proc values — `labs/` + `code/`
- [ ] Visibility-gap writeup — `labs/` + `notes/`

## Links
- [proc(5)](https://man7.org/linux/man-pages/man5/proc.5.html)
- [htop](https://htop.dev/)