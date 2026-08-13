# 25-07 · Potato family internals — Rogue Potato, PrintSpoofer, GodPotato

**Week:** W20+ parallel · **Track:** Q · **Prev:** [`../06-mimikatz-internals`](../06-mimikatz-internals/README.md) · **Next:** [`../08-windows-service-misconfig-tools`](../08-windows-service-misconfig-tools/README.md)

## Objective
The whole potato lineage, from the 2016 Rotten Potato to GodPotato — one underlying primitive (service account with `SeImpersonatePrivilege` + a way to get a SYSTEM token) and an arms race of trigger mechanisms.

## Tasks
- [ ] The primitive: a service account holds `SeImpersonatePrivilege` → if you can make a privileged process *authenticate to you* (NTLM to localhost), you get its token and `DuplicateTokenEx` → SYSTEM. That's every potato.
- [ ] Rotten Potato (2016): how it abused DCOM + BITS' privilege to trigger NTLM authentication to a local listener
- [ ] Juicy Potato (2017): COM object instantiation with chosen CLSID → token impersonation; why it died (patch: `SeImpersonate` restrictions on session 0 / anonymous)
- [ ] Rogue Potato (foxglove 2020): read the writeup — MS-RPC SSRF through the RPCSS pipe (OXID resolver), tricking the system into sending a privileged NTLM auth to your local named pipe; why it survived the Juicy patch
- [ ] PrintSpoofer (itm4n 2020): the named-pipe trick via the Print Spooler service — spawn a pipe as SYSTEM, have spoolsv connect, impersonate
- [ ] SweetPotato / GodPotato: DCOM + RPC variants; what each fixed or bypassed, current status (which work on modern Windows)
- [ ] Lab (VM): exploit a service account (`SeImpersonatePrivilege`) to SYSTEM with Rogue Potato and PrintSpoofer; diff the two mechanisms in `notes/`
- [ ] Read the Rogue Potato and PrintSpoofer writeups' diagrams; re-draw both from memory

## Resources
- foxglovesecurity Rogue Potato blog (the canonical writeup)
- itm4n PrintSpoofer blog
- potato github repos + READMEs

## Exit Criteria
- [ ] Both attacks run to SYSTEM on a lab VM — `labs/`
- [ ] Re-drawn diagrams of Rogue Potato's RPCSS pipe flow and PrintSpoofer's pipe trick — `notes/`
- [ ] Explain in ≤5 lines why a plain service account can impersonate SYSTEM at all — `notes/`

## Links
- [Rogue Potato writeup (foxglove)](https://foxglovesecurity.com/2020/04/18/rogue-potato-the-road-to-privileged-credentials/)
- [RottenPotato](https://github.com/foxglovesec/RottenPotato)
- [JuicyPotato](https://github.com/ohpe/juicy-potato)
- [PrintSpoofer (itm4n)](https://github.com/itm4n/PrintSpoofer)
- [PrintSpoofer blog](https://itm4n.github.io/printspoofer-abusing-print-spooler/)
- [GodPotato](https://github.com/BeichenDream/GodPotato)
