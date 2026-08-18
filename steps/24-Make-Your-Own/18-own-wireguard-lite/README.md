# 24-18 · Own WireGuard-lite — Noise IK + ChaCha20Poly1305 + tun (stretch)

**Week:** W46+ opt (stretch) · **Track:** P · **Prev:** [`../17-own-http-server`](../17-own-http-server/README.md) · **Next:** [`../19-own-dns-resolver`](../19-own-dns-resolver/README.md)

## Objective
WireGuard is the era's VPN — a 4k-line kernel module. Build a lite version: Noise-IK handshake, ChaCha20Poly1305 transport, a tun device carrying IP. The security lessons: why static-noise beats TLS here, replay protection (the counter), and what real WireGuard's "cryptokey routing" design means.

## Tasks
- [ ] Handshake: Noise IKpsk2 (or IK) — initiator/responder state machine, DH + HKDF transcript; interop: handshake with real `wg` if feasible (or own peer only)
- [ ] Transport: ChaCha20Poly1305 per-packet AEAD (pairs 20-01/03), monotonic counter replay protection, key rotation on rekey
- [ ] Data path: tun device (Linux, or utun on macOS) — IP in, encrypted out, peer-to-peer only (own lab)
- [ ] Attack lab: replay/cut-and-paste packets rejected, counter wrap, handshake MITM fails (Noise identity binding)
- [ ] Writeup: WireGuard vs IPsec/TLS-VPN tradeoffs — `notes/`

## Resources
- WireGuard whitepaper + protocol spec (the manual); Noise spec; your 20-03/07 notes

## Exit Criteria
- [ ] Two peers exchange IP traffic over your tunnel — `labs/`
- [ ] Replay/MITM rejected + writeup — `labs/` + `notes/`

## Links
- [WireGuard protocol](https://www.wireguard.com/protocol/)
- [Noise spec](https://noiseprotocol.org/)
