# 25-10 · macOS privesc — TCC, SIP, task_for_pid, launchd, helper tools

**Week:** W20+ parallel · **Track:** Q · **Prev:** [`../09-ad-domain-privesc`](../09-ad-domain-privesc/README.md) · **Next:** [`../11-android-ios-privesc`](../11-android-ios-privesc/README.md)

## Objective
macOS escalation is a different game — the goal is usually TCC approval or root, and the boundaries are TCC, SIP, entitlements, and the Mach/launchd plumbing.

## Tasks
- [ ] TCC: the `TCC.db`/`kTTBService` permission model, why it's a *trust boundary* above root (root doesn't bypass TCC), the `tccd` daemon, how tools dump it (`tccutil`, sqlite on `~/Library/Application Support/com.apple.TCC`), TCC bypass classes (CVE-2020-9771, CVE-2021-30657, Gatekeeper-style)
- [ ] SIP/rootless: what `/System` protections enforce, `csrutil` flags, how to check `csrutil status`, why SIP blocks DYLD injection into protected binaries
- [ ] task_for_pid: the Mach call that gives you a send right to another process's task port → `mach_vm_read/write`/`thread_create_running` — the macOS "OpenProcess+ReadProcessMemory"; why it requires root or the `com.apple.system-task-ports` entitlement
- [ ] launchd: daemons/agents (`/Library/LaunchDaemons`), writable plists, `ProgramArguments` abuse, `SMJobBless` helper-tool design (bless + `xpc_connection` — the classic local privesc surface, e.g. CVE-2019-8565)
- [ ] DYLD: `DYLD_INSERT_LIBRARIES`/`DYLD_FRAMEWORK_PATH` injection (killed for SIP-protected, works elsewhere), dylib hijacking in un-versioned installs, `codesign` vs entitlement checks
- [ ] sudo + NOPASSWD, writable `/etc/sudoers.d`, root-owned scripts; kext→dext transition (old kernel-module privesc largely dead on Apple Silicon)
- [ ] Tools: LinEnum-macOS ports, Ghostwriter, `macOS-privesc` checkers — read their source, note which checks map to which boundary
- [ ] Lab: build a deliberately-vulnerable `SMJobBless`-style helper (xpc) + client, exploit to root on a spare Mac or VM — `labs/`; if no helper lab, do a full TCC bypass writeup on a spare machine instead

## Resources
- theevilbit blog (TCC research), shenaniganslabs TCC posts
- Apple docs: task_for_pid, SMJobBless, XPC services
- HackTricks macOS sections

## Exit Criteria
- [ ] Helper-tool (or TCC-bypass) lab to root, writeup — `labs/`
- [ ] Explain in ≤5 lines why root doesn't bypass TCC but can still read the TCC database — `notes/`
- [ ] Diagram task_for_pid → task port → code injection chain from memory — `notes/`

## Links
- [HackTricks macOS privesc](https://book.hacktricks.xyz/macos-hardening/macos-security-and-privilege-escalation)
- [theevilbit TCC research](https://theevilbit.github.io/)
- [task_for_pid (MS? no — Apple:)](https://developer.apple.com/documentation/kernel/1578726-task_for_pid)
- [SMJobBless](https://developer.apple.com/documentation/servicemanagement/1431078-smjobbless)
- [CVE-2019-8565 analysis](https://theevilbit.github.io/posts/cve-2019-8565_suid_privesc/)
