# 01-08 · xv6-riscv — Lab fs

**Week:** W8 · **Track:** A · **Prev:** [`../07-lab-thread`](../07-lab-thread/README.md) · **Next:** [`../09-lab-net-e1000`](../09-lab-net-e1000/README.md)

## Objective
On-disk filesystem structure and syscall-level filesystem semantics.

## Tasks
- [ ] `bigfile` → extend inode max size (direct + indirect blocks)
- [ ] `symlink` — `O_NOFOLLOW`, symlink resolution in `sys_open`
- [ ] Read `kernel/fs.c`, `kernel/bio.c`, `kernel/log.c` (crash recovery)

## Resources
- MIT 6.S081 lab instructions (fs)
- xv6 book ch.8 (File system)

## Exit Criteria
- [ ] `make grade` fs tests pass
- [ ] Sketch inode/block-layer layout — `notes/`

## Links
- [6.S081 fs lab](https://pdos.csail.mit.edu/6.S081/2023/labs/fs.html)
- [Linux VFS docs](https://docs.kernel.org/filesystems/vfs.html)
