# 24-71 · Own encrypted overlay FS — gocryptfs-lite: transparent dir encryption (stretch)

**Week:** W46+ opt (stretch) · **Track:** P · **Prev:** [`../70-own-ca`](../70-own-ca/README.md) · **Pairs:** 24-15, 20-11, 20-12

## Objective
Encryption you can touch: a directory-encrypting overlay (gocryptfs-style). Plaintext files stored encrypted on disk: content AES-GCM (your 20-11 discipline), file names encrypted + base64 (the naming scheme), per-file random nonces, and your own **FUSE-lite** mount (pairs 24-15 filesystem) so apps just see a normal folder. The security lessons: key derivation (20-12 Argon2), the metadata leak surface (sizes, timestamps — the forensics note, pairs 21-04), and what "at rest" actually protects.

## Tasks
- [ ] Crypto: AES-GCM file content (20-11), HKDF/Argon2id master key (20-12), random nonce per file (the reuse trap)
- [ ] Naming: filename encryption + base64url (the scheme that breaks directory listings); preserve length-limit handling
- [ ] Overlay: mount plaintext view via FUSE-lite (or simplest: a faithful clone + sync — the honest ceiling), open/write/read/rename semantics
- [ ] Attack lab: steal the encrypted dir — no plaintext leaked (metadata too?); tamper a ciphertext block → GCM auth fail (the detection) — `labs/`
- [ ] Writeup: gocryptfs design (why filenames encrypted separately), the size/timing leak reality — `notes/`

## Resources
- gocryptfs docs + design doc (the manual); your 24-15/20-11/20-12 code

## Exit Criteria
- [ ] Encrypted overlay mounts + roundtrips, GCM tamper detected — `labs/`
- [ ] Metadata-leak + design writeup — `labs/` + `notes/`

## Links
- [gocryptfs](https://nuetzlich.net/gocryptfs/)
- [gocryptfs design](https://nuetzlich.net/gocryptfs/design/)