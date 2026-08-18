# 06-10 · Build a seccomp sandbox — filter DSL → BPF, then escape your own filter (stretch)

**Week:** W39 stretch · **Track:** A · **Prev:** [`../09-own-hypervisor`](../09-own-hypervisor/README.md)

## Objective
Phase 6 read escape papers. Build the sandbox side: a small filter DSL compiled to seccomp-BPF, dropped on a target process — then attack your own filter with 02's techniques until you're the escapee. Both sides of the boundary in one artifact.

## Tasks
- [ ] seccomp model: modes, `SECCOMP_RET_*` actions, BPF instruction set (`BPF_LD/BPF_JMP`), `prctl`/`seccomp(2)` install paths; what the filter cannot see (args dereferencing, pointers)
- [ ] DSL → BPF: allow/deny syscall rules + arg checks (e.g., deny `open` with `O_WRONLY`, allow read-only); compile to a real filter program; test with `seccomp-tools` / strace
- [ ] Install on a target: exec a child under your filter; verify denials (EPERM/EACCES) and the log line
- [ ] Escape round (pairs 06-02): attack your own filter — syscall-number aliasing, argument-confusion (ioctl/arch tricks), time-of-check gaps; document what makes a seccomp policy actually sound
- [ ] Self-check: your filter blocks its own escape PoC after you close the hole you found

## Resources
- seccomp man pages + kernel docs; seccomp-tools; your 06-02 challenge notes; KernelCI seccomp samples

## Exit Criteria
- [ ] DSL compiler → working filter → target running under it, denials observed — `labs/`
- [ ] Escape attempt + fix cycle documented: one hole found in your own policy, closed — `labs/` + `notes/`

## Links
- [seccomp(2)](https://man7.org/linux/man-pages/man2/seccomp.2.html)
- [seccomp-tools](https://github.com/david942j/seccomp-tools)
