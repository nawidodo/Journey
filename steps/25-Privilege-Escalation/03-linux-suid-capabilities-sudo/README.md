# 25-03 · Linux SUID / capabilities / sudo — mechanism deep-dives

**Week:** W20+ parallel · **Track:** Q · **Prev:** [`../02-linux-triage-tools-internals`](../02-linux-triage-tools-internals/README.md) · **Next:** [`../04-linux-service-cron-path-kernel`](../04-linux-service-cron-path-kernel/README.md)

## Objective
The three classic Linux primitives, at the level where you can predict the exploit from the binary's properties.

## Tasks
- [ ] SUID: why `setuid` bit ⇒ euid changes to owner; what changes in the cred struct (ruid/euid/suid/fsuid); why bash drops it and `/bin/sh` (dash) doesn't — and how that bit of trivia IS the exploit
- [ ] SUID binary attacks: shared-object hijack (ldd → writable dep → code as owner), `LD_PRELOAD` (why modern setuid ignores it — secure-execution mode), PATH hijack for `system()` calls, `cp`/`tar`/`find` -exec primitives, gtfobins derivation
- [ ] Capabilities: `getcap -r /`, what `cap_setuid+ep` means, why it equals root for escalation, ambient vs file caps
- [ ] sudo: every misconfig class — NOPASSWD entries, `env_keep` + `LD_PRELOAD`, `(ALL:ALL)` vs `(root)` semantics, writable sudoers.d, `sudo -l` full parse, sudoedit path tricks, CVE-2021-3156 (Baron Samedit) as a real bug in the parser
- [ ] Build `code/` lab: a deliberately vulnerable SUID binary (C, 20 lines) + 3 exploitation routes, each ending in a shell as the owner
- [ ] Re-derive one real CVE from Phase 5's ladder if it touches these (modprobe_path overlaps)

## Resources
- man credentials(7), man capabilities(7), man ld.so (secure-execution)
- ired.team Linux privesc labs
- HackTricks SUID/cap/sudo sections

## Exit Criteria
- [ ] Vulnerable SUID binary + 3 routes to root in the lab, writeup with gdb traces — `labs/`
- [ ] Explain in ≤5 lines the exact reason `LD_PRELOAD` fails on setuid binaries and the two ways it still works — `notes/`
- [ ] Predict (not test) which of 5 given binaries is exploitable via caps vs suid — `notes/`

## Links
- [HackTricks SUID](https://book.hacktricks.xyz/linux-hardening/privilege-escalation#suid-binaries)
- [HackTricks capabilities](https://book.hacktricks.xyz/linux-hardening/privilege-escalation#capabilities)
- [ired.team privesc](https://www.ired.team/offensive-security/privilege-escalation)
- [CVE-2021-3156 writeup](https://blog.qualys.com/vulnerabilities-threat-research/2021/01/26/cve-2021-3156-heap-based-buffer-overflow-in-sudo-baron-samedit)
