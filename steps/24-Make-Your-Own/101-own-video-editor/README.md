# 24-101 · Own video editor-lite — timeline, cuts, transitions, your no-NLE (stretch)

**Week:** W46+ opt (stretch) · **Track:** P · **Prev:** [`../100-own-audio-editor`](../100-own-audio-editor/README.md) · **Next:** [`../102-own-hls-streamer`](../102-own-hls-streamer/README.md) · **Pairs:** 24-98, 24-54, 24-76, 24-63

## Objective
Your 24-98 muxes, your 24-76 encodes: now edit — a NLE-lite — clip model (source ranges → timeline tracks, the two-track video/audio mental model), preview (decode frames via your 24-54/98 readers), edit ops (cut/trim/ripple, track alignment), transitions (crossfade/dissolve — the frame-blend math), and export back through your 24-98 muxer. Ends with the real test: edit your own 24-76/24-89 test clips into a short film your mp4 player plays — and the frame-accurate export check (pairs 24-30 precision discipline).

## Tasks
- [ ] Model: source clips + in/out points, timeline with tracks, clip graph (the data structure — recursive edit edits, pairs 24-72 DAG thinking)
- [ ] Preview: seek+decode frames (24-54/98 unmux), scrubbing rate; audio track playback (24-40)
- [ ] Editing: cut/trim/ripple/slip, track layering + visibility, the undo stack (24-38 patterns)
- [ ] Transitions: hard cut → crossfade (frame blend weights), duration/spacing rules
- [ ] Export: render frames → encode (24-76) → mux (24-98) with correct timestamps; play + compare frame-accurately (24-30) — `labs/`
- [ ] Writeup: why editors use proxy/mezzanine formats, keyframe-aware cutting (24-76 GOP) — `notes/`

## Resources
- NLE architecture notes (Shotcut/Kdenlive source — peer); your 24-98/24-54/24-76 code

## Exit Criteria
- [ ] Multi-clip timeline exports to playable MP4 with transitions — `labs/` + `code/`
- [ ] Frame-accuracy + format writeup — `notes/`

## Links
- [Kdenlive](https://kdenlive.org/)
- [Shotcut](https://shotcut.org/)