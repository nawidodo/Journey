# 20-06 · Capstone: Break a Deliberately-Weak Scheme 🚩 M21

**Week:** W19–20 · **Track:** L · **Prev:** [`../05-os-crypto-stores`](../05-os-crypto-stores/README.md)

## Objective
Everything together: a multi-layer "product-grade" weak scheme you must break end-to-end. Proves the phase.

## Tasks
- [ ] Design a weak scheme yourself first (in `notes/`): CBC + predictable IV + keyed hash built from raw SHA-1 + a naive RSA signature check
- [ ] Implement it as a service in `code/` (a network listener or file format)
- [ ] Break it with the phase's tools: padding oracle for confidentiality, length extension for integrity, RSA padding flaw for authentication
- [ ] Full writeup: scheme → each flaw → exploit primitive → recovered secret — `notes/`
- [ ] Defensive mirror: write the "what we should have done" re-design (AEAD + HKDF + Ed25519)

## Resources
- Your 20-01–05 outputs; CryptoPals remaining sets
- Real analog: this is how old secure-channel protocols failed — read one real post-mortem (e.g., an OTR/SSH/OpenSSL-era flaw)

## Exit Criteria
- [ ] **M21: weak scheme broken end-to-end** — `labs/`
- [ ] Re-design writeup — `notes/`

## Links
- [CryptoPals (remaining sets)](https://cryptopals.com/)
- [CryptoHack](https://cryptohack.org/)
