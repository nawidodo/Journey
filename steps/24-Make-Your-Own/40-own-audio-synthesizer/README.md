# 24-40 · Own audio synthesizer — oscillators, envelopes, DSP (stretch)

**Week:** W46+ opt (stretch) · **Track:** P · **Prev:** [`../39-own-package-manager`](../39-own-package-manager/README.md) · **Next:** [`../41-own-search-engine`](../41-own-search-engine/README.md)

## Objective
The fun side of the DSP you started in 22-10: a synthesizer — oscillators (sine/saw/square with aliasing awareness), envelopes (ADSR), filters (the biquad), effects (delay/reverb), render to WAV. The discipline is the same as every real-time system: sample-rate math, buffer management, and the aliasing lesson that pairs 22-10's FFT work.

## Tasks
- [ ] Core: WAV writer, sample buffer pipeline, oscillators with phase accumulators; anti-aliasing (the naive saw *will* alias — hear it)
- [ ] Voices: ADSR envelopes, note events → voice allocation; a simple polyphonic keyboard input
- [ ] Filters/effects: biquad lowpass (the transfer function math), delay with feedback; a playable patch
- [ ] Render: compose a track to WAV (your own music), plus a scope view (visualize — pairs 22-10 FFT)
- [ ] Writeup: aliasing, sample-rate conversion, where real-time audio and embedded DSP (22-10) meet — `notes/`

## Resources
- "The Audio Programming Book"; Will Pirkle's DSP books; your 22-10 notes

## Exit Criteria
- [ ] Polyphonic synth renders a WAV track with envelopes + filters — `labs/`
- [ ] Aliasing/dsp writeup — `notes/`

## Links
- [The Audio Programming Book](https://mitpress.mit.edu/9780262014465/the-audio-programming-book/)
- [Will Pirkle DSP](https://www.willpirkle.com/)
