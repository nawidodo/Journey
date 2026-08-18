# 24-96 · Own DNSSEC-lite — signed-zone verification, why the root holds keys (stretch)

**Week:** W46+ opt (stretch) · **Track:** P · **Prev:** [`../95-own-imap-client`](../95-own-imap-client/README.md) · **Pairs:** 24-19, 24-70, 24-66, 20-12

## Objective
Your 24-19 resolver trusts answers on the wire; production DNS does not. Build DNSSEC-lite: zone signing (your 24-70 CA keys, RRSIG over RRsets — the canonical ordering + signing rules, the format RE), DNSKEY/DS trust anchors, and a verifier in your resolver that walks RRSIG→DNSKEY→DS (key tags, algorithms, the delegation chain — pairs 24-66 hierarchy thinking). Then the attack labs that make it vivid: a poisoned A record from your own rogue zone (24-19 attack drill) — unsigned: accepted; signed: rejected, with the exact RRset/algorithm failure reported. End with a real signed zone (or your own root of trust) verified end-to-end.

## Tasks
- [ ] Signing: RRset canonical form (RFC 4034 §6.2 — the gotcha), RRSIG over RRsets, time validity (inception/expiration)
- [ ] Keys: DNSKEY/DS records, key tags, ZSK/KSK-lite split, your 24-70 CA issues them
- [ ] Verification: resolver walks RRSIG → DNSKEY → DS anchor; algorithm/hash checks (RSA/ECDSA-lite — 20-11)
- [ ] Attack lab: rogue zone injects forged A + unsigned answer → verifier outcome table (accepted/rejected + reason); expired/garbage-signature cases — `labs/`
- [ ] Glue: sign your 24-19 zone; your resolver verifies it; the trust-anchor config note — `notes/`

## Resources
- RFC 4033–4035 + 6840 (the manual); dnssec-tools (`dnssec-signzone` as oracle); your 24-19/24-70 code

## Exit Criteria
- [ ] Signed zone verified end-to-end by own resolver; forged/unsigned rejected with reasons — `labs/`
- [ ] DNSSEC design + failure-mode writeup — `labs/` + `notes/`

## Links
- [RFC 4035](https://www.rfc-editor.org/rfc/rfc4035)
- [cloudflare DNSSEC docs](https://www.cloudflare.com/dns/dnssec/how-dnssec-works/)