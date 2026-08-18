# 24-97 · Own Mach-O parser — fat headers, load commands, the mac/iOS binary (stretch)

**Week:** W46+ opt (stretch) · **Track:** P · **Prev:** [`../96-own-dnssec-lite`](../96-own-dnssec-lite/README.md) · **Next:** [`../98-own-mp4-muxer`](../98-own-mp4-muxer/README.md) · **Pairs:** 24-33, 15-08, 19-04, 07-06

## Objective
Mac/iOS binaries are Mach-O — the format nobody builds. Make one: a parser — fat/universal headers (arch slice selection), Mach header + load commands (LC_SEGMENT_64 → sections/relocs, LC_LOAD_DYLIB, LC_CODE_SIGNATURE), dyld info (bind/export trie walking — the dyld-linked-list discipline), and symbol/string extraction feeding your 15-08 disassembler. The security payoff: parse code signatures (SuperBlob → CodeDirectory → cdHash, the LC_CODE_SIGNATURE structure real tools hash), and a lab where you patch a `__TEXT` byte in your own binary → cdHash changes → `codesign --verify` behavior explained — the foundation for 19-04 hooking and dyld tricks.

## Tasks
- [ ] Loader: fat header + slice selection, MH_MAGIC_64 both endians, header flags (what MH_PIE/HARDENED mean — 07 pairs)
- [ ] Commands: walk LC list, parse segments/sections (addr→file mapping), LC_LOAD_DYLIB, LC_UUID, LC_CODE_SIGNATURE offset
- [ ] dyld info: export trie walk → symbol table; bind opcodes-lite (the compact table format RE)
- [ ] Signature: SuperBlob/CodeDirectory parse, cdHash computation (the hashing rules — 20-07/24-70 join), verify against `codesign` output (the oracle)
- [ ] Patch lab: flip a byte in `__TEXT` of a test binary → own verifier flags cdHash mismatch, macOS rejects (ad-hoc sign state explained); restore + re-sign — `labs/`
- [ ] Feed: your parser's sections feed 15-08 disassembly of a system binary (own machine, lab-only) — `notes/`

## Resources
- Apple Mach-O file format reference (the manual); llvm-objdump source (peer); your 24-33/15-08 code

## Exit Criteria
- [ ] Parser lists commands/sections/symbols + cdHash matches macOS — `labs/` + `code/`
- [ ] Code-signature + patch-lab writeup — `labs/` + `notes/`

## Links
- [Mach-O file format (Apple)](https://developer.apple.com/library/archive/documentation/DeveloperTools/Conceptual/MachRuntime/Introduction/introduction.html)
- [LLVM's MachOObjectFile](https://llvm.org/doxygen/MachOObjectFile_8h.html)