# 24-03 · Own Git (content-addressed store + diff)

**Week:** W46+ (parallel, optional, stretch) · **Track:** P · **Prev:** [`../02-own-database-storage-engine`](../02-own-database-storage-engine/README.md) · **Next:** [`../04-own-shell`](../04-own-shell/README.md)

## Objective
Rebuild git's core: blob/tree/commit objects, SHA-1 content-addressing, a delta/merge walk. You already use git daily (Phase 0) — now see why it works. Trains hashing, object graphs, and serialization (the same mechanics as file-system forensics and packer/unpacker work).

## Tasks
- [ ] `init`: .git layout, blob hashing, `hash-object`/`cat-file` — `code/`
- [ ] trees + commits + parent graph (`log` walk) — `code/`
- [ ] index/staging + one `status`/`diff` — `code/`
- [ ] Stretch: `checkout` to a working tree of that commit — `code/`
- [ ] Debrief: content addressing vs. file-forensic artifact parsing (Phase 21) — `notes/`

## Resources
- "Write yourself a Git" (Thibault); git source as the reference impl
- build-your-own-x: own git catalog

## Exit Criteria
- [ ] `git init`-equivalent producing real blob/tree/commit objects, recoverable by real git — `code/`
- [ ] Debrief note — `notes/`

## Links
- [Write yourself a Git](https://wyag.thb.lt/)
- [build-your-own-x: own git](https://github.com/codecrafters-io/build-your-own-x?tab=readme-ov-file#build-your-own-git)