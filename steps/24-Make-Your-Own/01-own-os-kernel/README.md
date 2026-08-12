# 24-01 · Own x86-64 OS Kernel (from scratch, on qemu)

**Week:** W46+ (parallel, optional, stretch) · **Track:** P · **Prev:** — · **Next:** [`../02-own-database-storage-engine`](../02-own-database-storage-engine/README.md)

## Objective
Boot a bare-metal x86-64 kernel you wrote: real mode → protected/long mode, interrupts, virtual memory, a tiny scheduler — a "clean canvas" that makes xv6 (Phase 1) and every later kernel phase click from the hardware side up. Drop it if it stalls — Track A is the priority.

## Tasks
- [ ] Toolchain + boot: linker script, multiboot/limine entry, bare-metal C, print to VGA — `code/`
- [ ] GDT/IDT: switches to long mode, one working interrupt handler — `code/`
- [ ] Paging: identity-map + a mapped user page; `ecall`-analog IRQ round-trip — `code/`
- [ ] Tiny scheduler: round-robin over 2–3 kernel threads + a print spinlock — `code/`
- [ ] Stretch: one syscall + a user process; or one uart driver — `code/`
- [ ] Debrief 1/2 page: what xv6/Linux now looks less mysterious — `notes/`

## Resources
- OSDev Wiki (gold), *osdev* / "Writing an OS in Rust" (concepts, language-agnostic), tinyrun/ bkerndev-class kernels
- Your Phase 0/1 toolchain + qemu (already used in 01-xv6)

## Exit Criteria
- [ ] Kernel boots on qemu, prints, handles one interrupt, schedules 2 threads — `code/`
- [ ] Debrief note — `notes/`

## Links
- [OSDev Wiki](https://wiki.osdev.org/Main_Page)
- [build-your-own-x: own OS](https://github.com/codecrafters-io/build-your-own-x?tab=readme-ov-file#build-your-own-operating-system) (canonical OS catalog)
- [Write your own x86 bootable kernel](https://github.com/dhavalhirdhav/Write-your-own-x86-bootloader-and-kernel) (start-point)