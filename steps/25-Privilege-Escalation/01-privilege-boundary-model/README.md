# 25-01 · Privilege boundary model — how each OS defines "higher privilege"

**Week:** W20+ parallel · **Track:** Q · **Prev:** — · **Next:** [`../02-linux-triage-tools-internals`](../02-linux-triage-tools-internals/README.md)

## Objective
Map the privilege boundary of every OS before touching any tool — every privesc tool is a lever against a boundary defined here. If you can't name the boundary, you can't read the tool.

## Tasks
- [ ] Windows: SIDs, ACLs/DACL-SACL, integrity levels (Low/Medium/High/System), tokens (primary vs impersonation), the privilege list (`SeImpersonate`, `SeDebug`, `SeAssignPrimaryToken`, `SeBackup`, `SeLoadDriver`, `SeRestore`)
- [ ] Linux: cred struct, real/effective/saved UID, setuid bit semantics, capabilities (why `cap_setuid` ≈ root), LSM (SELinux/AppArmor), namespaces + cgroups as boundaries
- [ ] macOS: TCC (Transparency, Consent & Control) database, SIP/rootless, entitlements vs code signing, task_for_pid + Mach ports, launchd as service root
- [ ] Android: SELinux domains, gids/`gid=root` tricks, Zygote, su daemon model, verified boot/AVB
- [ ] iOS: sandbox profiles, entitlements, code signing chain, SEP/secure enclave, PAC/APRR
- [ ] Draw the boundary map in `notes/` — one diagram per OS, label what sits "above" the boundary (root/SYSTEM/TCC-approval) and which primitives cross it
- [ ] For each boundary, name one real tool that crosses it (write the list in `notes/`)

## Resources
- HackTricks per-OS hardening/privilege-escalation sections
- Microsoft security docs — access tokens, privileges, integrity levels
- man capabilities(7), man 7 user_namespaces, kernel `Documentation/security/`
- Apple Platform Security Guide (TCC/SIP/entitlements/SEP)
- AOSP security docs (SELinux, verified boot)
- Phase 4 (`04-Real-Kernels-Linux-XNU`) security reading overlaps here

## Exit Criteria
- [ ] Boundary map drawn from memory for all 6 OSes — `notes/`
- [ ] For 5 of the 6 OSes, name the exact privilege that is the usual escalation goal (root, SYSTEM, TCC approval, etc.) and one tool that achieves it — `notes/`
- [ ] Explain in ≤5 lines why `SeImpersonatePrivilege` on Windows and `cap_setuid` on Linux are equivalent escalation primitives — `notes/`

## Links
- [HackTricks Linux privesc](https://book.hacktricks.xyz/linux-hardening/privilege-escalation)
- [HackTricks Windows privesc](https://book.hacktricks.xyz/windows-hardening/windows-local-privilege-escalation)
- [HackTricks macOS privesc](https://book.hacktricks.xyz/macos-hardening/macos-security-and-privilege-escalation)
- [Windows access tokens (MS)](https://learn.microsoft.com/en-us/windows/win32/secauthz/access-tokens)
- [capabilities(7)](https://man7.org/linux/man-pages/man7/capabilities.7.html)
- [Apple Platform Security Guide](https://support.apple.com/guide/security/welcome/web)
- [AOSP security](https://source.android.com/docs/security)
