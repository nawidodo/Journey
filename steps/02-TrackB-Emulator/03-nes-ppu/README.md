# 02-03 · NES Core — PPU

**Week:** W10–12 · **Track:** B · **Prev:** [`../02-nes-cpu-6502`](../02-nes-cpu-6502/README.md) · **Next:** [`../04-nes-apu`](../04-nes-apu/README.md)

## Objective
Picture Processing Unit: rendering pipeline that turns tile/attribute data into pixels.

## Tasks
- [ ] PPU registers (`$2000`–`$2007`), internal latch
- [ ] Nametables + attribute tables; palette RAM
- [ ] Pattern tables (tile data, 8×8/8×16 sprites)
- [ ] OAM sprites, sprite 0 hit, sprite overflow
- [ ] Scroll registers (fine X/Y), rendering enable bits
- [ ] Scanline/interlace timing (262 scanlines, dots 0–340)

## Resources
- NESDev wiki — PPU pages
- OneLoneCoder NES series, part 4 (PPU)
- gbdev Pan Docs (bus-level concepts transfer)

## Exit Criteria
- [ ] Scrollable background + sprites render (use `ppu_test` ROMs) — `labs/`

## Links
- [Nesdev PPU wiki](https://www.nesdev.org/wiki/PPU)
- [Nesdev PPU rendering](https://www.nesdev.org/wiki/PPU_rendering)
- [Mesen emulator (debugger to compare)](https://github.com/SourMesen/Mesen2)
