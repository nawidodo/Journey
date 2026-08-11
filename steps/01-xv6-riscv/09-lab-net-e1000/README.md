# 01-09 · xv6-riscv — Lab net: E1000 driver 🚩 M2

**Week:** W8–9 · **Track:** A · **Prev:** [`../08-lab-fs`](../08-lab-fs/README.md) · **Next:** [`../10-stretch-tcp-echo-virtio-net`](../10-stretch-tcp-echo-virtio-net/README.md)

## Objective
NIC driver: TX/RX descriptor rings, ARP, ICMP. **Your NIC-driver goal from the roadmap. Milestone M2.**

## Tasks
- [ ] E1000 init: MMIO BAR, `E1000_RCTL/TCTL`, descriptor rings
- [ ] TX path: `mbuf` → descriptor → transmit
- [ ] RX path: receive descriptor → `e1000_recv` → `net_rx`
- [ ] ARP resolution + ICMP reply
- [ ] `ping` the xv6 guest from the host

## Resources
- MIT 6.S081 lab instructions (net)
- Intel 82540EM/8254x datasheet (E1000 registers)
- xv6 book ch.9? → actual: lab spec + `kernel/e1000.c` skeleton
- *TCP/IP Illustrated* V1 (ARP/ICMP chapters)

## Exit Criteria
- [ ] **M2: xv6 answers `ping`**
- [ ] Packet path diagram (mbuf → ring → wire) — `notes/`

## Links
- [6.S081 net lab](https://pdos.csail.mit.edu/6.S081/2023/labs/net.html)
- [e1000 datasheet (Intel)](https://www.intel.com/content/dam/www/public/us/en/documents/manuals/pci-pci-x-family-gbe-controllers-software-dev-manual.pdf)
