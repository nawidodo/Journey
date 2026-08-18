# 24-108 · Own DASH player-lite — fMP4 manifests, the HLS rival (stretch)

**Week:** W46+ opt (stretch) · **Track:** P · **Prev:** [`../107-own-subtitle-engine`](../107-own-subtitle-engine/README.md) · **Pairs:** 24-102, 24-98, 24-76, 24-17

## Objective
You streamed with HLS (102); the other half of the internet uses DASH: build a player-lite — MPD parsing (Period/AdaptationSet/Representation — the manifest RE, pairs 102 playlist discipline), segment fetching (BaseURL + segment templates, 24-17 HTTP), fMP4 segment handling (box walker from 24-98 — `moof/moof` + `mdat` the fragmented layout), adaptive bitrate (bandwidth estimation + representation switching — the 24-30 measurement + the decision policy, pairs 24-66), and playback of your own DASH-packaged 24-98 content (the loop). The stress lab: simulated bandwidth dips — ABR picks lower reps smoothly (the stair-step trace, part of the fun).

## Tasks
- [ ] MPD: XML parse (24-09-lite), period/adaptation/representation tree, segment template math (timeline + numbers)
- [ ] fMP4: moof/traf/trun → sample offsets + durations (24-98 box depth), init segment (`moov`) + media segments
- [ ] Fetch: 24-17 range requests, buffer levels (the buffer manager — pairs 24-77 jitter-buffer thinking)
- [ ] ABR: throughput estimator (24-30), representation selection policy, seamless switch (keyframe alignment — 24-76 GOP)
- [ ] Lab: package your 24-98 output as DASH, play with/without bandwidth shaping (tc/netem-lite); ABR trace table at 3 bitrates — `labs/`
- [ ] Writeup: DASH vs HLS (why both, CMAF convergence), ABR challenges (bufferbloat) — `notes/`

## Resources
- ISO 23009-1 MPD spec (the manual); dash.js source (peer — the ABR logic); your 24-102/24-98/24-17 code

## Exit Criteria
- [ ] Own MPD + fMP4 streams play with ABR switching — `labs/` + `code/`
- [ ] ABR/segment writeup — `notes/`

## Links
- [ISO 23009-1](https://www.iso.org/standard/79329.html)
- [dash.js](https://github.com/Dash-Industry-Forum/dash.js)