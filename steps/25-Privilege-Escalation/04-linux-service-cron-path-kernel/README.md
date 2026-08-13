# 25-04 · Linux service / cron / path / container / kernel — the rest of the surface

**Week:** W20+ parallel · **Track:** Q · **Prev:** [`../03-linux-suid-capabilities-sudo`](../03-linux-suid-capabilities-sudo/README.md) · **Next:** [`../05-windows-token-model`](../05-windows-token-model/README.md)

## Objective
Everything linpeas flags that isn't SUID/cap/sudo — the misconfig classes and when they beat a kernel exploit.

## Tasks
- [ ] Cron: writable cron scripts, cron PATH manipulation, wildcard injection (`tar -czf *`), systemd timers as the modern equivalent
- [ ] systemd: writable unit files, `User=`/`ExecStart` abuse, unit drop-ins, socket-activated services
- [ ] PATH: writable dir earlier in PATH than the real binary, script shebang tricks
- [ ] Container/group escapes: docker group (socket ⇒ root), lxc, `nsenter`, `--privileged` + cgroup release_agent escape; link Phase 6 (container escapes S1)
- [ ] NFS: `root_squash` off, no_root_squash ⇒ write to NFS as UID 0, SUID files over NFS
- [ ] Kernel: when to go kernel vs config — the modprobe_path primitive, Dirty Pipe (CVE-2022-0847) read/write mechanics, why kernel exploits are last resort on patched boxes; defer depth to Phase 5 ladder
- [ ] Lab: chain 3 of the above into one escalation (e.g., writable cron → root cron → drop SUID root shell), scripted and documented — `labs/`
- [ ] Detection mirror: which of these does a defender see in `auditd`/systemd journal — note in `notes/` (feeds Track M)

## Resources
- systemd.unit man pages
- HackTricks Linux privesc misc sections
- Phase 6 `03-container-escape-runc-cves` for the container depth

## Exit Criteria
- [ ] 3-class chain to root on a lab VM with artifacts — `labs/`
- [ ] Matrix in `notes/`: misconfig class → detection signal → which blue-team tool sees it
- [ ] Explain in ≤5 lines how cgroup `release_agent` gives root from inside a privileged container — `notes/`

## Links
- [HackTricks Linux privesc](https://book.hacktricks.xyz/linux-hardening/privilege-escalation)
- [Dirty Pipe writeup](https://dirtypipe.cm4all.com/)
- [GTFOBins](https://gtfobins.github.io/)
