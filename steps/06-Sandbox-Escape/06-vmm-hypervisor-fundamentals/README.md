# 06-06 · VMM / Hypervisor Fundamentals

**Week:** W37–38 · **Track:** A · **Prev:** [`../05-ios-macos-sandbox-escape-techniques`](../05-ios-macos-sandbox-escape-techniques/README.md) · **Next:** [`../07-device-emulation-escapes`](../07-device-emulation-escapes/README.md)

## Objective
Know the virtualization boundary cold: where guest code meets host code, and every channel between them. **Targets: VMware, VirtualBox, Parallels (macOS), QEMU.**

## Tasks
- [ ] Virtualization models: type-1 vs type-2; VMX/SVM; QEMU+TCG vs QEMU+KVM; Apple Hypervisor.framework (Parallels/UTM on macOS)
- [ ] Emulated device surface: virtio (virtqueue rings), VMware SVGA II, VirtualBox device model
- [ ] Guest-tools channels: VMware Tools / open-vm-tools (RPC, HGFS), VirtualBox Guest Additions, Parallels Tools — each is a guest→host entry point
- [ ] Map the full attack surface (devices, PCI config, tools RPC, shared folders, clipboard) — `notes/`
- [ ] Run the same guest in QEMU and one of VirtualBox/VMware/Parallels; enumerate exposed PCI devices + services — `labs/`

## Resources
- Intel SDM vol.3 (VMX) / AMD manual (SVM)
- QEMU docs + source (`hw/`)
- VMware / VirtualBox / Parallels manuals (tools, shared folders)
- Apple Hypervisor framework docs

## Exit Criteria
- [ ] Guest→host attack-surface map with every channel annotated — `notes/`
