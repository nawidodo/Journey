# 24-76 · Own video codec-lite — frame differencing + RLE, motion lumpiness (stretch)

**Week:** W46+ opt (stretch) · **Track:** P · **Prev:** [`../75-own-package-registry`](../75-own-package-registry/README.md) · **Next:** [`../77-own-voip`](../77-own-voip/README.md) · **Pairs:** 24-54, 24-25, 24-27/35

## Objective
Your 24-54 AVI muxes encoded frames; now encode the frames. Build a video codec-lite: frame differencing (reference + delta), RLE/entropy coding on residuals (24-25 reuse), motion estimation-lite (block search — the algorithm that makes H.264 work), and the rate/quality tradeoff you control (quantization — pairs 10-16 thinking, 20-08 GPU numbers). Codec security angle: decoder robustness (malformed bitstream — pairs 24-27 clean-failure), and why codecs are a browser/phone attack surface (pairs 08).

## Tasks
- [ ] Intra/IDR + delta frames: reference frame + residual (the difference), decode restores exactly
- [ ] RLE + Huffman/Zlib-lite on residuals (pairs 24-25); the compression table intra-vs-delta
- [ ] Motion estimation-lite: 8×8 block search (SAD), motion vectors, motion-compensated residual — smaller than plain delta (the measured win)
- [ ] Rate control: quantize residuals (Q table), the size-vs-PSNR curve (pairs 10-16 discipline)
- [ ] Robustness lab: corrupt bitstream (bit flips, truncation) — decoder degrades cleanly (blocks, no crash) — `labs/`
- [ ] Self-check: your 24-54 player plays your codec's output; the 3-way table (intra/RLE/motion) written up

## Resources
- The "Video Compression" primer + H.264 concepts (the manual); ffmpeg source (peer, encode paths); your 24-54/24-25 code

## Exit Criteria
- [ ] Codec encodes/decodes with motion-estimation win measured — `labs/` + `code/`
- [ ] Rate-distortion + robustness notes — `labs/` + `notes/`

## Links
- [Video compression primer](https://github.com/leandromoreira/digital_video_introduction)
- [ffmpeg](https://ffmpeg.org/)