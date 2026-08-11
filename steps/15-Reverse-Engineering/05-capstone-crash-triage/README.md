# 15-05 · Capstone: Crash Log → Root Cause Chain 🚩 M16

**Week:** W20 · **Prev:** [`../04-patch-diffing-cve-workflow`](../04-patch-diffing-cve-workflow/README.md)

## Objective
Triage one real crash from a stripped binary to a root-cause chain, with a Ghidra walkthrough — the deliverable that proves the phase.

## Tasks
- [ ] Pick a target: an iOS crash log you generated, or a known-but-unpatched local binary
- [ ] Chain: crash frame → faulting instruction → owning function → calling path → triggering input
- [ ] Reconstruct the vulnerable logic in Ghidra; write a repro
- [ ] Cross-link to a real CVE of the same class (from 15-04's table)
- [ ] Analyst-style writeup, one page

## Resources
- Your 15-01–04 outputs; Project Zero / crash-analysis blogs

## Exit Criteria
- [ ] **M16:** crash → root-cause chain + Ghidra walkthrough — `notes/` + `labs/`

## Links
- [Volatility 3](https://github.com/volatilityfoundation/volatility3)
- [Ghidra](https://github.com/NationalSecurityAgency/ghidra)
