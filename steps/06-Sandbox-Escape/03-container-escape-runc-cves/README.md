# 06-03 · Container Escapes — Docker/runc CVEs 🚩 S1

**Week:** W35–36 · **Track:** A · **Prev:** [`../02-seccomp-syscall-escape-challenges`](../02-seccomp-syscall-escape-challenges/README.md) · **Next:** [`../04-apple-app-sandbox-seatbelt`](../04-apple-app-sandbox-seatbelt/README.md)

## Objective
Real container escapes — the boundary between your kernel-exploit skills and production impact. **New checkpoint S1.**

## Method (per CVE)
1. Read the advisory/writeup — understand the bug
2. Reproduce in a nested-Docker VM (never your host)
3. Close the POC; re-derive
4. Own walkthrough in `notes/`

## Ladder
- [ ] CVE-2019-5736 — runc file overwrite → host `runc` binary hijack
- [ ] CVE-2022-0492 — cgroup `release_agent` escape
- [ ] CVE-2024-21626 — runc WORKDIR/CWD escape
- [ ] Dirty Pipe container variant — page-cache write from inside (reuses 05-05)

## Resources
- Project Zero CVE-2019-5736 writeup + runc advisory
- Trail of Bits / NTT writeups (2022-0492, 2024-21626)
- Docker runtime internals docs

## Exit Criteria
- [ ] **S1: one container escape re-derived from scratch** — `labs/`
