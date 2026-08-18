# 02-15 · PSP emulator — MIPS R4000 family, VFPU, dual-core (stretch)

**Week:** W18+ stretch · **Track:** B · **Prev:** [`../14-ps1-emulator`](../14-ps1-emulator/README.md)

## Objective
The single-person ceiling of console emulation — and the same ISA family as your PS1 core. Allegrex = MIPS R4000-class main CPU + VFPU (128-bit vector/SIMD) + Media Engine (second MIPS core) + GPU. PS1 skills carry; parallelism, vector units, and closed-doc RE are the new load.

## Tasks
- [ ] Allegrex core: R4000-family extensions over your 02-14 R3000 core (MIPS III/IV bits, cache ops, privileged); reuse the R3000 skeleton, don't restart
- [ ] VFPU: 128-bit vector ops (the PSP's secret weapon — matrix/vertex math in hardware); pair with your 02-14 GTE lesson
- [ ] Dual-core: Media Engine (AV decode coprocessor) vs main CPU — synchronization, shared memory, where the timing traps live
- [ ] GPU + display: textured geometry, framebuffer, vsync; UMD-less homebrew path for testing (no disc needed — homebrew runs from memory stick)
- [ ] Self-check: commercial-class homebrew demo renders + plays audio; PPSSPP as behavioral reference, note where you diverge

## Resources
- Allegrex docs (leaked SDK docs — the "closed-doc RE" lesson vs GBATEK era); your 02-14 PS1 notes; PPSSPP source as reference

## Exit Criteria
- [ ] Allegrex core passes MIPS-family test vectors (reused/adapted from 02-14) — `labs/`
- [ ] Homebrew demo renders with VFPU geometry + audio — `labs/`

## Links
- [PPSSPP](https://github.com/hrydgard/ppsspp)
- [PSP hardware docs](https://www.psdevwiki.com/psp/)
