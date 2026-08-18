# 02-14 · PS1 emulator — MIPS R3000, GTE, GPU (stretch)

**Week:** W18+ stretch · **Track:** B · **Prev:** [`../13-genesis-emulator`](../13-genesis-emulator/README.md)

## Objective
The jump to 3D-era console: MIPS R3000 (RISC + coprocessor model), GTE (vector/geometry unit), GPU (textured quads). The MIPS half is the direct overlap with your 22-09 embedded-firmware MIPS exploitation — RE the same ISA twice, from both sides.

## Tasks
- [ ] R3000 core: MIPS I ISA, delay slots (the thing that breaks every naive decoder), CP0/CP2 coprocessor interface; validate against a known core
- [ ] GTE: fixed-point vector/matrix ops, polygon clipping pipeline — the geometry unit as *hardware you code against* (pairs 15-06 symbolic-execution-style reasoning about fixed-point)
- [ ] GPU + timing: 24-bit color, textured quads, framebuffer, DMA to VRAM; HBlank/VBlank, MIPS vs GPU clock split
- [ ] MIPS security note: map the R3000's coprocessor/delay-slot model onto 22-09's MIPS ROP work (ret2c0, delay-slot gadgets) — write it up — `notes/`
- [ ] Self-check: commercial-class demo ROM (e.g., a PS1 homebrew demo) renders textured scene; timing stable

## Resources
- R3000/MIPS I spec; PSX SPX docs; your 22-09 MIPS firmware notes (the cross-read)

## Exit Criteria
- [ ] R3000 core passes MIPS test vectors — `labs/`
- [ ] Demo renders textured scene with GTE geometry — `labs/`

## Links
- [PSX SPX](https://psx-spx.consoledev.net/)
- [MIPS I spec](https://www.mips.com/wp-content/uploads/2023/02/MIPS32-ISA5-01.pdf)
