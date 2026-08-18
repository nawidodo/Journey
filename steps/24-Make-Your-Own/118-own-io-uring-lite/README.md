# 24-118 · Own io_uring-lite — async I/O, submission/reaping, on Linux (stretch)

**Week:** W46+ opt (stretch) · **Track:** P · **Prev:** [`../117-own-gameboy-emulator`](../117-own-gameboy-emulator/README.md) · **Next:** [`../119-own-etw-consumer`](../119-own-etw-consumer/README.md) · **Pairs:** 04-08, 24-17, 24-20

## Objective
Your 24-17 server is blocking-socket I/O; Linux's modern answer is io_uring — ring-buffer async (fixed files/buffers, registered workers). Build io_uring-lite: the submission/complete ring pair (the shared-memory producer/consumer — 24-42 ring-buffer thinking), sqe/cqe struct walk (the opcode encoding — 24-20 format discipline), immediate vs deferred completion, and a stress lab: your 24-17-style echo/HTTP server on epoll vs io_uring, the ops/s + latency curves (24-30). Then the security-angle: the kernel API as attack surface (io_uring CVE class — the 05-12 fuzzing thinking against YOUR ring implementation's edge cases: full rings, dropped completions, fd reuse). Runs on your own Linux VM.

## Tasks
- [ ] Setup: io_uring_setup/enter syscalls, map the two rings, SQE/CQE layout (the ABI — header-verified)
- [ ] Ops: readv/writev/accept-now/fixed-files path; submission + reap loop (the two-phase dance — your 24-42 reuse)
- [ ] Compare: same workload via epoll (24-17) vs io_uring — ops/s + p50/p99 latency table — `labs/`
- [ ] Edge cases: full subm ring, CQE overflow, cancel; fuzz your usage with a corrupt-sqe generator (05-12 thinking) — `labs/`
- [ ] Writeup: why async I/O changes servers (the syscall cost curve), the io_uring CVE-class lesson (new kernel surface = new bugs) — `notes/`

## Resources
- io_uring man pages + kernel docs, Lord of the io_uring blog (the classic); your 24-17/24-42/24-30 code

## Exit Criteria
- [ ] Echo/HTTP server on your io_uring-lite with epoll-comparison curves — `labs/` + `code/`
- [ ] ABI + CVE-class writeup — `notes/`

## Links
- [io_uring docs](https://kernel.docs.io/) · [Lord of the io_uring](https://unixism.net/loti/)
- [io_uring CVEs (kernel.org)](https://www.kernel.org/doc/html/latest/admin-guide/bug-hunting.html)