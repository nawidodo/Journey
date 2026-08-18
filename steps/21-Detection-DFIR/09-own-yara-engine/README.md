# 21-09 · Build a YARA engine — rules you compile, matcher you write (stretch)

**Week:** W36+ stretch · **Track:** M · **Prev:** [`../08-honeypots`](../08-honeypots/README.md)

## Objective
03-detect-as-code uses YARA; build the engine. Rule parser/compiler → matcher: Aho-Corasick string sets, wildcards, XOR, anchored/wide strings, hashes, condition evaluation. Then run your engine against 12-06's implant samples — your own detection code on your own malware.

## Tasks
- [ ] Compiler: YARA rule grammar subset → internal representation (string sets, modifiers, conditions AST); error reporting that beats stock yara's messages
- [ ] Matcher: Aho-Corasick (the multi-string core), wildcard/anchor/wide handling, XOR modifier; hash + file metadata conditions
- [ ] Condition engine: boolean/arithmetic evaluation over match context; `all of them`, `2 of (a,b,c)`, counts
- [ ] Validate: same rules + samples vs stock `yara` CLI — identical verdicts on a corpus (your 12-06 implant, known packers from 12-10, a clean binary)
- [ ] Self-check: your engine flags 12-06 implant with ≥3 distinct rules; verdict parity log vs stock yara

## Resources
- YARA source (the reference); Aho-Corasick notes; your 12-06/12-10 artifacts as the corpus

## Exit Criteria
- [ ] Engine compiles + matches a rule set, parity vs stock yara — `labs/`
- [ ] Implant flagged by own rules — `labs/`

## Links
- [YARA](https://github.com/VirusTotal/yara)
- [Aho-Corasick](https://en.wikipedia.org/wiki/Aho%E2%80%93Corasick_algorithm)
