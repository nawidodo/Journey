# 24-09 · Own C compiler — small subset, x86-64, self-consistent (stretch)

**Week:** W46+ opt (stretch) · **Track:** P · **Prev:** [`../08-own-micro-transformer`](../08-own-micro-transformer/README.md)

## Objective
Own-assembler (07) emits bytes; a compiler is where intent becomes bytes. Write a C-subset compiler to x86-64: lexer → parser (Pratt/recursive descent) → type-check → codegen → link your 07 assembler output. The project that makes disassembly (15-08) read like a language you wrote.

## Tasks
- [ ] Front end: lexer + recursive-descent parser, C subset (int/pointers/arrays, structs, control flow, functions, recursion); error messages that point at the token
- [ ] Codegen: x86-64 System V (args in regs, stack frames, prologue/epilogue); expression codegen → emit to your 24-07 assembler format
- [ ] Self-host target: compile a nontrivial program (e.g., your own hash or a mini-ELF writer) and run it; then compile *itself* (the compiler compiles the compiler — the classic moment)
- [ ] Debug: generate DWARF or line info so your 15-07 debugger single-steps compiled code
- [ ] Self-check: compiler output passes a small test suite; self-hosting round-trip works (compiler v1 compiles compiler v2)

## Resources
- "Crafting Interpreters" (the parser half); 24-07 assembler + 15-08 disassembler as the other two vertices; your 15-01 Ghidra notes

## Exit Criteria
- [ ] Compiler builds + runs ≥1 real program (hash or ELF writer) — `labs/`
- [ ] Self-hosting round-trip — `labs/`

## Links
- [Crafting Interpreters](https://craftinginterpreters.com/)
- [System V ABI](https://wiki.osdev.org/System_V_ABI)
