# 18-02 · Linux Rootkit: LKM + eBPF

**Week:** W45–47 · **Track:** J · **Prev:** [`../01-rootkit-principles`](../01-rootkit-principles/README.md) · **Next:** [`../03-windows-driver-rootkit`](../03-windows-driver-rootkit/README.md)

## Objective
Classic LKM rootkit plus the modern eBPF variant — Phase 5 kernel skills applied offensively.

## Tasks
- [ ] LKM rootkit: syscall-table/ftrace/kprobes hooks; hide process, file, module
- [ ] `seq_file` ops hooking (hide `/proc` output); DKOM equivalents
- [ ] eBPF rootkit: BPF programs for hiding; understand lockdown/BTF restrictions
- [ ] Build Diamorphine-style, then re-implement from memory
- [ ] Detect it: auditd, kernel integrity check (lynis-style)

## Resources
- Diamorphine; phrack LKM articles; eBPF rootkit writeups; your Dojo VM

## Exit Criteria
- [ ] Working LKM rootkit + detection-bypass notes — `labs/`