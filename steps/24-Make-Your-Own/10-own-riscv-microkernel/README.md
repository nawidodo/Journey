# 24-10 · Own RISC-V microkernel — booting on your own emulator (stretch)

**Week:** W46+ opt (stretch) · **Track:** P · **Prev:** [`../09-own-c-compiler`](../09-own-c-compiler/README.md)

## Objective
The vertical: everything-you-built works together. Minimal RISC-V microkernel (interrupts, syscalls, one scheduler, a task that prints) booting on your own 02-11 rv32ima emulator, built with your own 24-09 compiler + 24-07 assembler. The plan's entire toolchain in one boot.

## Tasks
- [ ] Kernel: RISC-V privileged mode (M→S→U), trap table, PLIC-style timer IRQ, context switch, syscall ABI; boot handoff from a tiny bootloader you write
- [ ] Scheduler: round-robin tasks on your own timer; a task printing via your UART (the emulator's CLINT/UART from 02-11)
- [ ] Integration: compile kernel with your 24-09 compiler (or fall back to stock clang for the arch-specific bits you didn't support) → assemble with 24-07 → load via 02-11 emulator ELF loader → GDB-stub debug (02-11)
- [ ] Compare: why xv6 (01) does it differently; what your microkernel drops (VFS, fork) and why micro vs monolithic
- [ ] Self-check: two tasks round-robin printing, context-switch count logged, reproducible boot

## Resources
- 02-11 emulator (the board); 01-xv6 as the reference OS; RISC-V privileged spec; your 24-09 compiler + 24-07 assembler

## Exit Criteria
- [ ] Microkernel boots on own emulator, scheduler runs ≥2 tasks — `labs/`
- [ ] Boot log + GDB session recorded — `labs/`

## Links
- [RISC-V privileged spec](https://riscv.org/technical/specifications/)
- [xv6](https://pdos.csail.mit.edu/6.828/2023/xv6.html)
