# 04-08 · Rust for Systems Programming (memory safety)

**Week:** W14–20 (parallel) · **Track:** A · **Prev:** [`../07-apple-security-guide-ktrr-ppl-pac-aprr-ssv`](../07-apple-security-guide-ktrr-ppl-pac-aprr-ssv/README.md) · **Next:** [`../../05-Linux-Kernel-Exploitation/01-modprobe-path`](../../05-Linux-Kernel-Exploitation/01-modprobe-path/README.md)

## Objective
Read and write enough Rust to survive modern systems security work — the kernel ecosystem is moving (Linux `rust/` subsystem, Android TEE-in-Rust, Windows Rust adoption), and memory-safety claims are now part of every vendor's security story. Learn the language and, more importantly, how to *audit* `unsafe`.

## Tasks
- [ ] *The Rust Book* ch.1–15 — ownership, borrowing, lifetimes, traits
- [ ] `unsafe` rules: what it can/cannot do, the safety invariants, when kernels need it
- [ ] Reimplement one kernel-adjacent structure from Phase 1/4 in Rust: e.g. xv6 linked list/scheduler queue or a minimal slab allocator (unsafe where needed) — `code/`
- [ ] Read the Linux kernel `rust/` subsystem (Kconfig, an abi/leaf module, `kernel::` helpers) — `04-02` skills transfer
- [ ] Compare: what Rust guarantees vs C at the ABI/FFI boundary; where unsafe pragmatics live (drivers, MM, net) — 5-line `notes/` rule
- [ ] Optional: `cargo` + `rust-analyzer` toolchain tuned; clippy-clean

## Resources
- *The Rust Programming Language* ("the Book", free online)
- *Rust for Rustaceans* (Jon Gjengset) — ch. on unsafe
- Linux kernel `Documentation/rust/`, `rust/` tree
- VS Code / rust-analyzer

## Exit Criteria
- [ ] Kernel-adjacent structure compiled + tested in Rust with unsafe audit notes — `code/` + `notes/`
- [ ] "Where kernels need unsafe, and what auditing it means" explained from memory — `notes/`

## Links
- [The Rust Book](https://doc.rust-lang.org/book/)
- [Rustonomicon (unsafe)](https://doc.rust-lang.org/nomicon/)
- [Kernel Rust subsystem](https://www.kernel.org/doc/html/latest/rust/index.html)