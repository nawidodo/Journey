# 24-25 · Own compression — Huffman → LZ77 → DEFLATE, gzip interop (stretch)

**Week:** W46+ opt (stretch) · **Track:** P · **Prev:** [`../24-own-nat-router`](../24-own-nat-router/README.md) · **Next:** [`../26-own-lsm-tree`](../26-own-lsm-tree/README.md)

## Objective
Compression is everywhere and covered nowhere yet in the plan — zlib, gzip, DEFLATE run every HTTP response, firmware image, and malware packer. Build it: Huffman coding → LZ77 sliding window → DEFLATE (the real format), interop with real gzip/zlib as your oracle. Security payoff: decompression bombs (pairs 22-xx image-upload pentesting), zlib CVEs, and how packers 12-10 hide entropy.

## Tasks
- [ ] Huffman: canonical Huffman codes, encode/decode; bit-level I/O
- [ ] LZ77: sliding-window match finder (hash chains), length/distance pairs; ratio benchmark vs real gzip
- [ ] DEFLATE: static + dynamic Huffman blocks, block format; **write real .gz files `gzip` opens, and decode real gzip** — the oracle
- [ ] Attack lab: decompression bomb (zip-of-zips / high-ratio payload) — your decoder with a sane output cap; document the ratio limit lesson (pairs 22-09 image-upload pentest)
- [ ] Writeup: entropy view of packed malware (pairs 12-10 own-packer) — `notes/`

## Resources
- RFC 1950–51 (the manual); zlib source (peer); the "decompression bomb" writeups

## Exit Criteria
- [ ] Own DEFLATE: .gz interop both directions — `labs/`
- [ ] Bomb-capped decoder + writeup — `labs/` + `notes/`

## Links
- [RFC 1951 (DEFLATE)](https://www.rfc-editor.org/rfc/rfc1951)
- [zlib](https://zlib.net/)
