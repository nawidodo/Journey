# 06-09 · Own Hypervisor (stretch)

**Week:** W38–39 (stretch) · **Track:** A · **Prev:** [`../06-vmm-hypervisor-fundamentals`](../06-vmm-hypervisor-fundamentals/README.md) · **Next:** —

## Objective
Build the guest↔host boundary yourself once — then the escape writeups in 06-07 read like your own code review. Apple Silicon → Hypervisor.framework; Linux VM → KVM.

## Tasks
- [ ] Pick: **Hypervisor.framework on your Mac** (bare-metal VMM on Apple Silicon, Swift/C) or **KVM** in the Dojo VM (or both)
- [ ] Minimum VMM: VM create, vCPU run loop, exception exits, EPT/2nd-stage translation, a serial I/O channel
- [ ] Boot a minimal guest (a tiny kernel or existing image) to a point where it prints
- [ ] Interrupt/virtual-timer handling; one emulated device (UART or virtio-net stub)
- [ ] Map your VMM's trust boundary to 06-06's attack-surface map — `notes/`
- [ ] Stretch²: fuzz your own device stub with 05-11's method

## Resources
- Apple Hypervisor.framework docs + WWDC sessions; OSDev wiki "Virtualization"
- KVM kernel docs + `Documentation/virt/`; QEMU `hw/` source as reference
- "Writing a hypervisor" blog series (e.g., bgeorgel, pwnall)

## Exit Criteria
- [ ] Guest prints from your VMM — `labs/`
- [ ] "Boundary map" (vCPU exit → emulated device → host) — `notes/`
