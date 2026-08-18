# 24-83 · Own backup tool — restic-lite: dedupe chunks + encrypted snapshots (stretch)

**Week:** W46+ opt (stretch) · **Track:** P · **Prev:** [`../82-own-kernel-gdb-stub`](../82-own-kernel-gdb-stub/README.md) · **Next:** [`../84-own-init-system`](../84-own-init-system/README.md) · **Pairs:** 24-25, 24-71, 24-61, 24-58

## Objective
Backup you can trust because you built it: a restic-lite — content-defined chunking (rolling hash — pairs 24-61 delta + 24-25), dedupe (identical chunks stored once — the storage win), encrypted snapshot tree (24-71 crypto discipline) with authenticated headers (GCM), and restore with verify (hashes + decrypt check) into a foreign directory (24-15). Then the honest test: back up your own repo 100× with one change — snapshot 101 adds one chunk; restore byte-identical (the oracle).

## Tasks
- [ ] Chunking: CDC (Rabin-lite rolling hash) for content-defined boundaries — dedupe survives insertions (24-25 hashing)
- [ ] Store: chunk blob store + index (24-58 layout), snapshot tree (dir→tree of chunks — your 24-15 inode thinking)
- [ ] Crypto: key derivation (24-71/20-12), per-chunk AES-GCM + authenticated tree (tamper-detected restore)
- [ ] Restore: walk tree → reassemble → verify (GCM + hash); the corruption-lab roundtrip — `labs/`
- [ ] Metrics: dedupe ratio on 100-twist repo, backup/restore times (24-30), size table — `labs/` + `notes/`

## Resources
- restic design docs (the manual); your 24-25/24-71/24-61/24-58 code

## Exit Criteria
- [ ] Backup→dedupe→restore byte-identical with tamper detection — `labs/` + `code/`
- [ ] Dedupe/metrics table — `notes/`

## Links
- [restic](https://restic.net/)
- [restic design document](https://restic.readthedocs.io/en/latest/100_references.html)