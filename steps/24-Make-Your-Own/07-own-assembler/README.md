# 24-07 · Own Assembler (x86-64)

**Week:** W46+ (parallel, optional, stretch) · **Track:** P · **Prev:** [`../06-own-regex-engine`](../06-own-regex-engine/README.md) · **Next:** [`../08-own-micro-transformer`](../08-own-micro-transformer/README.md)

## Objective
Write an assembler: parse assembly text into the binary encodings a CPU actually executes. This is the mirror image of your disassembly work (Phase 15 RE) — knowing the encoding rules makes reading `objdump`/Ghidra bytes automatic, and explains why ROP gadgets (Phase 3) are what they are.

## Tasks
- [ ] Lexer/parser for a small ISA subset (mov/add/lea/jmp/call, reg/mem/immediate forms) — `code/`
- [ ] Encoding: ModRM, SIB, displacement, REX (x86-64) to actual bytes — `code/`
- [ ] Assemble a small real program; `objdump -d` round-trip to verify — `code/`
- [ ] Debrief: tie encoding back to gadget hunting (Phase 3) — `notes/`

## Resources
- Intel SDM / AMD manuals (encoding tables); "How to write an assembler" writeups
- Your Phase 0 (Assembly K&R-adjacent) + Phase 15 notes

## Exit Criteria
- [ ] Binary output that `objdump -d` disassembles to your source — `code/`
- [ ] Debrief note — `notes/`

## Links
- [Intel x86-64 opcode map / encoding resources](https://wiki.osdev.org/X86-64_Instruction_Encoding)
- [build-your-own-x: own assembler](https://github.com/codecrafters-io/build-your-own-x?tab=readme-ov-file#build-your-own-interpreter--compiler)