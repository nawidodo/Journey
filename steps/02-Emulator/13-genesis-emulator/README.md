# 02-13 · Sega Genesis emulator — 68000 + Z80 coprocessor, planar VDP (stretch)

**Week:** W18+ stretch · **Track:** B · **Prev:** [`../12-snes-emulator`](../12-snes-emulator/README.md)

## Objective
New ISA, new rendering model: 68000 (clean CISC — the Mac classic's CPU, your drivers/graphics tracks are 68k-adjacent) + Z80 coprocessor driving the FM sound chip; planar VDP (tile *planes* bit-packed, not byte-mapped like NES/SNES). Breadth the ARM/6502/MIPS cores don't give.

## Tasks
- [ ] 68000 core: 16 32-bit registers, address-unit semantics (the orthogonal addressing modes), condition codes, exception vector table; validate against a known core
- [ ] Z80 coprocessor: the audio brain — FM (YM2612) + PSG (SN76489) chips driven from Z80-side programs; dual-CPU scheduling (68k vs Z80 clock ratio)
- [ ] VDP: planar tile rendering (BG planes, sprites, window), 320x224, scroll; where 68k uploads VRAM vs the VDP's DMA
- [ ] Self-check: Sonic-class commercial ROM boots to playable attract/level; Z80 audio program audibly correct

## Resources
- 68000 user manual; Z80 manual; Mega Drive VDP docs; your NES/SNES PPU lessons (same pipeline, different packing)

## Exit Criteria
- [ ] 68000 core passes a known-good test suite (e.g., 68k tests) — `labs/`
- [ ] Game playable with FM audio — `labs/`

## Links
- [68000 manual](https://www.nxp.com/docs/en/reference-manual/M68000PRM.pdf)
- [Mega Drive VDP docs](https://github.com/bytecell/md-cpu-info)
