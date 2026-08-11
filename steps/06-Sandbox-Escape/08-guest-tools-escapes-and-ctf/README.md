# 06-08 · Guest-Tools / Shared-Surface Escapes + CTF Practice

**Week:** W38–40 · **Track:** A · **Prev:** [`../07-device-emulation-escapes`](../07-device-emulation-escapes/README.md) · **Next:** [`../../07-XNU-iOS-Exploitation/01-checkm8-bootrom`](../../07-XNU-iOS-Exploitation/01-checkm8-bootrom/README.md)

## Objective
Escape through the guest↔host service channels — shared folders, tools RPC, clipboard — and build reps on real VM-escape CTF challenges.

## Tasks
- [ ] Shared-folder bugs: VMware HGFS, VirtualBox shared folders — path traversal / host-file access
- [ ] VMware Tools / open-vm-tools RPC channel; clipboard & drag-drop services
- [ ] Parallels on macOS: shared folders + hypervisor boundary (relates to your Apple track)
- [ ] Port and solve QEMU VM-escape CTF challenges (e.g., hxp, Insomnihack, WCTF) — reuse your Phase 4–5 exploit stack
- [ ] One full end-to-end escape chain (guest → host) written up — `notes/`

## Resources
- P0 / MWR guest-tools research
- VMware HGFS + open-vm-tools source
- QEMU escape CTF writeups (hxp, Insomnihack)

## Exit Criteria
- [ ] ≥1 CTF/known VM escape exploited end-to-end — `labs/`
