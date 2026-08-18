# 24-99 · Own DMG reader — UDIF, the macOS disk image format (stretch)

**Week:** W46+ opt (stretch) · **Track:** P · **Prev:** [`../98-own-mp4-muxer`](../98-own-mp4-muxer/README.md) · **Pairs:** 24-33, 24-25, 24-45, 24-97

## Objective
.macOS installers and apps ship as DMGs: build a reader-lite — UDIF trailer (koly: checksum/sector counts), block/chunk table parsing (the run-length compressed chunk map — the format's soul), zlib-decompress chunks (24-25), and reconstruct the raw disk image → mount the HFS+/APFS filesystem inside with your 24-33 skills. The security tie-in: DMGs carry quarantine/flags (LSQuarantine), hdiutil attaches untrusted images = the click-and-mount attack surface (pairs 12-07), and dmg content is a macOS malware delivery path — your parser exposes what gatekeeper checks.

## Tasks
- [ ] UDIF: koly trailer (signature/checksum/block size), BLKX table → chunk descriptors (run types: zero/zlib/raw — the compression map)
- [ ] Decompress: zlib-inflate chunk runs (24-25), zero-fill runs, assemble the linear disk image (24-33 sector math)
- [ ] Filesystem: parse the resulting image with your 24-33 APFS or HFS+ knowledge — list files, extract one (the milestone)
- [ ] Checksum: chksum trailer verify (CRC32/blkx) — corrupt a block → detected (the integrity lab)
- [ ] Writeup: quarantine/gatekeeper semantics, why DMG-mount is a click-attack vector (pairs 12-07/17-03), hdiutil comparison — `labs/` + `notes/`

## Resources
- UDIF format notes (the manual — trailer/block tables); hfsleuth/dmg libraries (peer); your 24-33/24-25 code

## Exit Criteria
- [ ] Reader extracts a file from a real DMG (own, lab-only) — `labs/` + `code/`
- [ ] Trailer/checksum + quarantine writeup — `labs/` + `notes/`

## Links
- [UDIF/dmg format](http://newosxbook.com/DMG.html)
- [libdmg-hfsplus](https://github.com/planetbeing/libdmg-hfsplus)