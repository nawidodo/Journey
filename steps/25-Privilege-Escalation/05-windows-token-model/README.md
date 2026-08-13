# 25-05 · Windows token model — the substrate mimikatz and potatoes stand on

**Week:** W20+ parallel · **Track:** Q · **Prev:** [`../04-linux-service-cron-path-kernel`](../04-linux-service-cron-path-kernel/README.md) · **Next:** [`../06-mimikatz-internals`](../06-mimikatz-internals/README.md)

## Objective
Tokens are the single most-abusable object on Windows. Understand them cold — every token tool (mimikatz `token::`, potatoes, PrintSpoofer, incognito) is a manipulation of this model.

## Tasks
- [ ] Token anatomy: TokenUser/TokenGroups/TokenPrivileges/TokenOwner/TokenIntegrityLevel/TokenImpersonationLevel; primary vs impersonation tokens; `CreateProcessWithTokenW` vs `ImpersonateLoggedOnUser`
- [ ] Privileges: what each escalation-relevant one *means* — `SeDebugPrivilege` (OpenProcess on anything incl. lsass), `SeImpersonatePrivilege` (impersonate any token you hold a handle to), `SeAssignPrimaryTokenPrivilege`, `SeBackup/SeRestore` (read/write any file), `SeLoadDriver`, `SeTakeOwnership`
- [ ] Impersonation: why a service account holding `SeImpersonatePrivilege` can become SYSTEM via a token leak — the potato seed; how `DuplicateTokenEx` + `CreateProcessWithTokenW` chain
- [ ] Integrity levels: Low→Medium→High→System, UIPI, why UAC just adds a filter token, AppContainer
- [ ] Read your own token: `code/` a ~100-line C or PowerShell program calling `OpenProcessToken`/`GetTokenInformation` that dumps every privilege and group SID of the current process; run as user and as admin, diff the outputs
- [ ] Lab: `whoami /priv` on a service-owned process vs interactive; observe which privileges differ

## Resources
- MS Learn: tokens, privileges, integrity levels
- HackTricks Windows privesc (token section)
- Sektor7 / ired.team token writeups

## Exit Criteria
- [ ] Token dumper runs and its diff user-vs-admin is explained — `code/`
- [ ] Explain in ≤5 lines what `SeImpersonatePrivilege` actually authorizes and why service accounts hold it — `notes/`
- [ ] Diagram the token flow of one full impersonation→CreateProcessWithTokenW chain — `notes/`

## Links
- [Access tokens (MS)](https://learn.microsoft.com/en-us/windows/win32/secauthz/access-tokens)
- [Token privileges (MS)](https://learn.microsoft.com/en-us/windows/win32/secauthz/privilege-constants)
- [ired.team token manipulation](https://www.ired.team/offensive-security/privilege-escalation/windows-token-manipulation)
- [HackTricks Windows privesc](https://book.hacktricks.xyz/windows-hardening/windows-local-privilege-escalation)
