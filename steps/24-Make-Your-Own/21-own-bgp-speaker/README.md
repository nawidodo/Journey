# 24-21 · Own BGP speaker — the internet's routing protocol, hijack lab (stretch)

**Week:** W46+ opt (stretch) · **Track:** P · **Prev:** [`../20-own-packet-sniffer`](../20-own-packet-sniffer/README.md) · **Next:** [`../22-own-onion-router`](../22-own-onion-router/README.md)

## Objective
BGP carries the internet's routes — and its worst incidents (prefix hijacks). Build a speaker: session state machine (OPEN/KEEPALIVE/UPDATE), path attributes, route selection, and peer it with FRRouting/BIRD on your own VMs (your ASes, your lab). Then the cool part: hijack a prefix in your lab (announce + withdraw), see propagation, and defend with RPKI/ROA concepts.

## Tasks
- [ ] Session: TCP 179, OPEN (AS number, hold time), KEEPALIVE, UPDATE encode/decode; peer with FRR/BIRD in a VM lab (own ASes only)
- [ ] Path attributes: AS_PATH, NEXT_HOP, ORIGIN; route selection (shortest AS path, local-pref) — your RIB
- [ ] Hijack lab: announce a prefix you don't own *in your lab topology* → trace propagation to your peer; then ROA/RPKI-style validation (or AS-path filtering) blocks it — `labs/`
- [ ] Writeup: real hijack cases (YouTube/Pakistan 2008, RouteLeaks), how RPKI + BGPsec change the game — `notes/`

## Resources
- RFC 4271 (the manual); FRRouting/BIRD docs; your 24-20 + 21-07 notes

## Exit Criteria
- [ ] BGP session up with FRR/BIRD, routes exchanged, selection works — `labs/`
- [ ] Lab hijack propagated then blocked by validation — `labs/` + `notes/`

## Links
- [RFC 4271](https://www.rfc-editor.org/rfc/rfc4271)
- [FRRouting](https://frrouting.org/)
