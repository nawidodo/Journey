# 15-08 · Build a mini-disassembler — one arch, instruction-by-instruction (stretch)

**Week:** W20 parallel (stretch) · **Track:** — · **Prev:** [`../07-mini-debugger`](../07-mini-debugger/README.md)

## Objective
Ghidra decodes for you; build the decoder. One architecture (x86-64 core, or ARM64 subset) from the manual: fetch → decode → operand table → mnemonic/operand string. The payoff is reading disassembly as *structure*, not text.

## Tasks
- [ ] Pick arch + scope: x86-64 (REX prefix, ModRM, SIB, imm/disp) or ARM64 subset (base + memory + branch); commit to the manual (Intel SDM vol. 2 / ARM ARM) as source of truth
- [ ] Decoder core: instruction-length determination (the x86 hard part), opcode → mnemonic table, operand decode → text output
- [ ] Validate: disassemble 15-01's challenge binaries + your own 04-01 shellcode; diff against Ghidra/objdump on the same bytes — every mismatch is a decode bug or a doc gap
- [ ] Optional cheap add: linear vs recursive traversal (call-target following — the CFG seed angr 15-06 uses)
- [ ] Self-check: your disassembler + 15-07 debugger together annotate one crackme function correctly (cross-check Ghidra)

## Resources
- Intel SDM / ARM ARM; `objdump -d` as ground truth; your 15-01 Ghidra notes

## Exit Criteria
- [ ] Decoder handles your chosen subset, matching objdump on ≥20 test binaries — `labs/`
- [ ] Mismatch log: every decode bug found + fixed — `notes/`

## Links
- [Intel SDM](https://www.intel.com/content/www/us/en/developer/articles/technical/intel-sdm.html)
- [ARM ARM](https://developer.arm.com/documentation/ddi0487/latest/)
