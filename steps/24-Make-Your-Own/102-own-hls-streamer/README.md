# 24-102 · Own HLS streamer — segment + playlist + serve, the streaming stack (stretch)

**Week:** W46+ opt (stretch) · **Track:** P · **Prev:** [`../101-own-video-editor`](../101-own-video-editor/README.md) · **Pairs:** 24-98, 24-17, 24-90, 24-80

## Objective
How net video actually reaches phones: HLS — a streamer-lite — segment your 24-98 MP4 (keyframe-aligned segmentation, the GOP math from 24-76), generate the m3u8 playlist (variant/live semantics, #EXT-X-* directives), serve segments over your 24-17 HTTP (range + content-type discipline), and a live-lite loop (rolling playlist window + discontinuity tags — pairs 24-53/24-24 timing). Stress lab: your 24-80 replayer hammers the playlist → your 24-90 rate limiter + segment caching keep playback smooth (the curves), and the DRM-adjacent lesson: segment encryption-lite (AES-128 keys — 20-07), why streaming crypto is a content-protection cat-and-mouse (pairs 12-07).

## Tasks
- [ ] Segmenter: parse 24-98 timeline, split at keyframes (24-76), segment durations + timestamps; .ts or fmp4 output
- [ ] Playlist: master + media playlists, EXT-X-TARGETDURATION/EXTINF, endlist vs live variants (sliding window)
- [ ] Server: 24-17 serves segments + playlists, byte-range + MIME correctness (Content-Type the player checks)
- [ ] Encryption-lite: AES-128 segment keys + EXT-X-KEY (20-07), key rotation note
- [ ] Stress lab: 24-80 loads the playlist → 24-90 limiter + cache; player (VLC/ffplay, the oracle) stays smooth; metrics table — `labs/`
- [ ] Writeup: HLS vs DASH (why both exist), latency vs segment-size tradeoff, content-protection reality — `notes/`

## Resources
- RFC 8216 HLS (the manual); nginx-vod / ffmpeg hls muxer (peer); your 24-98/24-17/24-90 code

## Exit Criteria
- [ ] Own HLS stream plays end-to-end in ffplay/VLC under load — `labs/` + `code/`
- [ ] Latency/encryption writeup — `notes/`

## Links
- [RFC 8216](https://www.rfc-editor.org/rfc/rfc8216)
- [ffmpeg hls muxer](https://ffmpeg.org/ffmpeg-formats.html#hls-2)