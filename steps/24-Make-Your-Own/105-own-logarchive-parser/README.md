# 24-105 · Own .logarchive parser — unified logging, the macOS DFIR goldmine (stretch)

**Week:** W46+ opt (stretch) · **Track:** P · **Prev:** [`../104-own-dylib-injector`](../104-own-dylib-injector/README.md) · **Pairs:** 21-06, 24-45, 24-25, 21-10

## Objective
macOS forensics (21-06 DFIR) lives mostly in unified logging — .logarchive files (tracev3 format) that `log show` reads. Build a parser-lite: the container (chunk store: metadata + compressed text chunks, zlib per 24-25), the catalog/timesync structures, and reconstruct a timeline of events with subsystem/category/level — then query it (pairs 21-10 memory-scanner thinking, 24-45 hive-walking discipline). The payoff: your tool reads the same archives `log` reads (the oracle comparison), and you can hunt your own 104 injection events / 21-06 EDR markers in the log — the real DFIR loop on the platform you're on.

## Tasks
- [ ] Container: .logarchive directory layout, tracev3 chunk store, chunk metadata (u64/u32 fields — 24-97 ABI discipline), zlib text chunks (24-25)
- [ ] Catalog: parse catalog entries (message strings, subsystem/category/level metadata — the format RE)
- [ ] Query: filter by subsystem/level/process, time-reconstruct ordered timeline (timesyncdelta math — 24-30)
- [ ] Lab: `log collect`/archive your own device event (or a seeded test archive), parse with own tool vs `log show` — field-level match (oracle); hunt your own 103/104 test activity in the archive — `labs/`
- [ ] Writeup: why unified logging replaced ASL, privacy redaction (private/on-device), forensic value + tampering — 21 DFIR notes — `notes/`

## Resources
- tracev3/logarchive reverse-engineering notes (the manual — libtrace source); your 24-45/24-25/21-06 code

## Exit Criteria
- [ ] Parser reconstructs a queryable timeline matching `log` — `labs/` + `code/`
- [ ] Format + DFIR writeup — `notes/`

## Links
- [apt_tracev3 / logarchive format notes](https://github.com/ydkhatri/MacForensics)
- [libtrace](https://github.com/apple-oss-distributions/libtrace)