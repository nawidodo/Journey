# 24-13 · Own container runtime — runc-lite from scratch (stretch)

**Week:** W46+ opt (stretch) · **Track:** P · **Prev:** [`../12-own-ssh-server`](../12-own-ssh-server/README.md) · **Next:** [`../14-own-terminal-emulator`](../14-own-terminal-emulator/README.md)

## Objective
Containers are every deployment. Build a runc-lite: namespaces + cgroups + pivot_root + seccomp, zero Docker, from your own code. The security half: it's your runtime, so break it — escape tests, capability audit, seccomp-rule gaps. Pairs 06 sandbox-escape track and your 06-10 seccomp sandbox builder (same syscalls, opposite role).

## Tasks
- [ ] Isolation: unshare/CLONE_NEW* (pid/mnt/net/uts/user), pivot_root + chroot, mount /proc — a process that *looks* contained
- [ ] Limits: cgroups v2 (cpu/mem/pids) — enforce, then demonstrate a killer OOM-capped child
- [ ] Hardening: drop capabilities, seccomp filter (reuse 06-10 builder's rule language), read-only rootfs
- [ ] Break it: escape attempts — classic syscall escapes (mount, ptrace, user-ns), capability misuse; document each in a matrix (the OCI security docs shape) — `notes/`
- [ ] Self-check: `docker run`-equivalent: your runtime starts busybox, kills it, reaps it; escape attempts all fail

## Resources
- OCI runtime-spec; runc source (peer); lxc docs; your 06 sandbox-escape + 06-10 notes

## Exit Criteria
- [ ] Runtime runs a busybox rootfs isolated + limited + seccomp'd — `labs/`
- [ ] Escape-attempt matrix: attempted, blocked, why — `notes/`

## Links
- [OCI runtime-spec](https://github.com/opencontainers/runtime-spec)
- [runc](https://github.com/opencontainers/runc)
