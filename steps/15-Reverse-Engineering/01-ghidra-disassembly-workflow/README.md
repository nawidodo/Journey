# 15-01 · Ghidra + Disassembly Workflow

**Week:** W16–17 · **Prev:** — · **Next:** [`../02-xnu-kernelcache-re`](../02-xnu-kernelcache-re/README.md)

## Objective
Fast enough with a decompiler to triage real binaries — the prerequisite every later RE-heavy step (HEVD, kernelcache, checkm8) assumes.

## Tasks
- [ ] Ghidra (free — no macOS/IDA licensing); project + codebrowser workflow; disassembly vs decompiler view
- [ ] Function recovery: signatures, cross-references, string xrefs, struct recovery
- [ ] Binary Ninja free tier as a second lens
- [ ] RE one real small binary end-to-end: logic from decompiler alone, then confirm in disassembly
- [ ] crackmes.one beginner set (5–10) as reps

## Resources
- Ghidra docs; *Getting Started with Ghidra*; *Practical Binary Analysis* (Andriesse); *Reverse Engineering for Beginners* (Yurichev)

## Exit Criteria
- [ ] Crackme writeups — `notes/`
- [ ] A reusable Ghidra project — `code/`