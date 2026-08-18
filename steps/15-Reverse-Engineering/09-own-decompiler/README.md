# 15-09 · Build a mini-decompiler — CFG structuring, stack recovery (stretch)

**Week:** W20 parallel (stretch) · **Track:** — · **Prev:** [`../08-mini-disassembler`](../08-mini-disassembler/README.md)

## Objective
You disassemble (15-08) and read Ghidra output (15-01). Build the middle: disassembly → structured C. Stack-frame recovery (where locals live, calling convention), control-flow structuring (goto-elimination: if/while/switch from CFG), basic type inference (pointer vs int from usage). The project that turns "reading assembly" into "recovering intent".

## Tasks
- [ ] Input: own 15-08 disassembler output (single function, then whole program); recover stack frame + register args from prologue/ABI (pairs 24-09 codegen — you'll see your own compiler's frames)
- [ ] CFG: build from branch/jump targets → dominator-based structuring: if/else, loops, switch — the canonical HLL recovery algorithms
- [ ] Type inference: usage-based (ptr arithmetic vs index, syscall/ABI tells); local variable naming from offsets
- [ ] Validation: decompile your own 24-09-compiled functions and diff against source (ground truth you wrote); then a 04-01 shellcode function
- [ ] Self-check: decompiler output of your own compiled function is recognizable/re-expressible; Ghidra comparison on same function

## Resources
- "Reverse Engineering for Beginners" decompiler chapter; Hex-Rays research notes; your 15-08 + 24-09 code (the two inputs you control)

## Exit Criteria
- [ ] Decompiler recovers structured C for your own compiled functions — `labs/`
- [ ] CFG-structuring + type-inference pass documented — `notes/`

## Links
- [RE for Beginners](https://beginners.re/)
- [DREAM decompiler research](https://github.com/REhints/Publications)
