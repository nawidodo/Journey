# 03-02 · Linux Kernel — Read mm/, sched/, net/

**Week:** W15–17 · **Track:** A · **Prev:** [`../01-linux-kernel-dev-ostep`](../01-linux-kernel-dev-ostep/README.md) · **Next:** [`../03-write-linux-module`](../03-write-linux-module/README.md)

## Objective
Read real kernel source where future exploits will live.

## Tasks
- [ ] `mm/` — page allocator (`page_alloc.c`), `slab.c`/`slub.c`, `vmalloc.c`
- [ ] `sched/` — `core.c`, CFS
- [ ] `net/` — `sk_buff.h`, `netdevice.h`, `core/dev.c` (ingress/egress)
- [ ] **Trace an skb from NIC rx to socket** — write it up in `notes/`

## Resources
- torvalds/linux (current stable branch)
- LKD (mm/net chapters)

## Exit Criteria
- [ ] **skb lifecycle from memory** — `notes/`

## Links
- [Linux MM docs](https://docs.kernel.org/mm/index.html)
- [Linux scheduler docs](https://docs.kernel.org/scheduler/index.html)
- [Linux networking docs](https://docs.kernel.org/networking/index.html)
