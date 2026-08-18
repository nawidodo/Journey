# 24-89 · Own TTS — formant speech synthesis from your synth (stretch)

**Week:** W46+ opt (stretch) · **Track:** P · **Prev:** [`../88-own-ldap-lite`](../88-own-ldap-lite/README.md) · **Pairs:** 24-40, 22-10, 24-77

## Objective
Text-to-speech with no ML: formant synthesis — the engineering that made "robots talk" for 50 years (Klatt's synthesizer, the DECtalk voice). Vowel formants (F1/F2/F3 filter banks), consonants (noise bursts), pitch contour (prosody), and a phoneme-to-waveform pipeline from your own 24-40 synth + 22-10 filter skills. The delight: your computer says "Journey" with a voice you designed. The security note: voice as biometric surface (pairs 26-BT audio, deepfake fallout), and why synthesis-to-asr is an attack (your 24-79 ML detector thinking in reverse).

## Tasks
- [ ] Source: glottal pulse train (24-40 oscillator) + noise (fricatives); the voiced/unvoiced switch
- [ ] Filter: formant resonators (simple IIR biquads — 22-10), map phonemes → formant targets, transitions
- [ ] Prosody: pitch/amplitude contour, syllable timing; utterance assembly from a phoneme string
- [ ] Lab: synthesize the same sentence with 2 parameter sets; ABX yourself on intelligibility; visualize spectrogram (22-10) — `labs/`
- [ ] Writeup: formant vs concatenative vs neural TTS (where the field went), voice-biometric caveats — `notes/`

## Resources
- Klatt's JASA paper (the manual); the DECtalk docs/emulation projects (peer); your 24-40/22-10 code

## Exit Criteria
- [ ] Phonemes → audible intelligible speech from your synth — `labs/` + `code/`
- [ ] Intelligibility + design writeup — `notes/`

## Links
- [Klatt synthesizer paper](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC3468617/)
- [DECtalk](https://en.wikipedia.org/wiki/DECtalk)