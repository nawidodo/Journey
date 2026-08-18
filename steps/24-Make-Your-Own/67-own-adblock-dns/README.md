# 24-67 · Own ad-blocking DNS — Pi-hole-lite: sinkhole + metrics (stretch)

**Week:** W46+ opt (stretch) · **Track:** P · **Prev:** [`../66-own-load-balancer`](../66-own-load-balancer/README.md) · **Pairs:** 24-19, 24-42, 24-57

## Objective
Your 24-19 resolver resolves; now make it judge. Build a Pi-hole-lite: DNS sinkhole (blocklist → NXDOMAIN/null answer), the blocklist update pipeline (fetch/parse/validate — pairs 24-39 package-manager discipline), per-client tracking + metrics (24-42 storage → your 24-51 TSDB), and the security story: DNS as the attacker/defender choke point (malware C2 domains — pairs 27-C2, 21-07; why DNS filtering works and how malware dodges it: DGA, DoH — the evasion writeup).

## Tasks
- [ ] Sinkhole: blocklist (domains/patterns, wildcards), intercept own 24-19 resolver responses → 0.0.0.0/NXDOMAIN response
- [ ] Pipeline: fetch + validate blocklists (checksum/parse-check), status + last-update; the supply-chain care (24-39 lessons)
- [ ] Metrics: per-client query log → your 24-42/24-51 storage, top-blocked, query types; repeat-step graphs (24-41)
- [ ] Evasion lab: DGA-lite hostnames (your own 24-29-style randomness), DNS-over-HTTPS (your 24-19 resolver via 24-17) — see the filter miss, then decide the counter (encrypted-DNS visibility) — `labs/`
- [ ] Self-check: your ad domain gets sinkholed; client-query dashboard works

## Resources
- Pi-hole docs + blocklist format (the manual); your 24-19/24-42/24-51 code

## Exit Criteria
- [ ] Sinkhole blocks, blocklists update safely, metrics collected — `labs/`
- [ ] DGA/DoH evasion + counter writeup — `labs/` + `notes/`

## Links
- [Pi-hole](https://docs.pi-hole.net/)
- [DNS blocklist formats](https://firebog.net/)