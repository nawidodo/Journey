# 24-113 · Own noise reduction — spectral gating, the podcast cleanup (stretch)

**Week:** W46+ opt (stretch) · **Track:** P · **Prev:** [`../112-own-game-cheat-tool`](../112-own-game-cheat-tool/README.md) · **Next:** [`../114-own-image-stabilizer`](../114-own-image-stabilizer/README.md) · **Pairs:** 92, 22-10, 24-100, 24-89

## Objective
Your 24-100 editor cuts; podcasts shine because noise dies: build a denoiser-lite — STFT (your 92 FFT over windows), noise profile capture (the "record 1s of room tone" step — sampling the noise floor), spectral gating (per-bin gain: keep speech bins, kill noise bins — the threshold/release policy), and overlap-add reconstruction. Then the honest test: record your own voice with fan hum (own machine), denoise, and measure — SNR before/after (24-30 metric discipline) + the FFT view of the noise floor (92) before/after. The lesson that transfers: spectral masking is the same math as steganography removal (pairs 20-08), and every VoIP/codec tool does this (pairs 24-77/76).

## Tasks
- [ ] STFT: window + FFT (92) + overlap-add (the inverse half-overlap dance); the window choice tradeoff (92 Hann notes)
- [ ] Profile: noise-floor snapshot from a tone-only capture (learning the floor — the 24-79 data-thinking moment)
- [ ] Gate: per-bin gain function (spectral subtraction-lite / Wiener-lite), attack/release smoothing (no musical-noise — the classic artifact), floor clamp
- [ ] Measure: SNR estimator (signal/noise power over regions), before/after table + spectrogram images (92 render) — `labs/`
- [ ] Writeup: why spectral processing artifacts sound bad (musical noise), where this appears in products (25-04? no — in ASR/voip pipelines vs 24-89 TTS) — `notes/`

## Resources
- The DSP noise-reduction chapter (manual — spectral subtraction literature); Audacity's noise-reduction source (peer); your 92/22-10/24-100 code

## Exit Criteria
- [ ] Denoised clip with measured SNR gain — `labs/` + `code/`
- [ ] Algorithm + artifact writeup — `notes/`

## Links
- [Spectral subtraction paper](https://www.cs.cmu.edu/~robust/Papers/KiLuMa84.pdf)
- [Audacity noise reduction](https://manual.audacityteam.org/man/noise_reduction.html)