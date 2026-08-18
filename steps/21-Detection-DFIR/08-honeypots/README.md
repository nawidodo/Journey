# 21-08 · Honeypots & deception — capture the attacker (stretch)

**Week:** W36+ stretch · **Track:** M · **Prev:** [`../07-network-detection-zeek`](../07-network-detection-zeek/README.md)

## Objective
The third detection pillar: rules (03) and telemetry (06) react — deception invites. Run your own low-interaction honeypots, mine the intel, and convert it into the other two pillars.

## Tasks
- [ ] Honeypot taxonomy: low (Kippo-class SSH) vs medium (web emulation) vs high interaction (real services, firewalled); what each gains and risks; why tarpits are the cheap cousin
- [ ] Deploy: Kippo/Cowrie SSH honeypot + a web honeypot (Glastopf-class or your own fake login page) on your lab network; observe real-world vs lab noise (internet-facing only if you own the box and can contain it)
- [ ] Data: what an attacker's session reveals — username/password pairs, command history, tool downloads; mine it into IOCs
- [ ] Convert: honeypot intel → Sigma/YARA rules (03) and Zeek/Suricata signatures (07) — the intel loop closed
- [ ] Deception engineering: what makes a honeypot detectable (fingerprints, response timing) and how defenders accept the risk; when deception is a liability
- [ ] Writeup: deployment, one attacker session analyzed, rules produced — `notes/`

## Resources
- awesome-honeypots list; Cowrie docs; Glastopf; your 21-03/07 rule discipline

## Exit Criteria
- [ ] SSH + web honeypots running, one captured session analyzed end-to-end — `labs/`
- [ ] ≥1 Sigma rule + ≥1 Zeek/Suricata signature derived from that session — `labs/`

## Links
- [awesome-honeypots](https://github.com/paralax/awesome-honeypots)
- [Cowrie](https://github.com/cowrie/cowrie)
