# 03-04 · Linux Kernel — Read Drivers: e1000 / virtio-net

**Week:** W17–18 · **Track:** A · **Prev:** [`../03-write-linux-module`](../03-write-linux-module/README.md) · **Next:** [`../05-arm64-basics-azeria`](../05-arm64-basics-azeria/README.md)

## Objective
Production-grade NIC drivers — direct upgrade of your xv6 E1000 work.

## Tasks
- [ ] `drivers/net/ethernet/intel/e1000/` — `e1000_main.c`, `e1000_hw.c`
- [ ] `drivers/net/virtio_net.c` — virtqueue ring, kicks
- [ ] NAPI, interrupts, DQL — compare to xv6's polling/ring model
- [ ] Map xv6 ring vs Linux ring in `notes/`

## Resources
- torvalds/linux (drivers/net)
- Intel 82540EM datasheet (from Phase 1 net lab)
- virtio spec 1.0 (virtio-net)

## Exit Criteria
- [ ] Notes: xv6 E1000 vs Linux e1000 vs virtio-net — `notes/`
