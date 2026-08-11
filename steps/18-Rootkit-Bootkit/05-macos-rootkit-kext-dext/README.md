# 18-05 · macOS Rootkit (kext + dext)

**Week:** W51–53 · **Track:** J · **Prev:** [`../04-windows-uefi-bootkit`](../04-windows-uefi-bootkit/README.md) · **Next:** [`../06-mobile-rootkit-bootkit`](../06-mobile-rootkit-bootkit/README.md)

## Objective
What a macOS rootkit looks like in the DriverKit era — and the Secure Enclave limits.

## Tasks
- [ ] Legacy kext rootkit on an Intel VM (deprecated but instructive): syscall-table patching in kernelcache
- [ ] Modern: dext as persistence carrier; Endpoint Security Framework evasion vs use
- [ ] Hide process/file; persistence in LaunchDaemons / kernel extensions
- [ ] Limits: SSV, Secure Enclave, AMFI, notarization — what can't be persisted
- [ ] Detect: ESF telemetry, baseline/mtree, XProtect behaviour

## Resources
- Levin Vol 2; Apple Platform Security Guide (SSV, ESF); kext/dext docs; your Phase 4 notes

## Exit Criteria
- [ ] macOS rootkit (Intel VM) + ESF detection notes — `labs/` + `notes/`