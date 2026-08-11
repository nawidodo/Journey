# 02-02 · NES Core — 6502 CPU

**Week:** W8–10 · **Track:** B · **Prev:** [`../01-chip8-cli-c`](../01-chip8-cli-c/README.md) · **Next:** [`../03-nes-ppu`](../03-nes-ppu/README.md)

## Objective
Cycle-accurate 6502 CPU core — registers, addressing modes, interrupts.

## Tasks
- [ ] Registers A/X/Y/SP/P; flags
- [ ] 13 addressing modes; 56 official opcodes
- [ ] Interrupts: NMI, IRQ, RESET vectors
- [ ] Clock stepping + cycle counting (`Read/Write` per opcode)
- [ ] Verify against `nestest.nes` log

## Resources
- NESDev wiki — CPU (6502) pages
- OneLoneCoder NES series, part 2 (CPU)
- *The 6502 Instruction Set* reference cards
- Pikuma — [NES Programming with 6502 Assembly](https://pikuma.com/courses) (learn the console you're emulating: flips the lens, you'll know *why* each register exists) + [Digital Electronics & Computer Architecture](https://pikuma.com/courses) (computer-arch substrate for the 6502)

## Exit Criteria
- [ ] `nestest.nes` log matches official trace (up to first ~9k lines) — `labs/`
