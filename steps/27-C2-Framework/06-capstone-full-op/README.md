# 27-06 · Capstone — full operation + detection round-trip 🚩 M27

**Week:** W36–38 · **Track:** S · **Prev:** [`../05-advanced-transports-redirectors`](../05-advanced-transports-redirectors/README.md)

## Objective
Run a complete engagement with your own framework — operator → teamserver → redirector → agent → task → exfil — then defend against it: write the detection rules that catch your own C2, fix, re-run. The "create C2 in my own" ask, end to end, red-teamed.

## Tasks
- [ ] Full op: own lab VMs only — agent on a "victim" VM (Windows + Linux), redirector in front of teamserver, operator drives tasks from the UI (shell, upload/download, sleep changes, kill)
- [ ] Add one missing piece you want (exfil staging, download cradles, PowerShell stager, or an extra transport) — scope it small, ship it
- [ ] Defend: from your step 02–05 notes, write detection — Sysmon (DNS query events, named pipe events, process creation from injected parents), Sigma rules for your beacon's periodicity/UA, YARA for the agent binary (static strings/entropy); run against a second agent instance and confirm detection — `code/`
- [ ] Fix the top 3 detections you caught (jitter, UA, entropy) — re-run, show improvement
- [ ] Writeup: architecture (from step 01, updated with what you actually built), ops runbook, detection results before/after, lessons vs real C2s (Sliver/Mythic comparison) — `notes/`
- [ ] Hand off: agent binary + detection rules to Track M as practice targets (feeds Track M)

## Resources
- Steps 01–05 outputs; Sigma repo; Sysmon config references; Sliver/Mythic docs for comparison

## Exit Criteria
- [ ] Full-op runbook executed on own VMs with artifacts — `labs/`
- [ ] Detection rules catch your own agent; ≥2 fixes measurably reduce detections — `code/`
- [ ] Writeup with before/after detection data — `notes/`
- [ ] Explain in ≤5 lines how you'd run the whole thing against a *defended* network and where it breaks — `notes/`

## Links
- [Sigma](https://github.com/SigmaHQ/sigma)
- [Sysmon modular rules](https://github.com/SwiftOnSecurity/sysmon-config)
- [Sliver](https://github.com/BishopFox/sliver)
