# 24-110 · Own arm64 playground — shellcode-lite on your Apple Silicon (stretch)

**Week:** W46+ opt (stretch) · **Track:** P · **Prev:** [`../109-own-plist-parser`](../109-own-plist-parser/README.md) · **Next:** [`../111-own-metal-compute`](../111-own-metal-compute/README.md) · **Pairs:** 07-06, 15-08, 02-11, 24-97

## Objective
Your Mac is arm64; every iOS/XNU exploit (07) assumes you read this ISA. Build the sandbox: a runner that `mmap`s W+X-modifiable pages, writes your own arm64 snippets as bytes, executes them, and prints the trace — a shellcode lab on your own machine (no hardware beyond the Mac). Cover the platform essentials: registers/ABI (`x0-x30`/sp/cpsr, the calling conv), syscalls (`svc #0x80` — the macOS ABI vs Linux `svc 0` difference, pairs 24-19/24-20 syscall-table discipline), literal pools, `b.cond`/branch.lt, and position-independence (PC-relative `adrp/ldr` — the `adr`/`adrp` dance every macOS exploit trips on). Then the double-purpose: emit these snippets with your 15-08 disassembler and 24-97 parser as the verifier, and pattern-match your own 04-06 kernel exploit's gadgets.

## Tasks
- [ ] Runner: `mmap(PROT_READ|WRITE)` → write bytes → `mprotect` to RX → call via function pointer; the execution harness (24-16 page discipline)
- [ ] ISA drills: mov/add/ldr/str variants, conditionals, the literal pool trap (encoding offsets), stack discipline — each with a print-trace
- [ ] Syscalls: `svc` with macOS conventions (exit, write, mmap — pair 24-20 wire formats); the x0=sysno/x1.. args table
- [ ] Position-independence: `adrp+add` vs `adr`, global-offset loads; compile a PIC loop that runs at two different bases (the 07 jailbreak pattern)
- [ ] Verification: assemble with clang, compare against your hand bytes (oracle); disassemble your snippets with 15-08 — both agree
- [ ] Writeup: arm64 vs x86-64 (why the ISA change broke a generation of exploits), what PC-relative code means for JIT/shellcode (pairs 02-17) — `notes/`

## Resources
- ARMv8 ABI + ISA reference (the manual); Apple's `arm64` calling convention doc; your 24-97/15-08/24-16 code

## Exit Criteria
- [ ] Runner executes hand-written snippets incl. syscall + PIC at two bases — `labs/` + `code/`
- [ ] ABI/ISA writeup — `notes/`

## Links
- [Apple ARM64 Function Calling Conventions](https://developer.apple.com/documentation/xcode/writing-arm64-code-for-apple-platforms)
- [ARM A64 ISA](https://developer.arm.com/documentation/ddi0487/latest/)