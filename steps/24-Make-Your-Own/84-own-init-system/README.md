# 24-84 · Own init system — service deps, socket activation, cgroups-lite (stretch)

**Week:** W46+ opt (stretch) · **Track:** P · **Prev:** [`../83-own-backup-tool`](../83-own-backup-tool/README.md) · **Next:** [`../85-own-config-manager`](../85-own-config-manager/README.md) · **Pairs:** 24-01, 24-13, 25-04

## Objective
PID 1 decides what runs: build an init-lite — service units (declarative config — 24-09 parser), dependency ordering + parallel start (DAG — 24-72), restart policies (24-48 reconciliation), and cgroups-lite resource control (24-13). The security angle is the payload: 25-04 privesc surfaces (world-writable unit files, unit symlink attacks) are only real because init trusts config — your lab: a malicious unit file escapes the sandbox-ish semantics, then the fix (ownership validation). Boot your 24-01 kernel to your init as PID 1 (the capstone moment).

## Tasks
- [ ] Unit model: service/target/oneshot types, config parse, dependencies (After/Requires-lite)
- [ ] Manager: ordered start, parallel where safe, status transitions, restart-on-fail with backoff (24-48)
- [ ] cgroups-lite: CPU/mem limits on your services (24-13 reuse), per-service isolation
- [ ] Security lab: unit file owned by user with setuid binary → privesc demo on your own machine VM; fix with ownership/immutability checks — `labs/`
- [ ] Self-check: 24-01 boots → your init starts your scheduler + network stack (24-05) as ordered units

## Resources
- systemd.unit man + docs (the manual); your 24-01/24-13/24-48 code

## Exit Criteria
- [ ] Init boots 24-01 services with deps, restarts, and cgroup limits — `labs/`
- [ ] Unit-trust privesc lab + fix — `labs/` + `notes/`

## Links
- [systemd.unit](https://www.freedesktop.org/software/systemd/man/latest/systemd.unit.html)
- [systemd (GitHub)](https://github.com/systemd/systemd)