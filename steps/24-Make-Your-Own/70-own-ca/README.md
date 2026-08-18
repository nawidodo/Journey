# 24-70 · Own CA — X.509 issuance, CSR, chain validation, revocation (stretch)

**Week:** W46+ opt (stretch) · **Track:** P · **Prev:** [`../69-own-webauthn-authenticator`](../69-own-webauthn-authenticator/README.md) · **Next:** [`../71-own-encrypted-overlay-fs`](../71-own-encrypted-overlay-fs/README.md) · **Pairs:** 20-07, 20-09, 24-69

## Objective
Every TLS handshake you built (20-07) ends at a certificate nobody fully understands. Build the trust machinery: X.509 parse + build (ASN.1/DER — your 20-07 certificate knowledge deep-dived), CSR signing, intermediate chains, CRLs/OCSP-lite, and the validation algorithm (the path-building + checks — pairs 20-07 trust store). Then the attack lab: your client validates 5 hand-picked bad certs (expired, wrong-host, unknown CA, revoked) and rejects each — with exactly the error your 20-07 server sees.

## Tasks
- [ ] ASN.1/DER: tag/length/value decode + encode (pairs 24-49 formats discipline); the X.509 fields that matter
- [ ] CA: keygen (20-09), self-signed root, CSR (parse + sign), issue leaf + intermediate; extensions (SAN, EKU, BasicConstraints)
- [ ] Validation: chain building, expiry/notBefore, hostname check (SAN), key usage; revocation check (CRL-lite + OCSP-lite)
- [ ] Attack lab: issue the 5 bad certs, validate with your client — the rejection matrix — `labs/`
- [ ] Interop: openssl verifies your certs; your client verifies openssl's (the oracle)

## Resources
- RFC 5280 (the manual); openssl x509 source (peer); your 20-07 notes

## Exit Criteria
- [ ] CA issues chains both directions-verified — `labs/`
- [ ] Bad-cert rejection matrix + validation-writeup — `labs/` + `notes/`

## Links
- [RFC 5280](https://www.rfc-editor.org/rfc/rfc5280)
- [openssl](https://www.openssl.org/)