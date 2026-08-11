# 01-01 · xv6-riscv — Toolchain Setup

**Week:** W4 · **Track:** A · **Prev:** [`../../00-Foundations/04-swift-basics`](../../00-Foundations/04-swift-basics/README.md) · **Next:** [`../02-lab-util`](../02-lab-util/README.md)

## Objective
Build and boot xv6-riscv (MIT 6.S081/6.1810) on macOS.

## Tasks
- [ ] Install RISC-V toolchain (qemu-system-riscv64 + riscv64-elf gcc; on macOS use `brew` packages or the MIT prebuilt riscv-gnu-toolchain)
- [ ] Clone `mit-pdos/xv6-riscv`
- [ ] `make qemu` — boot to shell
- [ ] Run `ls`, `echo`, `forktest`
- [ ] Read `Makefile` (platform, kernel build flags) — notes in `notes/`

## Resources
- MIT 6.S081 / 6.1810 course site + labs
- *The RISC-V Reader*, MIT 6.004 (start: registers, calling convention)
- riscv.org spec (ISA basics)

## Exit Criteria
- [ ] xv6 boots in QEMU on this machine
