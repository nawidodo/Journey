# 02-12 · SNES emulator — 65C816, Mode 7, SPC700 (stretch)

**Week:** W18+ stretch · **Track:** B · **Prev:** [`../11-riscv-emulator`](../11-riscv-emulator/README.md)

## Objective
The direct continuation of your NES core: 65C816 is 6502 with 16-bit registers + banking — your existing core logic carries, the memory model doesn't. Adds Mode 7 (the rotation math), 8/16-bit BG modes, and a second CPU: SPC700 audio coprocessor (a CPU driving an APU — NES APU lesson ×2).

## Tasks
- [ ] 65C816 core: 8/16-bit accumulator/index modes, 24-bit banking (LoROM/HiROM), emulation vs native mode; extend your 02-02 6502 core rather than start fresh
- [ ] PPU: BG modes 0–7 (tilemaps, 8x8/16x16, palettes), Mode 7 affine/rotation with matrix math; OBJ + priority; interlace/layers
- [ ] SPC700: separate 8-bit core + 64KB SRAM + DSP registers; audio programs run on it (the coprocessor lesson — dual CPU + shared memory)
- [ ] Timing: NTSC scanline-driven; DMA/HBlank windows
- [ ] Self-check: SMW-style commercial ROM boots to title + plays a level with sound; the accepted SNES test ROMs (CPU/PPU/APU) pass

## Resources
- anomie's SNES docs; fullsnes.txt; your 02-02/03/04 NES notes (the carry-forward skeleton)

## Exit Criteria
- [ ] 65C816 core passes SNES CPU test ROM — `labs/`
- [ ] Game boots to playable level with audio (SPC700 program running) — `labs/`

## Links
- [fullsnes](https://problemkaputt.de/fullsnes.htm)
- [anomie's docs](https://github.com/undisbeliever/snesdev/blob/master/docs/anomie.md)
