# 24-92 · Own FFT visualizer — your synth's output, painted by math (stretch)

**Week:** W46+ opt (stretch) · **Track:** P · **Prev:** [`../91-own-supply-chain-signer`](../91-own-supply-chain-signer/README.md) · **Pairs:** 24-40, 22-10, 24-14, 24-63

## Objective
The classic payoff: your 24-40 synth plays, bars dance. Build an FFT analyzer — real-valued FFT (radix-2 Cooley–Tukey — the math you've used since 22-10), windowing (Hann, the spectral-leakage fix), log-frequency mapping into bands, and a 24-14 TUI / 24-63 pixel renderer painting the spectrum live. Verification is the fun part: play known tones (pairs 24-65 DTMF!) — your analyzer's peaks land on the exact bins (the numeric oracle). Then the security spin: FFT as the basis of 24-50 logic analysis, 24-77 audio, and spectrogram-based detection (pairs 24-79 data thinking).

## Tasks
- [ ] FFT: radix-2, complex math from scratch (bit-reversal + butterflies), sample sizes/power-of-two
- [ ] Windowing: Hann/Blackman, the leakage demo (unwindowed vs windowed spectrum)
- [ ] Analyzer: magnitude → dB, bands, peak tracking; live source: your 24-40 synth + microphone (own machine)
- [ ] Render: 24-14 ANSI bars / 24-63 pixels; refresh loop, hold/peak markers (retro spectrum look)
- [ ] Lab: DTMF tones from 24-65 → peaks at 697/770/852/941 Hz bins (the exact oracle); tone-pair classification — `labs/`
- [ ] Writeup: FFT everywhere (analyzers, codecs 24-76, modems 24-65), spectral-attacks thinking (side channels) — `notes/`

## Resources
- The FFT chapter of your DSP manual; KISS FFT / rfft source (peer); your 24-40/22-10/24-14 code

## Exit Criteria
- [ ] Live spectrum of known tones with exact-bin verification — `labs/` + `code/`
- [ ] Notes on windows/bands/uses — `notes/`

## Links
- [FFT primer](https://jakevdp.github.io/blog/2013/08/28/understanding-the-fft/)
- [The Scientist and Engineer's Guide to DSP (ch. 12)](https://www.dspguide.com/ch12.htm)