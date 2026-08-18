# 24-27 · Own PNG decoder — file-format RE with visual payoff (stretch)

**Week:** W46+ opt (stretch) · **Track:** P · **Prev:** [`../26-own-lsm-tree`](../26-own-lsm-tree/README.md) · **Next:** [`../28-own-quantum-simulator`](../28-own-quantum-simulator/README.md)

## Objective
Decode PNGs from the spec: chunk walk, zlib streams (reuse your 24-25 decompressor — or interop zlib), filter types, interlace, CRC. The payoff is visual — decode your own screenshots pixel-perfect — and the discipline is file-format RE (pairs 12-01 PE, 13 USB descriptors, 24-11 SQLite, 21-04 artifacts). Bonus: malformed-PNG handling (the parser-fragility lesson from image-upload pentesting).

## Tasks
- [ ] Chunk layer: signature, IHDR/IDAT/IEND walk, length/type/CRC validation, unknown-chunk tolerance
- [ ] IDAT: concatenate + inflate (reuse 24-25 or link zlib), then unfilter (None/Sub/Up/Average/Paeth), scanline → pixels
- [ ] Color: palette, RGB/RGBA, bit depths; gamma/ancillary chunks tolerated; interlace (Adam7)
- [ ] Render: write raw pixels → view (or PPM) — decode your own screenshots, diff against `sips`/`ffmpeg` output
- [ ] Attack lab: malformed PNGs (bad CRC, truncated IDAT, filter-5, huge dims) — your decoder fails *cleanly* (pairs 22-xx upload pentest mindset) — `labs/`

## Resources
- PNG spec (the manual); libpng source (peer); your 24-25 notes; W3C PNG test suite

## Exit Criteria
- [ ] Decoder renders own screenshots pixel-perfect (diff-clean) — `labs/`
- [ ] Malformed-PNG matrix: clean failures, no crashes — `labs/` + `notes/`

## Links
- [PNG spec](https://www.w3.org/TR/PNG/)
- [libpng](https://libpng.sourceforge.io/)
