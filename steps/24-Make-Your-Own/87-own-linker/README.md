# 24-87 · Own linker — ELF relocations, symbols, the toolchain's glue (stretch)

**Week:** W46+ opt (stretch) · **Track:** P · **Prev:** [`../86-own-htop-lite`](../86-own-htop-lite/README.md) · **Next:** [`../88-own-ldap-lite`](../88-own-ldap-lite/README.md) · **Pairs:** 24-07, 24-09, 24-11

## Objective
Your 24-07 assembler emits relocatable objects, your 24-09 compiler emits more; nobody links them. Build the missing tool: an ELF linker — symbol resolution (global/undefined/weak), relocation application (the R_X86_64_* classes — the math), section merging + layout, and output executable/DSO generation. Then the debugging payoff: your 24-09 C compiler's output links with your linker into a runnable binary (the three-tool chain moment — pairs 24-07/09), and you finally read a crash backtrace with real understanding (pairs 15-07).

## Tasks
- [ ] Object parse: ELF relocatable sections/symbols (24-07 format reuse), collect definitions/undefineds
- [ ] Resolution: global symbol table, weak/strong rules, duplicate detection (the first link error you've always cursed)
- [ ] Relocations: layout sections, apply R_X86_64_64/PLT32/GOTPCREL (+ assignment math); build PLT/GOT-lite for functions
- [ ] Output: executable with program headers (24-31 bootloader/loader awareness), or DSO-lite; runtime resolution dance (24-11 varint/format discipline)
- [ ] Self-check: as(24-07) → your linker → runs; your 24-09 C compiler builds a full binary through your toolchain

## Resources
- System V ABI relocation chapter (the manual); lld source (peer — the modern linker); your 24-07/24-09 code

## Exit Criteria
- [ ] Linker produces runnable executable from own objects — `labs/` + `code/`
- [ ] Symbol/relocation writeup — `notes/`

## Links
- [System V AMD64 ABI](https://www.uclibc.org/docs/psABI-x86_64.pdf)
- [LLD](https://lld.llvm.org/)