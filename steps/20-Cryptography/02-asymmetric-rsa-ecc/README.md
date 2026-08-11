# 20-02 · Asymmetric Crypto: RSA + ECC

**Week:** W15–16 · **Track:** L · **Prev:** [`../01-symmetric-ciphers-modes`](../01-symmetric-ciphers-modes/README.md) · **Next:** [`../03-hashes-macs-side-channels`](../03-hashes-macs-side-channels/README.md)

## Objective
The math behind RSA/ECC and why bad parameters = broken crypto. Feeds implant comms (Track E), TLS (20-04), and reading exploit writeups.

## Tasks
- [ ] RSA: keygen, encryption/signing, why padding (OAEP/PSS) is mandatory; textbook RSA attacks
- [ ] Attacks: small `e`, small `d`, shared `n`, Franklin-Reiter, low exponent broadcast (Håstad), Wiener
- [ ] ECC: group law, curves (P-256, Curve25519), ECDH/ECDSA; why ECDSA nonce reuse is fatal (k-reuse → private key)
- [ ] Implement a toy RSA and ECDH in `code/`; break a deliberately-weak scheme in `labs/`
- [ ] CryptoPals Set 5 (RSA) + Set 6 (DSA nonce reuse)

## Resources
- *Serious Cryptography* ch.5–6, 8; *Cryptography Engineering* (Ferguson) ch. on RSA
- Cryptopals sets 5–6
- "RSA and ECDSA attacks" CTF writeups (cryptohack.org)

## Exit Criteria
- [ ] One RSA attack + ECDSA nonce-reuse key recovery — `labs/`
- [ ] "Why padding matters" notes — `notes/`
