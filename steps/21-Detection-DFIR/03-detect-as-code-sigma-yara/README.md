# 21-03 · Detection-as-Code: Sigma + YARA Mastery

**Week:** W32–34 · **Track:** M · **Prev:** [`../02-log-analysis-threat-hunting`](../02-log-analysis-threat-hunting/README.md) · **Next:** [`../04-disk-artifact-forensics`](../04-disk-artifact-forensics/README.md)

## Objective
Upgrade from "wrote a few rules against my own implant" to building a real detection library — the skill blue teams hire for.

## Tasks
- [ ] Sigma: logsource modeling, selection/filter/condition, field mappings per backend (Elastic/Splunk/Windows Event Log)
- [ ] Write 5 production-quality Sigma rules from scratch; validate with a Sigma linter; convert to one backend query
- [ ] YARA: rule anatomy, string modifiers, byte patterns, performance (avoid catastrophic backtracking)
- [ ] Write YARA rules from a *binary* (your 12-07 loader) — tie to the write-yara-from-reversed-code workflow
- [ ] False-positive engineering: baseline, allowlist, threshold tuning; measure precision on your lab
- [ ] Adversary-in-the-loop: re-engineer one detection's target to evade it, then fix the detection (Track E style, mirrored)

## Resources
- Sigma spec + repo (sigma/sigma, sigma-converter); sigmac docs
- YARA docs; yara-x; your 12-05/17-05 outputs
- Detections from public SIGMA/github (Blueteam0ps, Sektor7 blog)

## Exit Criteria
- [ ] 5 Sigma + 3 YARA rules, linted and converted — `labs/`
- [ ] Detection-tuning notes (precision vs recall) — `notes/`

## Links
- [Sigma spec](https://github.com/SigmaHQ/sigma-specification)
- [YARA (VirusTotal)](https://github.com/VirusTotal/yara)
