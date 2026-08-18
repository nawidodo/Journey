# 20-07 · Build a mini-TLS — handshake + record layer, openssl-interop (stretch)

**Week:** W20 stretch · **Track:** L · **Prev:** [`../06-capstone-crypto-weak-scheme`](../06-capstone-crypto-weak-scheme/README.md)

## Objective
04-tls-internals reads TLS; build the wire protocol. Handshake (ECDHE, cert chain verify, Finished) + record layer (AEAD, sequence numbers, renegotiation), interop-tested against `openssl s_client`. The protocol half of every HTTPS C2 you'll write (12-04).

## Tasks
- [ ] Record layer: plaintext/application records, version, length, sequence numbers; then AES-GCM AEAD (TLS 1.3) vs MAC-then-encrypt (TLS 1.2 legacy) — the reason TLS 1.3 deleted it
- [ ] Handshake: ClientHello → ServerHello → ECDHE key exchange → cert chain parse + verify (pairs 20-02 RSA/ECC) → Finished (transcript hash — the "everything signed" invariant)
- [ ] TLS 1.3: PSK/0-RTT vs full handshake; why the transcript hash makes renegotiation attacks die
- [ ] Interop: your implementation negotiates with `openssl s_client -tls1_3` and serves a real request; then MITM yourself with your own proxy to prove record-level integrity checks fire
- [ ] Self-check: handshake completes against openssl both directions; tampered record rejected (bad record MAC observed)

## Resources
- RFC 8446 (TLS 1.3); your 04-tls-internals + 20-01/02 notes; openssl s_client as the peer

## Exit Criteria
- [ ] Mini-TLS completes TLS 1.3 handshake + record exchange vs openssl — `labs/`
- [ ] Tamper test log: corrupted record rejected — `labs/`

## Links
- [RFC 8446](https://www.rfc-editor.org/rfc/rfc8446)
- [openssl s_client](https://www.openssl.org/docs/manmaster/man1/openssl-s_client.html)
