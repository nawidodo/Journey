# 01-06 · xv6-riscv — Lab cow

**Week:** W6–7 · **Track:** A · **Prev:** [`../05-lab-traps`](../05-lab-traps/README.md) · **Next:** [`../07-lab-thread`](../07-lab-thread/README.md)

## Objective
Copy-on-write `fork` — page-fault-driven memory management. Direct conceptual bridge to later kernel exploitation (UAF, refcounting).

## Tasks
- [ ] PTE flag `PTE_COW` + per-page refcount
- [ ] `fork` maps shared, read-only pages
- [ ] Handle write faults in `usertrap` → alloc + copy + fix PTE
- [ ] Free only at refcount 0

## Resources
- MIT 6.S081 lab instructions (cow)
- xv6 book ch.4 (page-fault handling)

## Exit Criteria
- [ ] `make grade` cow tests pass
- [ ] Write up refcount + COW fault flow — `notes/`

## Links
- [6.S081 cow lab](https://pdos.csail.mit.edu/6.S081/2023/labs/cow.html)
- [Linux COW (kernel docs)](https://docs.kernel.org/mm/copy_page.html)
