# 11-03 · Pool Overflow → Token Stealing

**Week:** W33–36 · **Track:** D · **Prev:** [`../02-driver-re-hemd`](../02-driver-re-hemd/README.md) · **Next:** [`../04-real-cves`](../04-real-cves/README.md)

## Objective
The Windows replacement for `commit_creds`: token stealing. Exploit HEVD pool overflow end-to-end.

## Tasks
- [ ] Kernel pool basics: pool types, lookaside, page pool; HEVD pool-overflow trigger
- [ ] Pool grooming: adjacent-object layout (overwrite `_OBJECT_HEADER`/function pointer)
- [ ] Token stealing shellcode (replace current `_EPROCESS.Token` with System token) — 32→64 bit
- [ ] SMEP/SMAP bypass on modern Windows (disable CR4 via ROP/stack pivot or use what's available on target)
- [ ] Full chain: overflow → hijack → shellcode → `SYSTEM` cmd

## Resources
- HEVD writeups (hackSysTeam), connar's / paulweston blogs
- *Windows Kernel Programming* (Reeves); P0 Windows kernel posts
- Transfer: pool grooming from Linux `msg_msg`/freelist work + kernel ROP

## Exit Criteria
- [ ] HEVD pool overflow → SYSTEM, from scratch — `labs/`
- [ ] Grooming layout diagram in `notes/`
