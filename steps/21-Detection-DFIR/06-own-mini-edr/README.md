# 21-06 · Own mini-EDR — detection primitives, not rules 🚩 M29

**Week:** W36+ stretch · **Track:** M · **Prev:** [`../05-capstone-ir-scenario`](../05-capstone-ir-scenario/README.md)

## Objective
Track M wrote rules (Sigma/YARA). Rules need telemetry — build the telemetry. Invert Track J's offensive primitives into a small working EDR and feed it your own Track E/J artifacts.

## Tasks
- [ ] Windows: ETW-based process/thread/registry telemetry (or kernel callbacks + minifilter, reusing your Track G KMDF driver); what each primitive sees and misses
- [ ] Linux: auditd rules and/or eBPF (Falco-style) process-exec + file telemetry; macOS: Endpoint Security Framework (dext) if your silicon allows, else auditd
- [ ] Memory scanning: your own YARA scanner against process memory (reuse Track E 03 evasion knowledge — what an EDR's scanner is up against)
- [ ] One console: ingest, alert, false-positive measurement on a clean baseline
- [ ] Feed it your own 12-06 implant / 18-x rootkit artifact in a VM; at least one artifact must alert on each OS; tune FP on the clean run
- [ ] Honest writeup: what you detect, what you'd miss, why real EDRs are big — `notes/`

## Resources
- ETW docs (Pavel Yosifovich); Microsoft ETW; Falco/Tetragon; ESF docs; YARA memory-scanning examples; your Track J/G artifacts

## Exit Criteria
- [ ] **M29:** own mini-EDR detects its own implant/rootkit artifact on ≥2 OSes, FP-engineered against a clean baseline — `labs/` + `notes/`
- [ ] Detection-primitive coverage table: telemetry source → sees → misses — `notes/`

## Links
- [Falco](https://falco.org/)
- [Microsoft ETW](https://learn.microsoft.com/en-us/windows/win32/etw/about-event-tracing)
- [Endpoint Security Framework](https://developer.apple.com/documentation/endpointsecurity)
