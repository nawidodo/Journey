# 24-98 · Own MP4/QuickTime muxer — ISOBMFF boxes, the modern movie (stretch)

**Week:** W46+ opt (stretch) · **Track:** P · **Prev:** [`../97-own-mach-o-parser`](../97-own-mach-o-parser/README.md) · **Next:** [`../99-own-dmg-reader`](../99-own-dmg-reader/README.md) · **Pairs:** 24-76, 24-40, 24-54, 24-89

## Objective
Your 24-54 AVI is 1992; the internet ships ISOBMFF: build an MP4 muxer-lite — box tree (ftyp/moov/trak/mdat, box sizes + nesting rules), sample tables (stbl: stts/stsc/stsz/stco — the offsets math, the core skill), timebases (timescale/durations — how players sync), and encode you own: your 24-76 codec-lite stream (or MJPEG from 24-54) + your 24-40/24-89 audio, muxed into one `.mp4` that QuickTime/VLC/ffprobe opens — the interop oracle. Then the RE side: parse a real mp4 (your phone's video) with your box walker, and the malformed-box lab (bad stco, long sizes — pairs 24-27) for the decoder-robustness lesson.

## Tasks
- [ ] Box tree: full/size/type fields, container recursion, the `moov/mdat` layout; big-endian discipline (pairs 24-54)
- [ ] Sample tables: stts (decoding deltas), stsc/stsz, stco/stco64 — chunk/sample offsets (the math that makes or breaks files)
- [ ] Timescale: mvhd/tkhd/mdhd durations, edit lists-lite, track sync (the AV sync model — pairs 24-89/24-77 timing)
- [ ] Mux: your codec-lite or MJPEG video track + PCM/µ-law audio track → valid mp4; ffprobe validates the box tree (oracle)
- [ ] Parse+robustness lab: box-walk a real phone mp4; corrupt stco/long-size → your parser fails clean, ffmpeg's behavior comparison — `labs/`
- [ ] Writeup: why the internet standardized on MP4 (streaming/IP protection, 24-102 HLS ahead), fragmentation (fmp4) notes — `notes/`

## Resources
- ISO 14496-12 (the manual — box names every player shares); ffmpeg's mov.c (peer, the compat oracle); your 24-54/24-76/24-40 code

## Exit Criteria
- [ ] Own video+audio muxes to playable MP4 verified by ffprobe/VLC — `labs/` + `code/`
- [ ] Table math + robustness writeup — `labs/` + `notes/`

## Links
- [ISO 14496-12/BMFF](https://www.iso.org/standard/74428.html)
- [MP4 box structure (wiki)](https://wiki.multimedia.cx/index.php/ISO_Base_Media_File_Format)