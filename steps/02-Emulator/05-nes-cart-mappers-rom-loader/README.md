# 02-05 · NES Core — Cartridge, Mappers, ROM Loader

**Week:** W12–14 · **Track:** B · **Prev:** [`../04-nes-apu`](../04-nes-apu/README.md) · **Next:** [`../06-macos-swiftui-shell`](../06-macos-swiftui-shell/README.md)

## Objective
Load real ROMs: iNES header, PRG/CHR banking, mapper logic.

## Tasks
- [ ] iNES header parse (magic, PRG/CHR sizes, mapper id, mirroring)
- [ ] NROM (mapper 0) — SMB1, Donkey Kong
- [ ] MMC1 (mapper 1) — bank switching, PRG/CHR switching
- [ ] MMC3 (mapper 4) — IRQ counter, scanline IRQ (needed for many games)
- [ ] Save RAM + battery backup where applicable

## Resources
- NESDev wiki — Cartridge / Mapper pages
- iNES spec
- gbdev Pan Docs (memory map conventions)

## Exit Criteria
- [ ] SMB1 and one MMC1 game playable end-to-end — `labs/`

## Links
- [Nesdev mapper wiki](https://www.nesdev.org/wiki/Mapper)
- [iNES format wiki](https://www.nesdev.org/wiki/INES)
