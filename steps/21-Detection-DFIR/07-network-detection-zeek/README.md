# 21-07 · Network detection — Zeek + Suricata on the wire (stretch)

**Week:** W36+ stretch · **Track:** M · **Prev:** [`../06-own-mini-edr`](../06-own-mini-edr/README.md) · **Next:** [`../08-honeypots`](../08-honeypots/README.md)

## Objective
Track M reads memory, disk, and logs — not the wire. Close it: NSM with Zeek and Suricata, and write detections that catch your own Track S C2 and 28-04 rogue AP on the network.

## Tasks
- [ ] NSM model: Zeek (connection metadata, protocol parsing, scripting) vs Suricata (signature engine, IPS-capable) — what each sees that the other misses; deploy both on a SPAN/tap of your lab (or tcpdump-replay)
- [ ] Zeek scripting: write ≥3 scripts — e.g., HTTP user-agent anomaly, DNS query entropy (beacon detection), TLS JA3/JA4 logging; correlate with Track S's beacon profile from 27-02
- [ ] Suricata rules: convert the same hypotheses to signatures; write one rule that fires on your own 28-04 rogue-AP engagement (or Track S beacon) — replay the pcap, tune
- [ ] Detection pipeline: log → normalization → alert (Elastic/OpenSearch or plain TShark + jq — engineering over ops); hook into your 21-06 mini-EDR console if it has an ingest path
- [ ] The wire-shaped hole: what host telemetry (21-06) misses that network catches, and vice versa — cross-detection table — `notes/`

## Resources
- Zeek docs + scripting guide; Suricata rule docs; Emerging Threats ruleset; your 27-02 beacon profile + 28-04 captures

## Exit Criteria
- [ ] 3 working Zeek scripts + 1 Suricata rule that fires on your own C2/rogue-AP capture, tuned against a clean capture — `labs/`
- [ ] Host-vs-network detection table: telemetry source → sees → misses — `notes/`

## Links
- [Zeek](https://zeek.org/)
- [Suricata](https://suricata.io/)
- [Emerging Threats](http://doc.emergingthreats.net/)
