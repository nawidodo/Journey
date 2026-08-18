# 24-54 · Own video container — AVI/MJPEG muxer+demuxer (stretch)

**Week:** W46+ opt (stretch) · **Track:** P · **Prev:** [`../53-own-tailscale-lite`](../53-own-tailscale-lite/README.md) · **Next:** [`../55-own-3d-model-loader`](../55-own-3d-model-loader/README.md)

## Objective
Your PNG (24-27) and JPEG (24-35) decoders render stills; a container makes them move. Build an AVI (RIFF) muxer/demuxer with MJPEG frames + PCM audio (reuse your 24-40 synth for the soundtrack): chunks, index, interleaving, timing (the fps/audio-sync math). The payoff: play your own video — and the container RE discipline (RIFF, every forensic video artifact you'll parse) pairs 24-32/33/45.

## Tasks
- [ ] RIFF: chunk walking, LIST structure, AVI headers (avih/strh/strf), index (idx1); tolerance of malformed RIFF (pairs 24-27/35 clean-failure)
- [ ] Mux: interleave JPEG frames + WAV audio (from 24-40), maintain the index; playable in VLC/mpv (the oracle)
- [ ] Demux: parse your own file back → frames + audio with correct timing (the sync test); a frame-accurate player (your 10-05 renderer)
- [ ] Robustness lab: truncated/corrupt AVI (bad index, broken chunks) — clean degrade, no crash — `labs/`
- [ ] Writeup: container-vs-codec (why AVI/MKV/MP4 differ), forensic video artifacts — `notes/`

## Resources
- AVI/RIFF spec (the manual); ffmpeg source (peer — the muxing bible); your 24-27/24-35/24-40 notes

## Exit Criteria
- [ ] Own AVI plays in a real player; demux syncs — `labs/`
- [ ] Truncation-handling + writeup — `labs/` + `notes/`

## Links
- [RIFF/AVI spec](https://learn.microsoft.com/en-us/windows/win32/directshow/avi-riff-file-reference)
- [ffmpeg](https://ffmpeg.org/)
