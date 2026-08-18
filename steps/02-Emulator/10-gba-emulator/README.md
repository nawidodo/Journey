# 02-10 · GBA emulator — ARM7TDMI core, PPU, timing (stretch)

**Week:** W18+ stretch · **Track:** B · **Prev:** [`../09-unicorn-cpu-emulation`](../09-unicorn-cpu-emulation/README.md)

## Objective
NES taught 6502 + PPU/APU. GBA is the same shape with a real ARM core: ARM7TDMI (ARMv4T, THUMB mode), PPU (tilemaps, affine BG, sprites), APU, and the part NES skipped — cycle-accurate timing. The step where emulation stops being a toy and starts being systems work.

## Tasks
- [ ] ARM7TDMI core: ARM + THUMB instruction sets, CPSR flags, coprocessor (cp15 cache/MMU no-ops for GBA); validate against a known-good core or `arm-none-eabi` on the same opcodes
- [ ] Memory map + BIOS: IWRAM/EWRAM, IO registers, HBlank/VBlank IRQs — where the timing lives (pairs 02-03 NES PPU lessons, 5× deeper)
- [ ] PPU: tilemaps (4/8bpp), affine BG + rotation, OBJ (sprites), priority; APU: ch1–4 (square/wave/noise/DMA)
- [ ] Cycle counting: the difference between "runs" and "runs right" — demo frame timing against real hardware behavior
- [ ] Self-check: one commercial GBA ROM boots to attract mode; the classic AGB test ROMs (memory, timing, PPU) pass your emulator

## Resources
- GBATEK (the manual); ARM7TDMI TRM; your 02-02/03/04 NES notes (same skeleton)

## Exit Criteria
- [ ] ARM7TDMI + THUMB core passes AGB CPU test ROM — `labs/`
- [ ] One game reaches attract mode (PPU + APU + IRQs) — `labs/`

## Links
- [GBATEK](https://problemkaputt.de/gbatek.htm)
- [ARM7TDMI TRM](https://developer.arm.com/documentation/ddi0210/latest/)
