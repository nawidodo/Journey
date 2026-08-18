# 24-23 · Own QUIC-lite — HTTP/3-era transport from scratch (stretch)

**Week:** W46+ opt (stretch) · **Track:** P · **Prev:** [`../22-own-onion-router`](../22-own-onion-router/README.md) · **Next:** [`../24-own-nat-router`](../24-own-nat-router/README.md)

## Objective
QUIC is the era's transport — HTTP/3, half the web. Build QUIC-lite over UDP: connection IDs (the migration trick), streams with per-stream flow control, 0-RTT handshake (TLS 1.3 reuse — your 20-07), packet-number encryption. The cool lesson: what TCP got wrong (head-of-line blocking, handshake RTTs) and how QUIC fixes it.

## Tasks
- [ ] Framing: QUIC packet header, connection IDs (migration — change a client's IP mid-connection, it survives), long vs short headers
- [ ] Streams: multiplexed streams over one connection, stream/connection flow control; no HoL blocking demo vs your TCP stack (24-05)
- [ ] Handshake: TLS 1.3 over QUIC (reuse 20-07), 0-RTT — measure the RTT win vs TCP+TLS
- [ ] Loss/recovery: packet numbers, ACK frames, crypto (packet-number) protection; retransmission on your loss-injection lab
- [ ] Writeup: QUIC vs TCP+TLS RTT/loss comparison, where the web is headed (HTTP/3) — `notes/`

## Resources
- RFC 9000 (the manual); quiche source (peer); your 24-05 + 20-07 notes

## Exit Criteria
- [ ] QUIC-lite connection with streams over UDP, migration survives — `labs/`
- [ ] 0-RTT + loss-recovery working; comparison writeup — `labs/` + `notes/`

## Links
- [RFC 9000](https://www.rfc-editor.org/rfc/rfc9000)
- [quiche](https://github.com/cloudflare/quiche)
