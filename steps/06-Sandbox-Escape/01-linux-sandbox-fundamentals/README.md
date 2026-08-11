# 06-01 · Linux Sandbox Fundamentals

**Week:** W34–35 · **Track:** A · **Prev:** [`../../05-Linux-Kernel-Exploitation/09-cve-2023-32233-cross-cache-uaf`](../../05-Linux-Kernel-Exploitation/09-cve-2023-32233-cross-cache-uaf/README.md) · **Next:** [`../02-seccomp-syscall-escape-challenges`](../02-seccomp-syscall-escape-challenges/README.md)

## Objective
Know the Linux sandbox primitives cold — seccomp, namespaces, capabilities, LSMs — the target surface you will escape.

## Tasks
- [ ] seccomp-bpf: filter format, `PR_SET_SECCOMP`, `BPF_PROG_LOAD`; libseccomp
- [ ] Namespaces: pid/mount/net/user; chroot + pivot_root
- [ ] Capabilities: `capsh`, bounding set, file caps
- [ ] LSMs: AppArmor / SELinux / Landlock (what each filters)
- [ ] From the host: `nsenter`/`strace` into a Docker container and map its boundaries — `labs/`
- [ ] Write a seccomp filter allowing only `read/write/openat/exit`; prove `execve` is blocked — `code/`

## Resources
- man pages: `seccomp(2)`, `namespaces(7)`, `capabilities(7)`, `landlock(7)`
- Linux kernel docs (LSMs) + Docker runtime docs
- LKD (capabilities/LSM chapters)

## Exit Criteria
- [ ] Notes: each mechanism — what it blocks, where it lives, how to inspect it — `notes/`
- [ ] Working seccomp demo filter — `labs/`

## Links
- [cgroup v2 docs](https://docs.kernel.org/admin-guide/cgroup-v2.html)
- [namespaces (man7)](https://man7.org/linux/man-pages/man7/namespaces.7.html)
