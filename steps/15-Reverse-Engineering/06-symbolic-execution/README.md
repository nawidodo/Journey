# 15-06 · Symbolic execution — angr, Triton, KLEE (stretch)

**Week:** W19–20 parallel · **Track:** — · **Prev:** [`../05-capstone-crash-triage`](../05-capstone-crash-triage/README.md)

## Objective
The concolic half of the binary-analysis story (05-11 fuzzing is the other half). Solve and deobfuscate what brute force can't, and know when each tool wins.

## Tasks
- [ ] angr basics: CFG recovery, symbolic variables, the solver, path explosion and why it's the enemy; load and solve a small crackme
- [ ] Solve 3+ Phase-3-style challenges (crackmes.one / pwn-college / CTF) with angr where manual analysis would take hours — document what made them symex-friendly (magic-byte checks, length checks)
- [ ] Triton: one obfuscated sample (analysis only, no execution of live malware) — deobfuscate a control-flow-flattened or opaque-predicate function to readable logic
- [ ] KLEE on a small C harness: symbolic input vs your 05-11 libFuzzer harness — where concolic beats coverage-guided fuzzing and vice versa
- [ ] Write the decision rule: when do you reach for angr, Triton, KLEE, AFL, or manual RE — `notes/`

## Resources
- angr docs + Trail of Bits blog; Triton docs; KLEE docs; *Practical Binary Analysis* (Andriesse) ch. on symex

## Exit Criteria
- [ ] 3 angr solves + writeups — `labs/` + `notes/`
- [ ] Triton deobfuscation notes: flattened function → readable logic — `notes/`
- [ ] KLEE vs libFuzzer comparison on one harness — `notes/`

## Links
- [angr](https://angr.io/)
- [Triton](https://triton.quarkslab.com/)
- [KLEE](https://klee.github.io/)
