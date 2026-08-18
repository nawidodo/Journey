# 24-56 · Own network scanner — Nmap-lite: SYN scans, service detection (stretch)

**Week:** W46+ opt (stretch) · **Track:** P · **Prev:** [`../55-own-3d-model-loader`](../55-own-3d-model-loader/README.md) · **Next:** [`../57-own-ids-engine`](../57-own-ids-engine/README.md) · **Pairs:** 24-20, 03-07

## Objective
Recon is the first weaponized skill in the curriculum (03-07) — build the tool that does it. An Nmap-lite: raw-socket SYN scan (own 24-20 packet crafting), connect scan, port-state classification, banner grab + service matching, OS fingerprint-lite (TTL/window heuristics — pairs 04-09). Safety: own VMs + own lab only; the point is understanding every packet your scanner emits and why defenders see it.

## Tasks
- [ ] SYN scan: craft SYN (24-20 raw sockets), classify RST/ACK/no-reply → open/closed/filtered; the state machine + timeouts
- [ ] Connect scan fallback + banner grab (recv after connect), service match (port→service table + banner strings)
- [ ] Fingerprint-lite: TTL, window size, TCP options → candidate OS (pairs 04-09 FreeBSD vs Linux observations)
- [ ] Lab: scan your own VM (03-07 setup): verify every classification with tcpdump (24-20); run your 24-34 mail/24-17 HTTP servers as targets — `labs/`
- [ ] Writeup: why SYN scan is detectable (the IDS/IPS perspective — pairs 24-57) — `notes/`

## Resources
- TCP/IP Illustrated (the manual — the FIN/SYN state machine); nmap source (peer); your 24-20/03-07 notes

## Exit Criteria
- [ ] Scanner classifies open/closed/filtered correctly vs tcpdump ground truth — `labs/`
- [ ] Detectability + fingerprint writeup — `labs/` + `notes/`

## Links
- [nmap](https://nmap.org/)
- [TCP/IP Illustrated](https://en.wikipedia.org/wiki/TCP/IP_Illustrated)
