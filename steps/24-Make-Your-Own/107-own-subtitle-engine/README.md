# 24-107 · Own subtitle engine — SRT/VTT parse + render into your player (stretch)

**Week:** W46+ opt (stretch) · **Track:** P · **Prev:** [`../106-own-chiptune-sequencer`](../106-own-chiptune-sequencer/README.md) · **Next:** [`../108-own-dash-player`](../108-own-dash-player/README.md) · **Pairs:** 24-98, 24-101, 24-49, 24-63

## Objective
Your 24-101 player has picture and sound; captions are the third track: build a subtitle engine — SRT (the timestamp/CRLF parsing, the companion-file dance) and WebVTT (cue structure, `WEBVTT` header, settings-lite), timing model (cue windows vs media time, 24-98 timescale thinking), and rendering: text composited over frames (24-49 glyph rasterization → your 24-63 canvas), resync/fuzzing tolerance, and an encode path (burn-in captions into your exported 24-98 MP4). Then the security note: subtitle-file exploits are real (pairs 08 parser surfaces) — your malformed-cue lab (bad timestamps, huge text) degrades cleanly (24-27 robustness).

## Tasks
- [ ] SRT: block parse (index, start→end, text), CRLF/encoding tolerance; VTT: header, cues, settings (position/align)
- [ ] Timing: cue windows, overlap rules, offset/shift controls (sync fix); media-time alignment (24-98)
- [ ] Render: 24-49 glyphs → framebuffer text, background box, wrap; burn-in during 24-101/98 export or overlay live
- [ ] Robustness lab: malformed cues (missing end, negative duration, huge block, binary junk) → skip/repair, never crash; the fuzz-style sweep — `labs/`
- [ ] Interop: your engine renders a real VTT/SRT from your own 24-101 output (oracle) — `labs/`
- [ ] Writeup: subtitle attack history (the parser is a trust boundary — 08), a11y rationale — `notes/`

## Resources
- SRT/VTT spec notes + libass source (peer — the reference renderer); your 24-98/24-49/24-63 code

## Exit Criteria
- [ ] SRT/VTT cues render over your video, burn-in works — `labs/` + `code/`
- [ ] Robustness + attack writeup — `notes/`

## Links
- [WebVTT (W3C)](https://www.w3.org/TR/webvtt1/)
- [libass](https://github.com/libass/libass)