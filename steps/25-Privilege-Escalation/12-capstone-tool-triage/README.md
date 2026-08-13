# 25-12 · Capstone — own triage tool + tool-internals wiki + re-derive

**Week:** W20+ parallel · **Track:** Q · **Prev:** [`../11-android-ios-privesc`](../11-android-ios-privesc/README.md) · **Next:** — (feeds Track D `11-01`, Track E, Track M)

## Objective
The track's exit: stop using privesc tools as black boxes. Ship your own minimal cross-OS triage scanner, write the "how it works" wiki entry for every tool studied, and re-derive the two flagships from memory.

## Tasks
- [ ] **Triage tool**: extend `02`'s mini_linpeas into a small cross-OS scanner in `code/` — Linux shell script + Windows PowerShell (service DACLs, token privileges via .NET `GetTokenInformation`, AlwaysInstallElevated check) + macOS (TCC db perms, SIP status, launchd writable plists). Each check outputs the *boundary* it probes, not just a finding
- [ ] **Tool-internals wiki** (`notes/tool-internals.md`): one entry per tool studied this track — linpeas, pspy, mimikatz, Rogue Potato, PrintSpoofer, GodPotato, WinPEAS, PowerUp, BloodHound/SharpHound, impacket (one component), Magisk, checkm8/ipwndfu. Format per entry: mechanism → boundary crossed → why it works on modern X → what kills it (patch/detection)
- [ ] **Re-derive** (writeup closed): mimikatz `sekurlsa::logonpasswords` flow and Rogue Potato flow, both from memory in `notes/`
- [ ] **Detection round-trip**: for 3 of your wiki entries, write the Sigma/YARA or Sysmon rule that detects the primitive (feeds Track M)
- [ ] Cross-check at least one tool you built against the real one on a lab VM (your mini_linpeas vs linpeas — same findings, or explain the diff)

## Resources
- All prior steps in this phase; PEASS-ng + mimikatz + potato sources as cross-check only
- Track M steps for the detection side

## Exit Criteria
- [ ] Cross-OS triage tool runs on Linux + Windows + macOS targets, findings mapped to boundaries — `code/`
- [ ] Tool-internals wiki complete for ≥10 tools — `notes/`
- [ ] Mimikatz + Rogue Potato re-derived from memory (writeup closed) — `notes/`
- [ ] 3 detection rules written against your own wiki entries — `code/`

## Links
- [PEASS-ng](https://github.com/peass-ng/PEASS-ng)
- [Sigma repo](https://github.com/SigmaHQ/sigma)
- [Sysmon modular rules](https://github.com/SwiftOnSecurity/sysmon-config)
- [HackTricks privesc master](https://book.hacktricks.xyz/)
