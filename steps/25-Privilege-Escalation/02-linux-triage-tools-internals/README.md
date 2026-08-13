# 25-02 · Linux triage tools — how linpeas / linenum / pspy actually work

**Week:** W20+ parallel · **Track:** Q · **Prev:** [`../01-privilege-boundary-model`](../01-privilege-boundary-model/README.md) · **Next:** [`../03-linux-suid-capabilities-sudo`](../03-linux-suid-capabilities-sudo/README.md)

## Objective
Stop treating PEASS-ng as a black box. Understand what linpeas enumerates and *why* those checks map to the boundary model, then build a mini-linpeas in bash that finds a real misconfig.

## Tasks
- [ ] Read PEASS-ng source: categorize every check — filesystem (writable paths, `.ssh`, cron, systemd units), metadata (setuid bits, capabilities, world-writable files), config (`sudo -l` parse, `/etc/sudoers.d`, passwd/shadow perms), runtime (running services, listening ports, users)
- [ ] pspy internals: why it works — inotify + polling `/proc/*/fd` and `cmdline` for short-lived processes cron never shows in ps
- [ ] suid3num/linEnum: what they check that linpeas doesn't
- [ ] gtfobins/lolbas model: binary + capability combo → escalation primitive; learn the *format*, then derive 3 entries yourself
- [ ] Write `code/mini_linpeas.sh` (~100 lines): SUID scan, world-writable dirs, sudo -l output, writable cron/systemd units, `.ssh` readable, docker/lxc group membership, capabilities via `getcap -r /`
- [ ] Run it in a deliberately-vulnerable VM (Vulnhub `basic_pentesting`, HTB `Starting Point` boxes, or a local docker user), find the path to root

## Resources
- PEASS-ng source (bash, readable)
- pspy source (Go)
- Vulnhub basic_pentesting — the classic first box

## Exit Criteria
- [ ] mini_linpeas.sh runs and flags a real misconfig on a target VM — `code/`
- [ ] Writeup: the check → the boundary crossed → the primitive — `notes/`
- [ ] Explain in ≤5 lines why pspy sees processes `ps` misses — `notes/`

## Links
- [PEASS-ng (linpeas/winpeas)](https://github.com/peass-ng/PEASS-ng)
- [pspy](https://github.com/DominicBreuker/pspy)
- [GTFOBins](https://gtfobins.github.io/)
- [LOLBAS](https://lolbas-project.github.io/)
- [HackTricks Linux privesc](https://book.hacktricks.xyz/linux-hardening/privilege-escalation)
