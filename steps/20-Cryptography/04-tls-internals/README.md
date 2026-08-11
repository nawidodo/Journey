# 20-04 · TLS Internals + Real-World Flaws

**Week:** W17–18 · **Track:** L · **Prev:** [`../03-hashes-macs-side-channels`](../03-hashes-macs-side-channels/README.md) · **Next:** [`../05-os-crypto-stores`](../05-os-crypto-stores/README.md)

## Objective
Read TLS 1.2/1.3 handshakes like a protocol — the transport layer every C2 channel and every app you'll reverse rides on.

## Tasks
- [ ] TLS 1.2: handshake, cipher suite negotiation, key exchange (ECDHE), certificate chain validation
- [ ] TLS 1.3: why it changed (RSA key exchange removed, 0-RTT risks, downgrade protection)
- [ ] Capture + decode a real TLS session (openssl s_client / Wireshark with keys) — trace every message — `labs/`
- [ ] Attacks: Bleichenbacher (1.2 padding), POODLE, BEAST/CRIME, ALPACA, 0-RTT replay — one deep-dive each in `notes/`
- [ ] Certificates: chain building, name constraints, CT logs, revocation (OCSP/CRL) — what a checker must get right

## Resources
- RFC 5246, RFC 8446; Cloudflare TLS 1.3 explainer
- Wireshark TLS dissection docs; `openssl s_client -msg`
- P0 / TLS research writeups (Bleichenbacher 2.0, ALPACA)

## Exit Criteria
- [ ] Handshake decoded message-by-message from a live capture — `labs/`
- [ ] "TLS attack history" one-pager — `notes/`

## Links
- [RFC 8446 (TLS 1.3)](https://www.rfc-editor.org/rfc/rfc8446)
- [Cloudflare TLS 1.3 explainer](https://blog.cloudflare.com/tls-1-3-overview-and-q-and-a/)
