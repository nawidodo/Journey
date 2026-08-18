# 24-91 · Own supply-chain signer — in-toto-lite: metadata + verify your pipeline (stretch)

**Week:** W46+ opt (stretch) · **Track:** P · **Prev:** [`../90-own-rate-limiter`](../90-own-rate-limiter/README.md) · **Next:** [`../92-own-fft-visualizer`](../92-own-fft-visualizer/README.md) · **Pairs:** 24-78, 24-75, 24-70, 24-39

## Objective
The xz/solarwinds lesson: the artifact you run may not be the one you built. Build the answer — an in-toto-lite: attestation metadata (who/what produced the artifact — your 24-78 builder, 24-75 registry push, your own 24-70 CA signing chain), a verify tool that walks your pipeline's metadata and rejects anything not signed by the trusted steps, and the tamper lab: re-sign, swap a layer, replay old artifact metadata — each caught. The honest endpoint: your own 24-78 CI output verified end-to-end by your own tool (pairs 24-39 hashing discipline, 24-58 provenance).

## Tasks
- [ ] Layout: pipeline definition (steps + expected artifact states — the in-toto model), link metadata per step
- [ ] Sign: SHA-256 artifact digests (24-39) + your 24-70 CA keys, metadata JSON (24-88 format discipline)
- [ ] Verify: walk layout → links, check signatures, check artifact digests, order integrity (attestation chain)
- [ ] Tamper lab: swap layer blob, forge link, replay old metadata — verify rejects each with the exact reason — `labs/`
- [ ] Wire-in: your 24-78 image builder emits metadata, your 24-80/24-83 tooling consumes it — the end-to-end run

## Resources
- in-toto spec (the manual); SLSA levels (the grading you'll cite); your 24-70/24-39/24-78 code

## Exit Criteria
- [ ] Own pipeline artifacts verified end-to-end; tamper cases rejected — `labs/`
- [ ] SLSA/in-toto writeup — `notes/`

## Links
- [in-toto](https://in-toto.io/)
- [SLSA](https://slsa.dev/)