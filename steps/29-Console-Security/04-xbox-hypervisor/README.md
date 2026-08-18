# 29-04 · Xbox One/Series — Hyper-V-grown hypervisor, Secure Kernel (Phase 29)

**Week:** W41+ opt (niche) · **Track:** U · **Prev:** [`../03-nintendo-switch-tegra`](../03-nintendo-switch-tegra/README.md) · **Next:** [`../05-capstone-platform-matrix`](../05-capstone-platform-matrix/README.md) · **Pairs:** 24-13, 06-06, 04-07, 20-10

## Objective
Xbox One/Series runs x86-64 AMD silicon under a Hyper-V-derived hypervisor: every game is a *guest*, the OS core is the Hypervisor + Secure Kernel pair — the closest a consumer box gets to your 24-13 container runtime and 06-06 isolation study. Study the architecture: boot (UEFI-based → HV → Secure Kernel trim), the HV/SK split (who enforces what — compare to ARM TrustZone on Switch 29-03 and Apple SEP 07), the driver/userland model (every partition a guest — the 24-48 orchestrator pattern), and the public research state (hypervisor fuzzing/RE writeups — the 05-12 coverage-fuzzer thinking). The lab: stand up your own VM pair — kernel guest under a hypervisor you configure (or your 24-13 container equivalent) and measure isolation: a crashing guest must not hurt the host (24-30 measurement) — the mini proof that HV isolation is the security boundary, exactly Microsoft's design. Own-consoles lab-only; research is the core.

## Tasks
- [ ] Architecture map: boot chain + HV/SK split + partition model — the 29-01 matrix row
- [ ] Comparison: HV/SK vs Switch TZ (29-03) vs iOS SEP (07) — three hardware-trust designs, one principle (keys + isolation + signed everything)
- [ ] Lab: nested/configured VM — guest crash vs host health, the isolation metric (reboot/uptime/IO unaffected) + your own hypervisor-lite notes from 04-07 — `labs/`
- [ ] Research survey: public Xbox hypervisor/kernel work (fuzzing, debug docs), why the surface is thin vs PS4 (no browser attack corridor — the 29-01 entry-surface insight)
- [ ] Writeup: the case for hypervisor-as-norm (the 24-13/06-06 future: every app a guest) — `notes/`

## Resources
- Microsoft's public HV docs, console research writeups; your 24-13/06-06/04-07 notes

## Exit Criteria
- [ ] VM isolation lab with crash-vs-host data — `labs/` + `code/`
- [ ] Three-platform trust comparison writeup — `notes/`

## Links
- [Xbox security overview](https://learn.microsoft.com/en-us/windows/security/hardware-security/hardware-security-and-isolation)
- [Hyper-V architecture docs](https://learn.microsoft.com/en-us/virtualization/hyper-v-on-windows/)