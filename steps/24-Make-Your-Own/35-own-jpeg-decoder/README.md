# 24-35 · Own JPEG decoder — DCT, Huffman, chroma (stretch)

**Week:** W46+ opt (stretch) · **Track:** P · **Prev:** [`../34-own-mail-server`](../34-own-mail-server/README.md)

## Objective
PNG was the easy one. JPEG is the hard one: SOI/SOF/SOS markers, Huffman tables (reuse 24-25), quantized DCT (the math), chroma subsampling, zigzag. The payoff is visible — decode your own photos pixel-perfect — and the discipline is file-format RE at full complexity (pairs 24-27 PNG, 12-01 PE). Malformed-JPEG handling closes the parser-fragility loop from image-upload pentesting.

## Tasks
- [ ] Markers: SOI/APPn/SOF/SOS/EOI, segment walk, DQT/DHT definitions, restart intervals
- [ ] Entropy: baseline Huffman decode of MCUs (reuse your 24-25 canonical Huffman); progressive mode stretch
- [ ] Math: dequantize → inverse DCT (your first serious signal math — pairs 22-10 DSP) → chroma upsampling (4:2:0) → YCbCr→RGB
- [ ] Render: decode your own photos, diff vs `sips`/ImageMagick (the oracle); profile where time goes (your 24-30 profiler)
- [ ] Attack lab: malformed JPEGs (bad Huffman codes, truncated scan, dimension bombs) — clean failures, no crashes (pairs 22-xx upload pentest) — `labs/`

## Resources
- ITU T.81 (the manual); libjpeg-turbo source (peer); your 24-25/24-27/22-10 notes

## Exit Criteria
- [ ] Baseline JPEG decodes own photos pixel-diff-clean — `labs/`
- [ ] Malformed-JPEG clean-failure matrix — `labs/` + `notes/`

## Links
- [ITU T.81](https://www.w3.org/Graphics/JPEG/itu-t81.pdf)
- [libjpeg-turbo](https://libjpeg-turbo.org/)
