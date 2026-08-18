# 04-09 · FreeBSD deep dive — jails, ZFS, DTrace, pf (the missing popular OS)

**Week:** W14–20 parallel · **Track:** A · **Prev:** [`../08-rust-for-systems-programming`](../08-rust-for-systems-programming/README.md)

## Objective
Every popular desktop/mobile OS except FreeBSD is covered in this plan — and FreeBSD is the missing piece with outsized influence: jails predate containers, ZFS is the forensic goldmine, DTrace is the original dynamic tracing (the probe model your eBPF 05-14 tool recreated), pf is a real firewall. Run it in a VM: internals reading → jails → ZFS → pf → DTrace; then the security labs.

## Tasks
- [ ] Setup + internals: install FreeBSD (VM), ports/pkg, kernel layout vs Linux (the table: process, VM, IPC, fs); read the design docs
- [ ] Jails: create jails (the container ancestor — pairs 06 sandbox escapes + 24-13 own container); jail escape history (CVE-2020-25560-shaped thinking), mount restrictions
- [ ] ZFS: pools, snapshots, send/recv; forensic angle — snapshot-based forensics, deleted-file recovery at filesystem level (pairs 21-04 disk artifacts, 24-15 own-filesystem)
- [ ] pf + DTrace: firewall rules; DTrace one-liners tracing syscalls/opens (compare vs eBPF 05-14), the probe-model lesson
- [ ] Security lab: jail breakout attempt fails (own VM); ZFS rollback as incident-recovery primitive — `labs/`

## Resources
- FreeBSD Handbook + Design Elements; the FreeBSD Architecture Handbook; FreeBSD security advisories; your 06/24-13/21-04 notes

## Exit Criteria
- [ ] Jails + ZFS + pf + DTrace working in VM — `labs/`
- [ ] Escape-attempt + ZFS-forensics writeup — `labs/` + `notes/`

## Links
- [FreeBSD Handbook](https://docs.freebsd.org/en/books/handbook/)
- [FreeBSD Architecture Handbook](https://docs.freebsd.org/en/books/arch-handbook/)
