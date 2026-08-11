# 01-03 · xv6-riscv — Lab syscall 🚩 M1

**Week:** W5 · **Track:** A · **Prev:** [`../02-lab-util`](../02-lab-util/README.md) · **Next:** [`../04-lab-pgtbl-sv39`](../04-lab-pgtbl-sv39/README.md)

## Objective
Add a syscall; trace the full `ecall` → kernel → return path. **Milestone M1.**

## Tasks
- [ ] `trace` — syscall tracing per-process (with `SYS_*` table)
- [ ] `sysinfo` — collect free memory + process count
- [ ] Write up the full path in `notes/`: user stub → `ecall` → `syscall()` dispatch → handler → return

## Resources
- MIT 6.S081 lab instructions (syscall)
- xv6 book ch.4 (Traps and system calls)

## Exit Criteria
- [ ] `trace 32 grep hello` shows every syscall
- [ ] **M1: explain `ecall` → kernel → return from memory** — `notes/`
