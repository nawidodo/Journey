# 24-81 · Own unikernel — your OS without an OS (stretch)

**Week:** W46+ opt (stretch) · **Track:** P · **Prev:** [`../80-own-packet-replayer`](../80-own-packet-replayer/README.md) · **Pairs:** 24-01, 24-13, 02-11

## Objective
Your 24-01 kernel boots into a multitasking OS; a unikernel is the other end: one app linked directly to kernel bits, no process model. Build a unikernel-lite: choose a target (x86_64 on QEMU, or your 02-11 RISC-V), boot (24-31 bootloader or multiboot-lite), minimal memory setup, your 24-05 network stack as the only interface, and a single app (your 24-17 HTTP server or 24-19 DNS) as the whole image. The payoff: boot time, image size, attack surface — measured against your 24-01 OS (the table, pairs 24-30).

## Tasks
- [ ] Boot path: Multiboot/QEMU direct boot, GDT/IDT/paging minimal (24-01/24-31 reuse), console
- [ ] Harness: link your app + minimal runtime (no libc: your own malloc 24-16, printf-lite) into one image
- [ ] Stack: bring your 24-05 TCP/IP + 24-17 HTTP as the only services; serve one request end-to-end
- [ ] Measurement: image size / boot-to-request time / RSS vs your 24-01 OS + 24-13 container (the honest table)
- [ ] Writeup: why unikernels exist (cloud/microVMs — pairs 24-48), the security tradeoff (single address space) — `notes/`

## Resources
- Unikraft/Solo5 docs (the manual); your 24-01/24-05/24-17/24-31 code

## Exit Criteria
- [ ] Single-app unikernel boots and serves on QEMU — `labs/` + `code/`
- [ ] Size/time/attack-surface comparison table — `labs/` + `notes/`

## Links
- [Solo5](https://github.com/Solo5/solo5)
- [Unikraft](https://unikraft.org/)