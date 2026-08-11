# 01-07 · xv6-riscv — Lab thread

**Week:** W7 · **Track:** A · **Prev:** [`../06-lab-cow`](../06-lab-cow/README.md) · **Next:** [`../08-lab-fs`](../08-lab-fs/README.md)

## Objective
Threads, scheduling, and locking — foundation for `cred`/race reasoning later.

## Tasks
- [ ] `uthread` — user-level context switch (`thread_switch`)
- [ ] `ph` — fix race in parallel hashing with locks
- [ ] Read `kernel/proc.c` (scheduler loop), `kernel/spinlock.c`

## Resources
- MIT 6.S081 lab instructions (thread)
- xv6 book ch.5 (Scheduling), ch.6 (Locking)
- OSTEP concurrency chapters (parallel reading)

## Exit Criteria
- [ ] `make grade` thread tests pass
- [ ] Explain a context switch in `notes/`

## Links
- [6.S081 thread lab](https://pdos.csail.mit.edu/6.S081/2023/labs/thread.html)
- [xv6 book ch.7 (scheduling)](https://pdos.csail.mit.edu/6.828/2023/xv6/book-riscv-rev3.pdf)
