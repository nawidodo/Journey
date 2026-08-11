# 02-01 · CHIP-8 — CLI Emulator in C

**Week:** W6–10 · **Track:** B · **Prev:** — · **Next:** [`../02-nes-cpu-6502`](../02-nes-cpu-6502/README.md)

## Objective
First emulator: CPU, instruction set, timers, input — pure portable C, CLI rendering.

## Tasks
- [ ] Memory (4 KB RAM), V0–VF registers, I, stack, delay/sound timers
- [ ] Fetch–decode–execute loop; 36 opcodes (`0nnn`…`Fx29`)
- [ ] Keypad input mapping
- [ ] Display (64×32) + timers at 60 Hz
- [ ] Load a ROM (IBM logo, PONG) from `labs/roms/`

## Resources
- Cowgod's Chip-8 Technical Reference
- CHIP-8 on Wikipedia (opcode table)
- OneLoneCoder tutorial (first halves; NES next)

## Exit Criteria
- [ ] PONG-style ROM runs at 60 Hz in the CLI — `code/`
