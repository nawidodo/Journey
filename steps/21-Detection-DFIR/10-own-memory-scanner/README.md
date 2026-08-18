# 21-10 · Own memory scanner — kernel structures from a raw dump (stretch)

**Week:** W36+ stretch · **Track:** M · **Prev:** [`../09-own-yara-engine`](../09-own-yara-engine/README.md)

## Objective
01 rides Volatility's API. Walk the kernel yourself: from a raw memory dump, find kernel base (signature scan), walk the PID list / process structures, list modules and sockets — the primitive Volatility wraps. Then compare your output against Volatility's on the same dump (the honesty check).

## Tasks
- [ ] Dump acquisition + parsing: raw vs page-table walk (KASLR — scan for kernel signature, then translate via CR3/PML4); one OS first (Linux or Windows VM dump)
- [ ] Structure walking: task/PID list from kernel global (linux: `init_task` task_struct list; win: PsActiveProcessHead EPROCESS), module list; offsets verified against the exact kernel build (pairs 15-01 Ghidra for struct offsets)
- [ ] Sockets/files: one more structure level (files or netns) — enough to identify a C2 beacon's PID from 27-06
- [ ] Validation: same dump → your output vs Volatility (linux_pslist / pslist) — field-level diff log; every divergence explained (that's the lesson)
- [ ] Self-check: your scanner identifies the C2 process (27-06) from its dump; parity diff < documented threshold

## Resources
- Volatility source (the reference); your 01-memory-forensics notes; kernel struct docs (win: vergilius project; linux: your 05 kernel reading)

## Exit Criteria
- [ ] Scanner lists processes/modules from a dump, parity-diffed vs Volatility — `labs/`
- [ ] C2 PID identified from own dump — `labs/`

## Links
- [Volatility](https://github.com/volatilityfoundation/volatility3)
- [Vergilius (Win structs)](https://www.vergiliusproject.com/)
