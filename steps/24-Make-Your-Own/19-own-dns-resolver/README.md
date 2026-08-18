# 24-19 · Own DNS resolver — wire format + cache, poisoning defense (stretch)

**Week:** W46+ opt (stretch) · **Track:** P · **Prev:** [`../18-own-wireguard-lite`](../18-own-wireguard-lite/README.md) · **Next:** [`../20-own-packet-sniffer`](../20-own-packet-sniffer/README.md)

## Objective
DNS is the internet's address book — and a giant attack surface. Build a recursive resolver: wire format (message/name compression, RR encoding), iterative resolution, a cache. Then defend it: Kaminsky-style cache poisoning, transaction-ID + source-port randomization, 0x20-case encoding, DNSSEC verification. The era's DNS-over-HTTPS shift documented.

## Tasks
- [ ] Wire: parse/build DNS messages, name compression, A/AAAA/CNAME/NS/SOA; query a real resolver, diff with dig output
- [ ] Resolver: iterative walk root → TLD → authoritative; cache with TTLs; concurrent queries
- [ ] Attack lab: cache poisoning — Kaminsky birthday-style race vs your cache; defenses: random source ports + case randomization; DNSSEC (validate a signed zone) — `labs/`
- [ ] Writeup: DNS attack taxonomy (spoofing, NXDOMAIN injection, DNS rebinding — pairs 21-07 hunting, 08 browser) — `notes/`

## Resources
- RFC 1035 (the manual); the Kaminsky paper; DNSSEC RFCs; your 24-05 TCP/IP notes

## Exit Criteria
- [ ] Resolver answers real names via cache, dig-compatible — `labs/`
- [ ] Poisoning attack → defended (ports/case/DNSSEC), writeup — `labs/` + `notes/`

## Links
- [RFC 1035](https://www.rfc-editor.org/rfc/rfc1035)
- [Kaminsky talk](https://www.blackhat.com/presentations/bh-dc-08/Kaminsky/BlackHat-DC-08-Kaminsky-DNS-Reconnaissance.pdf)
