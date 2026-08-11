# 01-05 · xv6-riscv — Lab traps

**Week:** W6 · **Track:** A · **Prev:** [`../04-lab-pgtbl-sv39`](../04-lab-pgtbl-sv39/README.md) · **Next:** [`../06-lab-cow`](../06-lab-cow/README.md)

## Objective
Trap handling and register-context surgery.

## Tasks
- [ ] `backtrace` — walk frame pointers in kernel
- [ ] `alarm` — periodic timer via trapframe manipulation (`sa_handler` after trap return)
- [ ] Read `kernel/trap.c`, `kernel/trampoline.S`, `kernel/trapasm.S`

## Resources
- MIT 6.S081 lab instructions (traps)
- xv6 book ch.4

## Exit Criteria
- [ ] `make grade` traps tests pass
- [ ] Explain `userret`/`trampoline` flow in `notes/`
