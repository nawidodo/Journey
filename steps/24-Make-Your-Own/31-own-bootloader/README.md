# 24-31 · Own bootloader — BIOS + UEFI paths, the firmware→kernel handoff (stretch)

**Week:** W46+ opt (stretch) · **Track:** P · **Prev:** [`../30-own-sampling-profiler`](../30-own-sampling-profiler/README.md)

## Objective
The most hardware-adjacent software there is: what runs before your OS. Build a bootloader: real-mode BIOS path (MBR-style, 16-bit, int 13h) and a UEFI path (PE/COFF app, GOP graphics), GDT/IDT setup, long-mode transition, then hand off to your own 24-01 kernel. The security payoff: this is the bootkit seat (pairs 18-04 UEFI bootkit) — Secure Boot / BootGuard / signed-firmware chain is the defense you'll understand from the writer's side.

## Tasks
- [ ] BIOS path: 16-bit real mode, MBR, int 13h disk read, A20, GDT + protected mode → long mode; load your kernel at 1MB
- [ ] UEFI path: a PE/COFF EFI app, boot services, GOP framebuffer, LoadImage/StartImage — hand off with a memory map
- [ ] Handoff contract: kernel entry conventions (multiboot-ish or your own struct), memory map, framebuffer info — your 24-01 kernel consumes it
- [ ] Security lab: bootkit placement — modify your own bootloader to hook a syscall / hide code, detect it with a Secure-Boot-style signature check (pairs 18-04) — `labs/`
- [ ] Self-check: boots your kernel on QEMU (both paths), and on real hardware (USB/VM with OVMF) if available

## Resources
- OSDev wiki (the manual); UEFI spec; your 24-01/24-10 + 18-04 notes

## Exit Criteria
- [ ] Kernel boots via both BIOS and UEFI paths — `labs/`
- [ ] Bootkit placement + signature-detection demo — `labs/` + `notes/`

## Links
- [OSDev wiki](https://wiki.osdev.org/)
- [UEFI spec](https://uefi.org/specifications)
