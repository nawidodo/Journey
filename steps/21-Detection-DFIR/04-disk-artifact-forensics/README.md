# 21-04 · Disk + Artifact Forensics

**Week:** W34–35 · **Track:** M · **Prev:** [`../03-detect-as-code-sigma-yara`](../03-detect-as-code-sigma-yara/README.md) · **Next:** [`../05-capstone-ir-scenario`](../05-capstone-ir-scenario/README.md)

## Objective
The persistence side of the story: what survives reboot on disk, and how to find it. Complements Track J (persistence construction) with the inversion.

## Tasks
- [ ] Filesystem artifacts: timeline analysis (MAC times), slack/unallocated, deleted-file recovery, $MFT/NTFS vs ext4 vs APFS
- [ ] Persistence hunting: Run keys, services, scheduled tasks, WMI, LaunchAgents/Daemons, systemd units, cron, authorized_keys — the same list Track J/J 19-02 build from, now searched
- [ ] Browser/OS artifacts: prefetch, Amcache, ShimCache, login history, thumbnails — what a session reveals
- [ ] Carving: recover deleted files from a raw image (`forensic-archive-carve`/`binwalk`-class tools); hash + validate what you find
- [ ] Forensics hygiene: image first, work copies, chain of custody notes — you're building evidence, not a sandbox

## Resources
- *Practical Forensic Imaging* (Bruce Nikkel) or equivalent; Sleuth Kit / Autopsy docs; Velociraptor artifacts
- Windows artifact knowledge base (ForensicsWiki, DFIR: it's not that simple); Apple unified log guides

## Exit Criteria
- [ ] Full artifact timeline from one lab image (login → malware → persistence → exfil) — `labs/`
- [ ] Carved + recovered file with integrity hash — `labs/`

## Links
- [Sleuth Kit](https://github.com/sleuthkit/sleuthkit)
- [Velociraptor](https://github.com/Velocidex/velociraptor)
