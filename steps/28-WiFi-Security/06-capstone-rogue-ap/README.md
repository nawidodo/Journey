# 28-06 · Capstone: Full Wi-Fi engagement → detection round-trip 🚩 M28

**Week:** W38–40 · **Track:** T · **Prev:** [`../05-detection-mirror-wids`](../05-detection-mirror-wids/README.md)

## Objective
One complete op on your own lab, from recon to credential to pivot — then prove your own detections catch it, fix, and re-run. The deliverable that proves the track.

## Tasks
- [ ] Recon: scan your lab airspace (own APs/test SSIDs only), map clients, pick the target SSID
- [ ] Attack: choose the weakest door — PSK crack, EAP relay, or captive portal — and execute the full 28-01–04 chain to a credential + pivot
- [ ] Detection round-trip: run the 28-05 rules against the engagement capture; at least one rule must fire; tune the attack until it's caught, then tune the rules (adversary-in-the-loop, Track M style)
- [ ] Full writeup: target config, chain, before/after detection data, what you'd change on the real job
- [ ] Cross-link: what Track M learned about disk/memory applies to the air — and what does not

## Resources
- Your 28-01–05 outputs; Track M 03/05 discipline

## Exit Criteria
- [ ] **M28:** rogue AP + client compromise + pivot on own lab, caught by own detection rules (before/after data) — `labs/` + `notes/`

## Links
- [Track M detection discipline](../21-Detection-DFIR/03-detect-as-code-sigma-yara/README.md)
