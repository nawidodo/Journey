# 24-69 · Own WebAuthn authenticator — FIDO2-lite: CBOR, challenge-response, attestation (stretch)

**Week:** W46+ opt (stretch) · **Track:** P · **Prev:** [`../68-own-terminal-multiplexer`](../68-own-terminal-multiplexer/README.md) · **Next:** [`../70-own-ca`](../70-own-ca/README.md) · **Pairs:** 24-64, 20-12, 20-09

## Objective
Passkeys are WebAuthn: your 24-64 HD-wallet key-management discipline meets a challenge-response protocol. Build an authenticator-lite: CBOR encode/decode (the wire format), registration (credential creation + keygen), assertion (challenge signature), resident keys + user verification, and attestation (the "I'm a real device" claim — pairs 24-70 CA). The security layer is the point: phishing resistance (no shared secrets to phish), why origin-binding matters, and the authenticator threat model (pairs 20-12).

## Tasks
- [ ] CBOR: encoder/decoder (the map/byte-string rules) — the format RE (pairs 24-27/49 discipline)
- [ ] Registration: generate keypair (reuse 24-64 secp256k1/P-256), credential ID, store; assertion: sign challenge with SHA-256/AES-CBC (20-07 hashing)
- [ ] Client/device split: your "browser" talks to your "authenticator" over a local transport (the CTAP-lite protocol — your own wire format)
- [ ] Security lab: replay challenge → rejected (the point); origin swap → rejected; phishing demo — credential bound to origin — `labs/`
- [ ] Interop: a real WebAuthn test page with your authenticator via a small WebExtension/proxy (the oracle — stretch)

## Resources
- WebAuthn spec + the CBOR RFC (the manuals); your 24-64/20-12/20-09 code

## Exit Criteria
- [ ] Register + assert flow passes challenge/replay/origin tests — `labs/` + `code/`
- [ ] Threat-model + phishing-resistance writeup — `notes/`

## Links
- [WebAuthn spec](https://www.w3.org/TR/webauthn-2/)
- [CBOR RFC 8949](https://www.rfc-editor.org/rfc/rfc8949)