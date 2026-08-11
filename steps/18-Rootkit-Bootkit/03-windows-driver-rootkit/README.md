# 18-03 · Windows Driver Rootkit

**Week:** W47–49 · **Track:** J · **Prev:** [`../02-linux-lkm-ebpf-rootkit`](../02-linux-lkm-ebpf-rootkit/README.md) · **Next:** [`../04-windows-uefi-bootkit`](../04-windows-uefi-bootkit/README.md)

## Objective
Kernel rootkit on Windows — callbacks, DKOM, minifilter. Reuses Track D (kernel) + Track G (KMDF driver craft).

## Tasks
- [ ] Kernel callbacks (`PsSetCreateProcessNotifyRoutine`, etc.) for hide-on-create
- [ ] DKOM: unlink `EPROCESS`/`ETHREAD` from the active list
- [ ] Minifilter driver: hide files, filter registry (build on your Track G KMDF)
- [ ] Delivery: BYOVD or test-signed driver (Track D's loldrivers context)
- [ ] Detect: KPP/PatchGuard response, custom kernel-memory scanner

## Resources
- *Rootkits* (Hoglund); ired.team; loldrivers.io; your Track D VM

## Exit Criteria
- [ ] Callback + DKOM rootkit on your VM — `labs/`

## Links
- [Rootkits: Subverting the Windows Kernel (book info)](https://www.oreilly.com/library/view/rootkits-subverting-the/0321294319/)
- [MiniFilter samples (Microsoft)](https://github.com/microsoft/Windows-driver-samples)
