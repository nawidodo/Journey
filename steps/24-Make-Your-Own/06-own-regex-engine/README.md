# 24-06 · Own Regex Engine

**Week:** W46+ (parallel, optional, stretch) · **Track:** P · **Prev:** [`../05-own-tcp-ip-stack`](../05-own-tcp-ip-stack/README.md) · **Next:** [`../07-own-assembler`](../07-own-assembler/README.md)

## Objective
Build a regex engine by compiling patterns to NFAs and matching — the classic *Crafting Interpreters*-style exercise. Trains automata/complexity reasoning, the same engine logic behind signatures (Sigma/YARA/`detection` — Phase 21), input-grammar parsing, and exploit pattern matching.

## Tasks
- [ ] Lexer/parser for literals, `.`, `*`, `?`, `+`, `[]`, char classes — `code/`
- [ ] Thompson NFA construction; NFA→DFA subset construction — `code/`
- [ ] Matching with backreferences (stretch: grouping/captures) — `code/`
- [ ] Debrief: how this formalizes YARA/signature matching (Phase 21) — `notes/`

## Resources
- Russ Cox: "Regular Expression Matching: the Virtual Machine Approach" (defined truth)
- *Crafting Interpreters* (matching machine chapters precedent)

## Exit Criteria
- [ ] Engine matches the operator set with an NFA — `code/`
- [ ] Debrief mapping to signature engines — `notes/`

## Links
- [Russ Cox — Regular Expression Matching: the VM approach](https://swtch.com/~rsc/regexp/regexp1.html)
- [build-your-own-x: own regex](https://github.com/codecrafters-io/build-your-own-x?tab=readme-ov-file#build-your-own-regex)