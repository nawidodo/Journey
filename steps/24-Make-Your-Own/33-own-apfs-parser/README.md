# 24-33 · Own APFS parser — containers, fs-tree, snapshots (stretch)

**Week:** W46+ opt (stretch) · **Track:** P · **Prev:** [`../32-own-ntfs-parser`](../32-own-ntfs-parser/README.md) · **Next:** [`../34-own-mail-server`](../34-own-mail-server/README.md)

## Objective
You read XNU but not its filesystem. APFS is macOS/iOS storage: container superblock, object maps, B-tree (your 24-02/24-11 skill), clone/reflink, snapshots. Build a parser over a raw disk image (or a `dd` of a VM volume): container → volume tree → files, snapshot walking. The forensic angle: APFS snapshots + clone-on-write make deleted data *recoverable by design* (Time Machine's trick); pairs 21-04, 04-07, 15-02.

## Tasks
- [ ] Container: NXSB superblock, object map (the indirection layer), checksummed objects (the format's discipline)
- [ ] Volumes: APFS B-trees (reuse your 24-02 B-tree understanding), inode records, extents; read a real file's data
- [ ] Snapshots: snapshot metadata, walk a snapshot tree — diff current vs snapshot (recover "deleted" file = it's in the snapshot)
- [ ] Forensic lab: on a VM image — deleted file recoverable via snapshot; clone/reflink file (the dedup trick) identified; timestamps (pairs 12-08) — `labs/`
- [ ] Writeup: APFS vs NTFS/ext4 design (copy-on-write vs journal) — `notes/`

## Resources
- Apple's APFS reference (the manual); apfs-fuse source (peer); your 24-02/24-15/24-32 notes

## Exit Criteria
- [ ] Parser reads container → files; snapshot walk works — `labs/`
- [ ] Snapshot-recovery lab + design writeup — `labs/` + `notes/`

## Links
- [Apple APFS reference](https://developer.apple.com/support/downloads/Apple-File-System-Reference.pdf)
- [apfs-fuse](https://github.com/sgan81/apfs-fuse)
