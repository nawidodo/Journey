# 06-02 · Seccomp / Syscall Escape Challenges

**Week:** W34–35 · **Track:** A · **Prev:** [`../01-linux-sandbox-fundamentals`](../01-linux-sandbox-fundamentals/README.md) · **Next:** [`../03-container-escape-runc-cves`](../03-container-escape-runc-cves/README.md)

## Objective
Escape naive seccomp filters using only what's still allowed — the practical side of sandbox analysis.

## Tasks
- [ ] Allowed-syscall exploitation: ORW chains (`openat`→`read`→`write`), flag reads through allowed surfaces
- [ ] Avoid blocked syscalls: `memfd_create` + `fexecve`, `openat2`, io_uring side channels, `pidfd_getfd`
- [ ] ptrace-based escalation against a sibling sandboxed process
- [ ] Solve sandboxed CTF / pwn.college challenges solo; log each in `notes/`
- [ ] Tools: seccomp-tools, strace, gdb, pwntools

## Resources
- pwn.college (seccomp/sandboxing modules)
- ctf-wiki.org (seccomp page)
- seccomp-tools (david942j)

## Exit Criteria
- [ ] ≥3 sandboxed challenges solved solo, each with a distinct bypass technique — `labs/`

## Links
- [seccomp man page](https://man7.org/linux/man-pages/man2/seccomp.2.html)
- [seccomp filters (kernel docs)](https://docs.kernel.org/userspace-api/seccomp_filter.html)
