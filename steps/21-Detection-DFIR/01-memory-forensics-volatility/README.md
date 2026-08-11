# 21-01 · Memory Forensics with Volatility 3

**Week:** W30–31 · **Track:** M · **Prev:** — · **Next:** [`../02-log-analysis-threat-hunting`](../02-log-analysis-threat-hunting/README.md)

## Objective
Read a compromised machine's RAM like a file system — the reverse of everything you've exploited. See your own Phase 5 exploits from the defender's side.

## Tasks
- [ ] Capture memory (Linux: `linux_mem`, Windows: live + hibernation dumps; Mac: `macos` plugin set)
- [ ] Volatility 3 core plugins: `pslist`/`pstree`/`psscan`, `net` (conns), `malfind`/`modscan`, `cmdline`, `dlllist`, `handles`
- [ ] Profile/OS detection (symbol files vs `kdbg`); what changes across OSes
- [ ] Triage a provided incident image: find the injected process, the C2 connection, the persistence — `labs/`
- [ ] Correlate with your Track E knowledge: what your own implant would look like in memory

## Resources
- *The Art of Memory Forensics* (Ligh et al.) — the book
- Volatility 3 docs + sample images (Volatility Foundation / CISA)
- Your Track E + Track J artifacts (make your own image with your implant, then find it)

## Exit Criteria
- [ ] One incident image fully triaged (rogue process → injected code → C2 → persistence) — `labs/`
- [ ] Notes: plugin→artifact map — `notes/`

## Links
- [Volatility 3](https://github.com/volatilityfoundation/volatility3)
- [The Art of Memory Forensics (No Starch)](https://nostarch.com/artmemoryforensics)
