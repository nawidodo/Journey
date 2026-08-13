# 25-08 · Windows service misconfigs + UAC + kernel CVEs — the non-token surface

**Week:** W20+ parallel · **Track:** Q · **Prev:** [`../07-potato-family-internals`](../07-potato-family-internals/README.md) · **Next:** [`../09-ad-domain-privesc`](../09-ad-domain-privesc/README.md)

## Objective
WinPEAS/PowerUp/Watson output decoded — every check they run, the underlying Windows mechanism, and which CVE classes still matter.

## Tasks
- [ ] WinPEAS internals: which SCM calls (`EnumServicesStatusExW`), registry reads (`HKLM\SYSTEM\CurrentControlSet\Services\*`), and file scans produce each finding; how it walks the service database without admin
- [ ] Service misconfigs: unquoted service path (how `CreateProcess` tokenizes the path), weak service DACL (`ChangeServiceConfig` → repoint binary), `ImagePath` writable, AlwaysInstallElevated (why it's a registry *policy*), scheduled tasks, startup folders, DLL search-order hijacking
- [ ] PowerUp (mechanism, not usage): `Get-ServiceUnquoted`, `Get-ModifiableServiceFile`, how they resolve DACLs via `Get-Acl` and service config via SCM
- [ ] Watson/Sherlock/winpeas CVE database: how they map OS build → vulnerable — and why they're mostly dead (patch cadence); what still works: PrintNightmare (CVE-2021-1675/34527) mechanics — spoolsv + driver install + `LoadLibrary` as SYSTEM
- [ ] UAC bypass classes: auto-elevating COM objects, fodhelper/computerdefaults registry hijack, why filter-token bypass is a *same-user* elevation, not a boundary break
- [ ] Lab: three service-misconfig routes to SYSTEM on a test VM (unquoted path, weak DACL, AlwaysInstallElevated) — `labs/`
- [ ] Detection mirror in `notes/`: which of these SIGMA/Sysmon rules catch (feeds Track M)

## Resources
- winpeas source (C#), PowerUp source (PowerShell)
- HackTricks Windows privesc misconfig sections
- Track D VM for the Windows side (same VM)

## Exit Criteria
- [ ] Three service-misconfig routes to SYSTEM, artifacts + writeup — `labs/`
- [ ] Explain in ≤5 lines how an unquoted service path becomes code execution — `notes/`
- [ ] Explain in ≤5 lines why UAC bypass ≠ privilege escalation in the token sense — `notes/`

## Links
- [HackTricks Windows privesc](https://book.hacktricks.xyz/windows-hardening/windows-local-privilege-escalation)
- [PowerUp](https://github.com/PowerShellMafia/PowerSploit)
- [Watson](https://github.com/rasta-mouse/Watson)
- [PrintNightmare (cube0x0)](https://github.com/cube0x0/CVE-2021-1675)
- [UAC bypass (ired.team)](https://www.ired.team/offensive-security/privilege-escalation/t1134-access-token-manipulation)
