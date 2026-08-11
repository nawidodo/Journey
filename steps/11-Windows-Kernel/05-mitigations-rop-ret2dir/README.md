# 11-05 · Modern Mitigations: ROP, ret2dir, Data-only

**Week:** W40–43 · **Track:** D · **Prev:** [`../04-real-cves`](../04-real-cves/README.md) · **Next:** [`../06-capstone-hevd-cve`](../06-capstone-hevd-cve/README.md)

## Objective
Windows 10/11 hardening: KVA shadow, CET (shadow stack + IBT), HVCI (VBS), WDAC. How modern exploits still work.

## Tasks
- [ ] Kernel ROP (KROP) on Win10+; stack pivot; `!process` mitigation states
- [ ] ret2dir / data-only attacks: corrupt `_TOKEN`, `_SEP_TOKEN_PRIVILEGES`, or a callback list without code exec
- [ ] HVCI/VBS: why classic token-stealing shellcode dies; what survives (data-only, WNF/state corruption)
- [ ] CET: shadow-stack impact on ROP; when it's off and why (compat, drivers)

## Resources
- P0 "Windows kernel exploits" series; Black Hat/HITB Windows exploit talks
- saelo.io Windows kernel exploitation posts; 10Pwn/Peter Hlavaty blogs

## Exit Criteria
- [ ] One data-only exploit variant for an HEVD bug — `labs/`
- [ ] Mitigation matrix (like your KTRR/PPL/PAC matrix, but Windows) — `notes/`

## Links
- [Windows mitigations docs](https://learn.microsoft.com/en-us/windows/security/application-security/application-isolation/windows-sandbox)
- [ret2dir paper (USENIX)](https://www.usenix.org/conference/usenixsecurity14/technical-sessions/presentation/evtyushkin)
