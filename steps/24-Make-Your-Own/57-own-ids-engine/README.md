# 24-57 · Own IDS rule engine — Snort-lite: signatures over your captures (stretch)

**Week:** W46+ opt (stretch) · **Track:** P · **Prev:** [`../56-own-network-scanner`](../56-own-network-scanner/README.md) · **Pairs:** 24-20, 24-56, 21-06, 21-07

## Objective
You've built the EDR (21-06); now the network half. A Snort-lite: rule language (header + content match), matching engine (fast pattern prefilter — pairs 24-41 index thinking, 24-25), TCP stream reassembly, alerts → feed your 21-06 alert pipeline. Then the evasion lab: fragment/obfuscate a payload against your own IDS and watch it miss — the defender's constant loop (pairs 21-07 Zeek).

## Tasks
- [ ] Rule parser: `alert tcp $HOME_NET any -> $EXT any (msg; content:; ...)` — header + options
- [ ] Engine: capture from 24-20 (pcap + live), content match with prefilter (Aho-Corasick/Boyer-Moore-lite — pairs 24-25), flow tracking (TCP reassembly — pairs 24-20)
- [ ] Alerts: rule hits → event stream → your 21-06 EDR alert format (the unified pipeline)
- [ ] Evasion lab: scan + exploit-ish traffic from your own 24-56 scanner vs your rules — fragmentation, case/encoding tricks, split payloads; adjust rules (the whack-a-mole writeup) — `labs/`
- [ ] Self-check: rule set detects your own scanner's SYN sweep + a reverse shell string; evasions listed

## Resources
- Snort rule guide (the manual); Suricata docs (peer); your 24-20/24-56/21-06/21-07 notes

## Exit Criteria
- [ ] Rule engine detects own traffic; evasions documented — `labs/`
- [ ] Reassembly + evasion writeup — `labs/` + `notes/`

## Links
- [Snort rules](https://docs.snort.org/rules/)
- [Suricata](https://suricata.io/)
