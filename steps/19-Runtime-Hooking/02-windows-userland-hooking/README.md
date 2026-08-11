# 19-02 · Windows Userland Hooking

**Week:** W21–23 · **Track:** K · **Prev:** [`../01-hooking-fundamentals`](../01-hooking-fundamentals/README.md) · **Next:** [`../03-linux-hooking`](../03-linux-hooking/README.md)

## Objective
The malware-dev hooking layer — directly feeds Track E's AMSI/ETW bypasses.

## Tasks
- [ ] IAT patcher from scratch: parse PE headers, walk import descriptors, patch + flush cache
- [ ] Own x64 detour engine: absolute-jump encoder, trampoline (original bytes + jump back), hook list, thread-safe patch/unhook
- [ ] EAT patcher from scratch (export table)
- [ ] VEH/breakpoint hook from scratch (INT3 → handle in VEH); hotpatching (5-byte NOP region)
- [ ] MinHook/Detours as comparison only: diff your engine against theirs
- [ ] Detect your own hook from another process (read-only memory scan)
- [ ] Kernel cross-ref: SSDT/minifilter — where EDR hooks (Track J)

## Resources
- MinHook; Microsoft Detours; ired.team hooking; your Track E VM

## Exit Criteria
- [ ] Own engine: IAT + detour demos + detection — `code/` + `labs/`

## Links
- [MinHook](https://github.com/TsudaKageyu/minhook)
- [Detours (Microsoft)](https://github.com/microsoft/Detours)
