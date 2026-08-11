# 01-04 · xv6-riscv — Lab pgtbl (Sv39)

**Week:** W5–6 · **Track:** A · **Prev:** [`../03-lab-syscall`](../03-lab-syscall/README.md) · **Next:** [`../05-lab-traps`](../05-lab-traps/README.md)

## Objective
RISC-V Sv39 three-level page tables; virtual memory from the inside.

## Tasks
- [ ] `vmprint` — walk and print a kernel page table
- [ ] `kvminit/kvmalloc` — kernel page table per process (`proc.pagetable`)
- [ ] `uaccess` — copyin/copyout through user PTEs
- [ ] Read `kernel/vm.c`, `kernel/memlayout.h`, `kernel/riscv.h` (PTE flags, satp)

## Resources
- MIT 6.S081 lab instructions (pgtbl)
- xv6 book ch.2 (Page tables) + ch.3

## Exit Criteria
- [ ] `make grade` pgtbl tests pass
- [ ] Draw the Sv39 walk (VPN → PTE → physical) — `notes/`
