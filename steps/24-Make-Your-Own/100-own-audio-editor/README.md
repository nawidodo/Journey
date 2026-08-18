# 24-100 · Own audio editor — waveform, cut/fade/effects, your DAW-lite (stretch)

**Week:** W46+ opt (stretch) · **Track:** P · **Prev:** [`../99-own-dmg-reader`](../99-own-dmg-reader/README.md) · **Next:** [`../101-own-video-editor`](../101-own-video-editor/README.md) · **Pairs:** 24-40, 22-10, 92, 89, 24-14

## Objective
Every podcast/video you edit passes through one tool class: build a DAW-lite — audio file I/O (your 24-40 WAV or Apple AIFF parsing), waveform rendering (peak/decimation math → your 24-14/24-92 canvas), transport (playhead, selection, snap), edit ops (cut/copy/paste, fade in/out — the curve math), and an effect chain (gain, EQ biquads from 22-10, echo/chorus-lite — feedback delay). The payoff is dual: it's the tool to actually finish your 24-89 TTS / 24-65 modem projects, and the A/B effects lab (before/after waveform + FFT views) trains the ear.

## Tasks
- [ ] I/O: WAV/AIFF read/write (24-40 + Mac's IFF format reuse), sample-rate/format handling
- [ ] Waveform: peak/min-max decimation per pixel, zoom/scroll, selection + playhead (24-14 TUI or 24-63 pixels)
- [ ] Edit: cut/copy/paste, crossfade (equal-power curve), snap to zero-cross (the click-prevention trick)
- [ ] Effects: gain/normalize (the meter math), 22-10 biquad EQ, echo/delay (feedback), a bypass A/B switch
- [ ] Lab: record (mic, own machine) or synthesize (24-40) → edit + chain effects → render → waveform/FFT (92) before/after — `labs/`
- [ ] Writeup: why editors anti-alias (the Nyquist 22-10 loop), sample-accurate editing notes — `notes/`

## Resources
- A DAW's edit model (Audacity source — peer); your 24-40/22-10/92/24-14 code

## Exit Criteria
- [ ] Edit + effects pipeline turns one clip into a rendered master — `labs/` + `code/`
- [ ] Effect/waveform notes — `notes/`

## Links
- [Audacity](https://www.audacityteam.org/)
- [AIFF format](https://en.wikipedia.org/wiki/Audio_Interchange_File_Format)