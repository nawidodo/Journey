# 25-06 · Mimikatz internals — reading lsass, kerberos, and the LSA

**Week:** W20+ parallel · **Track:** Q · **Prev:** [`../05-windows-token-model`](../05-windows-token-model/README.md) · **Next:** [`../07-potato-family-internals`](../07-potato-family-internals/README.md)

## Objective
The flagship Windows credential tool, explained from source. Why it needs `SeDebugPrivilege`, what it reads out of lsass, and how the "pass-the-*" family reuses what it extracts.

## Tasks
- [ ] lsass.exe: what it is, why all logon credentials/tickets live in its memory, why it's SYSTEM-owned and how mimikatz gets in (`SeDebugPrivilege` → `OpenProcess` → `OpenProcessToken`/`DuplicateTokenEx` → `ReadProcessMemory`)
- [ ] `sekurlsa::logonpasswords`: the MSV/wdigest/kerberos/credman SSPI providers in lsass; why wdigest cleartext requires `UseLogonCredential=1`, where DPAPI master keys sit, how mimikatz walks lsass memory (known structure offsets per Windows version)
- [ ] Kerberos extraction: tickets in memory (LRU cache), how `sekurlsa::tickets` pulls them, pass-the-ticket = re-inject via `kerberos::ptt`; pass-the-hash = NTLM hash reuse without the password (`sekurlsa::pth` / `sekurlsa::msv`)
- [ ] Golden/silver tickets: what a KRBTGT hash signs, what the PAC contains, forge → use; why `dcsync` (DRSUAPI `GetNCChanges`) beats local extraction on DCs
- [ ] `lsadump::secrets` / `lsadump::sam`: LSA secrets + SAM registry hives, DPAPI `dpapi::` decrypt
- [ ] Read mimikatz source: `mimikatz/modules/kuhl_m_sekurlsa.c` + `kuhl_m_kerberos.c` — trace one function end to end in `notes/`
- [ ] Lab (isolated VM only): extract + use your own box's creds; pass-the-hash into a second local account; forge a golden ticket in a test domain (link Track D VM)

## Resources
- mimikatz source + blog.gentilkiwi.com
- SpecterOps/ired.team pass-the-* posts
- HackTricks Windows credentials sections

## Exit Criteria
- [ ] One mimikatz source function traced end to end with annotations — `notes/`
- [ ] Lab artifacts: PTH + golden ticket sessions on your own VMs — `labs/`
- [ ] Explain in ≤5 lines why mimikatz needs `SeDebugPrivilege` and what exactly it reads from lsass — `notes/`

## Links
- [mimikatz](https://github.com/gentilkiwi/mimikatz)
- [gentilkiwi blog](https://blog.gentilkiwi.com/)
- [HackTricks credentials](https://book.hacktricks.xyz/windows-hardening/active-directory-methodology)
- [The Hacker Recipes (AD attacks)](https://www.thehacker.recipes/)
