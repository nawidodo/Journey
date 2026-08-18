# 28-05 · Detection mirror — WIDS, rogue-AP hunting, own rules

**Week:** W36–38 · **Track:** T · **Prev:** [`../04-redteam-wifi-hosting`](../04-redteam-wifi-hosting/README.md) · **Next:** [`../06-capstone-rogue-ap`](../06-capstone-rogue-ap/README.md)

## Objective
The blue side of the air. Detect the 28-01–04 playbook the way Track M detects disk/memory artifacts.

## Tasks
- [ ] WIDS/WIPS: rogue-AP detection classes — signal/channel fingerprinting, clock-skew (the AP's clock drifts), beacon/probe anomalies, deauth floods, 802.11 association forensics
- [ ] Client-side tells: unexpected SSIDs, transition-mode downgrade detection, cert-validation failures (what a client *should* log)
- [ ] Detection-as-code on the air: TShark/Suricata 802.11 signatures; write own Sigma rules for the 28-04 engagement (rogue SSID, deauth rate, EAP relay timing)
- [ ] Hunt loop: hypothesis → capture → rule → validate, using your own 28-04 capture as the target (Track M `03-detect-as-code-sigma-yara` discipline, radio substrate)
- [ ] Document WPA2 vs WPA3 detection asymmetry (what WPA3 transition-mode does to your visibility) — `notes/`

## Resources
- WIDS papers (clock-skew research); Suricata/TShark docs; Sigma repo; your 28-04 runbook/logs

## Exit Criteria
- [ ] Detection rules that catch your own 28-04 rogue-AP engagement, validated against a clean capture — `labs/`
- [ ] WIDS survey table: attack → detection signal → cost — `notes/`

## Links
- [Suricata](https://suricata.io/)
- [Sigma](https://github.com/SigmaHQ/sigma)
