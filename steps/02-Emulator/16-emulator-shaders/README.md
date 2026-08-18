# 02-16 · Emulator shaders — PPU-in-compute + CRT pack (stretch)

**Week:** W18+ stretch · **Track:** B/C · **Prev:** [`../15-psp-emulator`](../15-psp-emulator/README.md)

## Objective
Your emulators software-render; your Metal layer (02-06) just blits. Close the loop: rewrite the NES/SNES PPU as a Metal compute shader (tile decode → palette → scanline in parallel), then write the CRT emulation shaders that make it look like a 1990 TV. Emulator × GPU crossover.

## Tasks
- [ ] PPU-in-compute: take 02-03 (NES) or 02-12 (SNES) PPU scanline logic → Metal compute kernel: per-tile decode, palette lookup, sprite priority — one dispatch per frame; keep CPU reference for pixel-exact diffing
- [ ] Correctness: your GPU renderer must match the CPU PPU output bit-for-bit (or documented divergence) — the "GPU is a footgun" lesson; verify on the 02-03 test ROMs
- [ ] CRT pack: scanlines, aperture grille (RGB subpixel), phosphor decay (persistence), vignette/bloom; per-mode switchable; shader params for a "tuning the TV" feel
- [ ] Perf: occupancy/divergence notes (pairs 10-07 shader perf) — where the GPU wins vs loses vs CPU PPU
- [ ] Self-check: 02-03 test ROM renders identically CPU vs GPU; CRT mode visibly differs per config (screenshot set)

## Resources
- Your 02-03/02-06/02-12 notes; MSL reference; shader toy CRT examples (read, don't copy)

## Exit Criteria
- [ ] GPU PPU matches CPU output on test ROMs — `labs/`
- [ ] CRT shader pack with ≥3 modes + before/after screenshots — `labs/`

## Links
- [Metal Shading Language](https://developer.apple.com/metal/Metal-Shading-Language-Specification.pdf)
- [CRT shader examples](https://www.shadertoy.com/view/MslGR8)
