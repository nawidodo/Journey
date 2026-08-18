# 24-77 · Own VoIP client — RTP, jitter buffer, loss concealment (stretch)

**Week:** W46+ opt (stretch) · **Track:** P · **Prev:** [`../76-own-video-codec-lite`](../76-own-video-codec-lite/README.md) · **Next:** [`../78-own-container-image-builder`](../78-own-container-image-builder/README.md) · **Pairs:** 24-65, 24-40, 24-19, 22-10

## Objective
Phone calls over UDP: build a VoIP-lite stack — RTP packetization of your own 24-40 audio (payload format, timestamp/sequence — the RFC 3550 mechanics), jitter buffer (the adaptive playout delay — where VoIP audio quality lives), and loss concealment (packet-loss concealment-lite: replay/insertion — pairs 22-10 DSP). Call your own client over loopback UDP (own VMs), then the stress lab: induced jitter/loss → the quality curve (pairs 24-30 measurement, 24-51 TSDB logging).

## Tasks
- [ ] RTP: header (seq/timestamp/SSRC), payload of PCM/µ-law-lite (RFC 3550 + 3551); RTCP-lite (reports — pairs 24-17 framing)
- [ ] Jitter buffer: per-packet arrival timing, adaptive depth, playout scheduling; the underrun/overrun handling
- [ ] Loss + concealment: detect gaps (seq gaps), PLC-lite (repeat/pattern match), the audible-quality table
- [ ] Stress lab: own 24-24 NAT / tc-lite delay+jitter+loss → jitter buffer adapts, call survives (the demo); the quality-vs-conditions curve — `labs/`
- [ ] Writeup: why VoIP security differs (media encryption — pairs 20-11; RTCP eavesdrop; voicemail as attack surface) — `notes/`

## Resources
- RFC 3550/3551 (the manuals); your 24-65/24-40/22-10 code

## Exit Criteria
- [ ] Two instances talk over UDP with jitter/loss resilience — `labs/` + `code/`
- [ ] Quality curve + security writeup — `labs/` + `notes/`

## Links
- [RFC 3550 RTP](https://www.rfc-editor.org/rfc/rfc3550)
- [RFC 3551 payload types](https://www.rfc-editor.org/rfc/rfc3551)