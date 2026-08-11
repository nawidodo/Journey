# 01-10 · xv6-riscv — Stretch: TCP echo / virtio-net

**Week:** W9+ · **Track:** A (stretch) · **Prev:** [`../09-lab-net-e1000`](../09-lab-net-e1000/README.md) · **Next:** [`../../03-Exploitation-Fundamentals/01-buffer-overflow-shellcode`](../../03-Exploitation-Fundamentals/01-buffer-overflow-shellcode/README.md)

## Objective
Go deeper on the network stack. Choose one.

## Tasks
- [ ] **Option A — TCP echo on E1000:** minimal TCP accept/recv/send; respond to a TCP handshake + payload
- [ ] **Option B — virtio-net:** write a virtio-net driver using xv6's virtio-blk framework
- [ ] Wire up the device interrupt + `net_tx`/`net_rx` integration

## Resources
- *TCP/IP Illustrated* V1 (TCP state machine)
- xv6 `kernel/virtio_*.c`, virtio spec 1.0 (virtio-net chapter)

## Exit Criteria
- [ ] Host `nc`/curl exchanges data with xv6 over one of the two paths

## Links
- [virtio spec](https://docs.oasis-open.org/virtio/virtio/v1.2/csd01/virtio-v1.2-csd01.html)
- [Linux virtio-net source](https://github.com/torvalds/linux/tree/master/drivers/net/virtio_net.c)
