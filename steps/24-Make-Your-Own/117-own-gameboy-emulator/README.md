# 24-117 · Own Game Boy emulator — the classic CPU core + PPU, from scratch (stretch)

**Week:** W46+ opt (stretch) · **Track:** P · **Prev:** [`../116-own-ble-scanner`](../116-own-ble-scanner/README.md) · **Next:** none (last) · **Pairs:** 02-11, 02-10, 24-63, 24-110

## Objective
Phase 02 emulates PS1/PSP/GBA but no DMG — the purest first core: an original Game Boy emulator — 8-bit LR35902 CPU (Game Boy's Z80-ish core: register file, opcode table, the timing/carry/flag discipline 24-110 taught for A64), 4KB VRAM + tile/background/OBJ rendering (the PPU: scanlines, mode timing, the 24-63 raster-thinking), joypad + timer, and cartridge MBC (mapper banks — the 24-28 memory-mapping pattern). The payoff: your own emulator boots an open-source homebrew ROM to the logo screen with sound — versus the real device is optional (own console only); the correctness check is the public blargg test ROMs (the 15-08 oracle discipline). Then the security-angle page: why emulator bugs are the same class as console bugs (timing side effects, OP cache errors — 24-90/04-06 thinking) and what a JIT (02-17) would change.

## Tasks
- [ ] CPU: LR35902 core — full opcode map (8-bit ALU flags, 16-bit loads, interrupts), fetch-decode-execute with cycle counts
- [ ] PPU: tile data → tilemap → background + sprites, the scanline/mode state machine (the famous timing)
- [ ] Memory: bus with cartridge MBC1/3 banking + RAM, joypad/timer registers (the 24-16/discipline)
- [ ] APU-lite: the 4-channel square/noise (24-40 pulse-engine reuse, mono mix)
- [ ] Verify: blargg cpu_instrs/timer/mem_timing ROMs pass (or a documented subset) — `labs/` + `code/`
- [ ] Writeup: emulator-bug class vs console-bug class, JIT differences — `notes/`

## Resources
- Pandocs (the DMG bible), blargg test ROMs, your 02-10/02-11/24-110/24-40 code

## Exit Criteria
- [ ] Emulator boots homebrew to logo, passes blargg cpu/timer tests — `labs/` + `code/`
- [ ] Emulator-vs-console bug writeup — `notes/`

## Links
- [Pandocs](https://gbdev.io/pandocs/)
- [blargg test ROMs](https://github.com/retrio/gb-test-roms)