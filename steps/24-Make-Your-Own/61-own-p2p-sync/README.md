# 24-61 · Own p2p sync — Syncthing-lite: versioned file sync over your DHT (stretch)

**Week:** W46+ opt (stretch) · **Track:** P · **Prev:** [`../60-own-spam-filter`](../60-own-spam-filter/README.md) · **Pairs:** 24-36, 24-29, 24-53, 24-15

## Objective
BitTorrent syncs files to many; Syncthing syncs a folder between your devices. Build a p2p sync engine: folder/index model, version vector + conflict detection (the CRDT-lite discipline — pairs 24-02 DB, 24-26 LSM), chunked transfer with delta (reuse 24-36 piece model + 24-25 hashing), peer discovery (24-29 Kademlia) + direct transfer (24-53 traversal). Then the corruption lab: concurrent edits on two devices → conflicts detected, no data loss (the deliverable).

## Tasks
- [ ] Index: folder → file list + version vectors per file; the sync algorithm (compare → pull/push missing)
- [ ] Transfer: chunking + SHA-256 verify (24-36 pieces), delta sync (hash-based — pairs 24-53 Tailscale wire reuse)
- [ ] Conflicts: simultaneous edits → version-vector conflict → keep-both + marker (the no-silent-loss guarantee); test matrix
- [ ] Peer layer: discovery via own 24-29 DHT + direct transfer via own 24-53/NAT traversal; offline→online catch-up
- [ ] Corruption lab: kill mid-transfer, concurrent edits, disk-full — verify integrity + conflict handling — `labs/`

## Resources
- Syncthing protocol docs (the manual); your 24-36/24-29/24-53/24-15 code (reuse the pieces)

## Exit Criteria
- [ ] Two "devices" converge to identical state; conflicts surfaced, never silent — `labs/`
- [ ] Failure-matrix writeup (kill/corrupt/concurrent) — `labs/` + `notes/`

## Links
- [Syncthing](https://syncthing.net/)
- [Syncthing protocol](https://docs.syncthing.net/specs/)
