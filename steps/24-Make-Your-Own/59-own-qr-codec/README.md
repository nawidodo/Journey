# 24-59 · Own QR codec — encoder + decoder, Reed–Solomon error correction (stretch)

**Week:** W46+ opt (stretch) · **Track:** P · **Prev:** [`../58-own-object-storage`](../58-own-object-storage/README.md) · **Next:** [`../60-own-spam-filter`](../60-own-spam-filter/README.md) · **Pairs:** 24-27, 24-35, 24-49, 20-crypto GF(256)

## Objective
The most-parsed format in the world you've never parsed. Build a full QR codec: encode (data → bitstream → Reed–Solomon ECC → module placement with masks + finder/alignment patterns) and decode (image → modules → RS error correction → payload). The RS decoder is the prize: GF(256) arithmetic (the same field as AES — pairs 20-09/11), syndromes, Berlekamp–Massey — error correction you can hold in your hand (put a speck on it, it still scans).

## Tasks
- [ ] Encode: byte/alphanumeric modes, version/error-level selection, mask evaluation (penalty scoring)
- [ ] RS: GF(256) math, generator polynomials, encode ECC codewords (pairs 20-11 field operations)
- [ ] Decode: binarize (your 24-49/24-35 image skills), finder detection, perspective + sampling grid (pairs 10-05), unmask, syndrome + Berlekamp–Massey correction
- [ ] Lab: encode → print/screen → decode with your own decoder; add damage (specks, rotation) → RS corrects; decode real-world QR codes — `labs/`
- [ ] Writeup: why RS (vs CRC), QR payload abuse (malicious URL, the "scan this" attack surface) — `notes/`

## Resources
- ISO/IEC 18004 (the manual — or the Thonky tutorial); ZXing/zbar source (peer); your 24-27/24-35/24-49 notes

## Exit Criteria
- [ ] Own encode→decode roundtrip + real QR decode — `labs/`
- [ ] Damage-correction demo + RS writeup — `labs/` + `notes/`

## Links
- [Thonky QR tutorial](https://www.thonky.com/qr-code-tutorial/)
- [ZXing](https://github.com/zxing/zxing)
