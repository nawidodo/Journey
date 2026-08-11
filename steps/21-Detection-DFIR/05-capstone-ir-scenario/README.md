# 21-05 · Capstone: Full IR Scenario 🚩 M22

**Week:** W35–36 · **Track:** M · **Prev:** [`../04-disk-artifact-forensics`](../04-disk-artifact-forensics/README.md)

## Objective
One incident, three artifacts (memory + disk + logs), one root-cause chain and a shipping detection. The phase's proof of the full defensive loop.

## Tasks
- [ ] Build (or download) an incident lab: a compromised VM/image with a real-ish kill chain (initial access → privilege escalation → persistence → exfil)
- [ ] Reuse your own work as the attacker: detonate your Track E implant or Track J rootkit, snapshot memory + disk + collect logs
- [ ] Investigate: memory first (Volatility), then disk timeline, then log correlation — build the kill-chain timeline
- [ ] Name root cause + technique; scope the blast radius (what else could be affected)
- [ ] Write one durable detection for the incident (Sigma/YARA) and tune it
- [ ] Analyst report: summary for decision-makers + technical appendix — `notes/`

## Resources
- Your 21-01–04 outputs; your Track E/J artifacts
- Public IR walkthroughs (SANS DFIR posters, CISA advisories) for calibration

## Exit Criteria
- [ ] **M22: memory + disk + logs → root cause → detection rule** — `labs/`
- [ ] Analyst writeup — `notes/`

## Links
- [Your 21-01–04 outputs] — composition step
- [SANS DFIR posters](https://www.sans.org/posters/dfir/)
