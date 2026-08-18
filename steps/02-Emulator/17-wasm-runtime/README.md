# 02-17 · Own WebAssembly runtime — interpreter, then JIT (stretch)

**Week:** W18+ stretch · **Track:** B · **Prev:** [`../16-emulator-shaders`](../16-emulator-shaders/README.md)

## Objective
The era's portable sandbox: WASM runs in every browser, edge runtime, and plugin system. Build a runtime: module decode (the binary format) → validation → interpreter → JIT (stack machine → native). The sandbox lesson is the point — pairs 08 browser-track escapes and your emulator ladder.

## Tasks
- [ ] Decoder + validator: wasm binary format (sections, types, function bodies), validation (types, stack discipline — the "no bad stack" invariant that makes it sandboxable)
- [ ] Interpreter: stack machine, control flow (blocks/loops/calls), linear memory (the sandbox boundary — bounds-checked, no host pointers)
- [ ] JIT: tier up — a simple x86-64 JIT for a subset (or baseline compiler); keep interpreter for cold code (the two-tier pattern real runtimes use)
- [ ] Host interface: imports/exports, WASI-style syscalls through your own host ABI; then *break your own sandbox* — memory OOB, stack overrun, host-call confusion (pairs 08-03-style thinking)
- [ ] Self-check: run a wasm-compiled C program (clang --target=wasm32) end-to-end; sandbox-escape attempt fails with a clean trap

## Resources
- WebAssembly spec; wabt tools; SpiderMonkey/V8 baseline-compiler notes; your 02-01 CHIP-8 → 02-14 emulator experience (same ladder)

## Exit Criteria
- [ ] Runtime executes a wasm32 C program (interpreter + JIT path) — `labs/`
- [ ] Escape-attempt trap log — `labs/`

## Links
- [WebAssembly spec](https://webassembly.github.io/spec/)
- [wabt](https://github.com/WebAssembly/wabt)
