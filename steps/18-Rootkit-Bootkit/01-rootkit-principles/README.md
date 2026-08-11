# 18-01 · Rootkit Principles

**Week:** W44–45 · **Track:** J · **Prev:** — · **Next:** [`../02-linux-lkm-ebpf-rootkit`](../02-linux-lkm-ebpf-rootkit/README.md)

## Objective
What rootkits actually do and where they live — before writing one. The taxonomy that the per-OS steps slot into.

## Tasks
- [ ] Hide primitives: process/file/network/registry/driver hiding per OS
- [ ] Where hooks live: syscall table, IDT, kernel callbacks, BPF, page tables, firmware
- [ ] Detection primitives: kernel integrity, ESF/auditd/Sysmon/Play Integrity
- [ ] Trust boundaries: kernel vs TEE/Secure Enclave vs firmware — what survives reboot/reflash
- [ ] Map all 5 platforms: Linux, Windows, macOS, iOS, Android

## Resources
- *Rootkits: Subverting the Windows Kernel* (Hoglund/Butler); *The Rootkit Arsenal* (Blunden)
- BlackLotus analyses (ESET, SentinelOne); UEFI spec

## Exit Criteria
- [ ] Per-OS hook/hide/trust map — `notes/`

## Links
- [The Rootkit Arsenal (book site)](https://www.securityfocus.com/) — see plan Resources for the book
- [MITRE ATT&CK (rootkit techniques)](https://attack.mitre.org/techniques/T1014/)
