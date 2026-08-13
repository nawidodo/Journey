# 25-09 · AD domain privesc — BloodHound, Kerberoasting, delegation, impacket

**Week:** W20+ parallel · **Track:** Q · **Prev:** [`../08-windows-service-misconfig-tools`](../08-windows-service-misconfig-tools/README.md) · **Next:** [`../10-macos-privesc`](../10-macos-privesc/README.md)

## Objective
In any real environment the fastest path to DA isn't a kernel bug — it's the directory. The tools, their LDAP/ADSI mechanisms, and how "privesc" becomes "domain takeover".

## Tasks
- [ ] AD object model: users/groups/ACEs, why group membership = the privesc graph; `SeEnableDelegationPrivilege`, `GenericAll`/`GenericWrite` on objects
- [ ] BloodHound internals: SharpHound collects via LDAP (`DirectorySearcher`), session enumeration (NetSessionEnum/SCM `NetWkstaUserEnum`), ACL parsing from the schema; how the Neo4j graph computes shortest paths to DA
- [ ] Kerberoasting: request a service ticket for an SPN account → extract from the TGS → offline crack; why no admin needed, what the countermeasure is (the "revisited" blog)
- [ ] AS-REP roasting: `DONT_REQ_PREAUTH` accounts → TGT offline crack
- [ ] Delegation: unconstrained (TGT in memory → ticket theft), constrained (S4U2Self/S4U2Proxy → RBCD), resource-based constrained delegation — the modern dominant primitive
- [ ] ACL attacks: GenericAll → password reset / RBCD write; `WriteDacl` → grant yourself DCSync; the attacker-controlled SPN trick
- [ ] impacket internals: `GetNPUsers`/`GetUserSPNs` (ASN.1 Kerberos encoding by hand in `impacket/krb5/`), `secretsdump` (DRSUAPI), `wmiexec`/`smbexec`/`psexec` (service creation over SMB)
- [ ] Lab: build a 3-host test domain (VM), walk user→DA via BloodHound's recommended path, execute it with impacket — `labs/`

## Resources
- BloodHound docs, thehacker.recipes, SpecterOps posts
- impacket source
- Track D VM / AD lab

## Exit Criteria
- [ ] user → DA chain executed end-to-end on own domain, artifacts — `labs/`
- [ ] Kerberoast + AS-REP both run; explain in ≤5 lines what each extracts and why no creds are needed — `notes/`
- [ ] Re-draw the RBCD flow (S4U2Self → S4U2Proxy → ticket to service) from memory — `notes/`

## Links
- [BloodHound](https://github.com/SpecterOps/BloodHound)
- [Kerberoasting revisited](https://posts.specterops.io/kerberoasting-revisited-d434351bd4d1)
- [The Hacker Recipes](https://www.thehacker.recipes/)
- [impacket](https://github.com/fortra/impacket)
- [HackTricks AD methodology](https://book.hacktricks.xyz/windows-hardening/active-directory-methodology)
