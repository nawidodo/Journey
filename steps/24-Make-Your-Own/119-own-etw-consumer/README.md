# 24-119 · Own ETW consumer — Windows Event Tracing, the internals telemetry (stretch)

**Week:** W46+ opt (stretch) · **Track:** P · **Prev:** [`../118-own-io-uring-lite`](../118-own-io-uring-lite/README.md) · **Next:** none (last) · **Pairs:** 21-01, 12-08, 24-86

## Objective
Your 12-08 evasion and 21-01 DFIR read about Windows telemetry; consume it — an ETW-lite consumer on your own Windows VM: enable a provider (kernel/process/network) via the session APIs (StartTrace/EnableTrace — the trace-session dance), read real-time buffers (the event header walk: GUID/opcode/version — 24-105's tracev3 sibling on Windows), decode a payload (image load + process start), and the lab: launch your own 12-03-style test binary and catch it appearing (the 24-86 rootkit-visibility question, Windows edition). Then the X-ray: what an implant's defense does against ETW (patch/disable — 12-08 notes) and why kernel-only telemetry survives it (the 21-06 detection lesson). Runs on your own Windows eval VM, lab-only.

## Tasks
- [ ] Session: StartTrace/EnableTrace with a real provider GUID, buffer size/flush policy (the config knobs — 24-30 discipline)
- [ ] Reading: ProcessTrace callback, event descriptor decode (header → properties), map to readable fields
- [ ] Lab: own test binary → observed process-create/image-load events in your consumer — the artifact + timestamp proof — `labs/` + `code/`
- [ ] Evasion-cross: from 12-08, list the classic ETW tampering classes; test which survive kernel-mode collection (12-08 notes say)
- [ ] Writeup: ETW as the Windows alternative to /proc/`log` (your 24-103/24-86 cross-platform telemetry chart) — `notes/`

## Resources
- ETW docs + manifest/event decoding notes, WinDbg/`logman` as oracle; Microsoft's ETW samples; your 24-105/24-86/12-08 notes

## Exit Criteria
- [ ] Consumer captures own-binary process/image events, verified vs logman — `labs/` + `code/`
- [ ] ETW-vs-/proc-vs-logarchive writeup — `notes/`

## Links
- [ETW basics (MS)](https://learn.microsoft.com/en-us/windows-hardware/drivers/devtest/event-tracing-for-windows--)
- [logman (oracle tool)](https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/logman)