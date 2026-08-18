# 24-32 · Own NTFS parser — MFT records, USN journal, deleted-file recovery (stretch)

**Week:** W46+ opt (stretch) · **Track:** P · **Prev:** [`../31-own-bootloader`](../31-own-bootloader/README.md) · **Next:** [`../33-own-apfs-parser`](../33-own-apfs-parser/README.md)

## Objective
Windows is covered for exploitation but its filesystem isn't. NTFS is the DFIR goldmine: $MFT, $MFTMirr, $LogFile, $UsnJrnl, alternate data streams, timestamps that never lie (usually). Build a parser over a raw image (or your Windows VM's disk copy): MFT record walk, attributes, runlists, resident/non-resident, deleted-record carving, USN journal replay. The forensic payoff pairs 21-04 and your 24-15 ext2-lite (same discipline, richer format).

## Tasks
- [ ] Volume: boot sector (cluster size, MFT offsets), $MFT/$MFTMirr, attribute lists (the format's complexity is the lesson)
- [ ] Records: MFT record headers, $STANDARD_INFORMATION (the four timestamps), $FILE_NAME, $DATA (resident + runlist/non-resident)
- [ ] Deleted recovery: deleted-record state (the $MFT record survives), carve data runs; USN journal (your own 24-30 profiler's read audit)
- [ ] ADS + timestamps lab: alternate data streams (hide a file — the malware trick), timestamp forensics (pairs 12-08 anti-forensics)
- [ ] Self-check: parse an image you created (mkfs.ntfs + files + deletes), find the deleted file; writeup on NTFS timestamp semantics (the $FILE_NAME vs $SI lie)

## Resources
- NTFS spec (the manual); The Sleuth Kit source (peer); your 24-15 + 21-04 notes

## Exit Criteria
- [ ] Parser walks MFT, reads files, recovers deleted records — `labs/`
- [ ] ADS + timestamp lab writeup — `labs/` + `notes/`

## Links
- [NTFS spec (Microsoft)](https://learn.microsoft.com/en-us/windows/win32/fileio/ntfs-overview)
- [The Sleuth Kit](https://www.sleuthkit.org/)
