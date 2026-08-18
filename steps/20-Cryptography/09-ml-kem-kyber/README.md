# 20-09 · ML-KEM (Kyber) from the spec — post-quantum, test-vector verified (stretch)

**Week:** W20 stretch · **Track:** L · **Prev:** [`../08-gpu-password-cracker`](../08-gpu-password-cracker/README.md)

## Objective
NIST standardized ML-KEM (FIPS 203) in 2024 — the era's crypto. Implement from the spec: NTT (the polynomial transform), Kyber.PKE (IND-CPA) → Kyber.KEM (IND-CCA2 via Fujisaki-Okamoto), verify against the official test vectors, interop with `openssl`.

## Tasks
- [ ] Ring math: Z_q[x]/(x^256+1), NTT + inverse (the modular arithmetic core — pairs 20-02 RSA number theory, 3× the algebra)
- [ ] ML-KEM-PKE: keygen, encrypt (with noise sampling), decrypt; then KEM: encapsulate/decapsulate + FO transform (why CCA2 conversion matters — the "rejection sampling is not optional" lesson)
- [ ] Vectors: pass FIPS 203 test vectors (keygen/encaps/decaps deterministic values) — your oracle
- [ ] Interop: exchange with `openssl genpkey -algorithm ML-KEM-768` / a peer implementation; size/perf notes vs RSA/ECDH
- [ ] Writeup: what breaks in a post-quantum world (harvest-now-decrypt-later) — pairs TLS 1.3 hybrid handshakes (20-07) — `notes/`

## Resources
- FIPS 203 (the manual); NIST sample vectors; openssl ML-KEM support (3.5+); your 20-02 notes

## Exit Criteria
- [ ] ML-KEM-768 passes FIPS 203 vectors — `labs/`
- [ ] Interop with openssl peer — `labs/`

## Links
- [FIPS 203](https://nvlpubs.nist.gov/nistpubs/FIPS/NIST.FIPS.203.pdf)
- [NIST test vectors](https://github.com/usnistgov/ACVP-Server)
