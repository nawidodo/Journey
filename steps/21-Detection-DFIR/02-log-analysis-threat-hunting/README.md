# 21-02 · Log Analysis + Threat Hunting

**Week:** W31–32 · **Track:** M · **Prev:** [`../01-memory-forensics-volatility`](../01-memory-forensics-volatility/README.md) · **Next:** [`../03-detect-as-code-sigma-yara`](../03-detect-as-code-sigma-yara/README.md)

## Objective
The hunting loop: hypothesis → data source → query → validate. Turns the "detection mirrors" in your malware steps into a method.

## Tasks
- [ ] Telemetry sources: Linux (auditd, journald, eBPF), macOS (unified log, ESF), Windows (Event Log, Sysmon), network (Zeek/pcap)
- [ ] Build a small logging lab: your Dojo VM + your Track E implant generating events
- [ ] Hunt for one technique end-to-end: write the hypothesis, pick the source, query it, prove the finding (e.g., "find my beacon's C2 cadence in connection logs")
- [ ] Baselines vs anomalies: what "normal" looks like before you can find "weird"
- [ ] Log pitfalls: truncation, timezones, source-spoofing, missing context — how adversaries abuse gaps

## Resources
- Sigma repo + Sysmon docs; Zeek docs; macOS unified log guide
- ATT&CK as the hunt vocabulary (map findings to techniques)
- Your Track E/F/J outputs as hunt targets

## Exit Criteria
- [ ] One hypothesis → query → confirmed finding on your own lab — `labs/`
- [ ] "Hunt loop" notes (source→query→validate) — `notes/`

## Links
- [Sigma HQ](https://github.com/SigmaHQ/sigma)
- [MITRE ATT&CK](https://attack.mitre.org/)
- [Zeek](https://github.com/zeek/zeek)
