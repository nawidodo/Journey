# 11-06 · Capstone: HEVD → Real CVE 🚩 M12

**Week:** W43–44 · **Track:** D · **Prev:** [`../05-mitigations-rop-ret2dir`](../05-mitigations-rop-ret2dir/README.md)

## Objective
Prove the full loop: bug discovery → exploit → mitigation-aware variant.

## Tasks
- [ ] Exploit 2 different HEVD bug classes *without* reading existing exploits (re-derive)
- [ ] Take one real CVE from step 04 and extend it: add an SMEP/CET-aware path or stabilize it
- [ ] Full writeup in `notes/`: discovery, trigger, primitive, exploitation, mitigations hit, lessons
- [ ] Record your VM config so it's reproducible

## Resources
- Your step 02–05 notes; HEVD + P0 archives
- ctf-wiki Windows section; sami97/mitigation posts

## Exit Criteria
- [ ] **M12:** 2 HEVD classes re-derived + 1 real CVE variant — `labs/`
- [ ] Publishable writeup — `notes/`
