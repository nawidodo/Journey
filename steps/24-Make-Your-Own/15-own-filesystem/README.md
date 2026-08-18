# 24-15 · Own filesystem — ext2-lite on a raw image, forensic payoff (stretch)

**Week:** W46+ opt (stretch) · **Track:** P · **Prev:** [`../14-own-terminal-emulator`](../14-own-terminal-emulator/README.md) · **Next:** [`../16-own-memory-allocator`](../16-own-memory-allocator/README.md)

## Objective
Build a filesystem: superblock, block/inode bitmaps, inode tables, directory entries — ext2-lite on a raw image file, mountable/checkable with your own tools. The security payoff: you already parse disk images in 21-04 DFIR; now you *wrote* the format. Deleted-file recovery (the DFIR trick) becomes your own feature.

## Tasks
- [ ] Layout: superblock, block bitmap, inode bitmap, inode table, data blocks; mkfs-lite creates a real image you can `dd` and inspect
- [ ] Ops: inode lookup, file read/write (indirect blocks), directory entries, create/unlink
- [ ] Tooling: own `ls`/`cat`/`stat` over the image (no kernel mount); own `fsck-lite` (bitmap/inode cross-check — finds your own corruptions)
- [ ] DFIR payoff: deleted-file recovery — unlink leaves inode + data (ext2 behavior); carve it back; write up as an artifact-forensics lesson (pairs 21-04, 12-08 anti-forensics) — `notes/`
- [ ] Self-check: create → write → unlink → recover on your image; fsck passes after controlled corruption

## Resources
- ext2 spec (the manual); e2fsprogs source (peer); your 24-02 storage-engine + 21-04 notes

## Exit Criteria
- [ ] ext2-lite image: create/write/unlink/recover, own ls/cat/fsck — `labs/`
- [ ] Deleted-file recovery writeup — `notes/`

## Links
- [ext2 spec](https://www.nongnu.org/ext2-doc/ext2.html)
- [e2fsprogs](https://e2fsprogs.sourceforge.net/)
