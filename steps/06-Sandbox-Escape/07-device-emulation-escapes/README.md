# 06-07 · Device-Emulation Escapes 🚩 S2

**Week:** W37–39 · **Track:** A · **Prev:** [`../06-vmm-hypervisor-fundamentals`](../06-vmm-hypervisor-fundamentals/README.md) · **Next:** [`../08-guest-tools-escapes-and-ctf`](../08-guest-tools-escapes-and-ctf/README.md)

## Objective
Exploit bugs in emulated hardware — the classic VM-escape class. **New checkpoint S2.**

## Method (per escape)
1. Read the writeup — identify the vulnerable emulated device + the bug class
2. Reproduce in an isolated VM environment (never production hardware)
3. Close the POC; re-derive
4. Own walkthrough in `notes/`

## Ladder
- [ ] VirtualBox 3D / Host-GPU escapes (Niklas Baumstark / Project Zero series)
- [ ] VMware SVGA escape (Saar Amar & Alex Plaskett, MWR)
- [ ] QEMU device escapes — history: VENOM (CVE-2015-3456, FDC) → Phoenix Talon (CVE-2018-xxxx, net device bug class)
- [ ] virtio virtqueue bugs (QEMU)

## Resources
- Project Zero blog — "Exploiting virtual devices" series
- MWR / ZDI writeups (SVGA, VirtualBox Host-GPU)
- QEMU bug trackers + `hw/` source

## Exit Criteria
- [ ] **S2: one device-emulation escape re-derived from scratch** — `labs/`
