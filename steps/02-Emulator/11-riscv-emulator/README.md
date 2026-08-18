# 02-11 · RISC-V emulator — rv32ima from the spec, run bare-metal firmware (stretch)

**Week:** W18+ stretch · **Track:** B · **Prev:** [`../10-gba-emulator`](../10-gba-emulator/README.md)

## Objective
The cleanest modern ISA — spec is short enough to hold in your head, no legacy baggage (the anti-x86). Build rv32ima from the spec, run bare-metal firmware, add a GDB stub, and debug 22-09-style firmware with it. The emulator as RE instrument.

## Tasks
- [ ] Core: rv32i base (I) + M extension (mul/div) + A (atomics, lr/sc — the fun part) + F (floats, or defer); privileged mode csrs as needed for bare-metal
- [ ] Memory + platform: UART + timer (CLINT-style), load ELF; run a bare-metal hello + a small RTOS (Zephyr/RTIC sample) — boots real firmware, not toy code
- [ ] GDB stub: `target remote :1234` — breakpoints, single-step, regs; then debug one of your own 22-09 firmware bugs in it (pairs 15-07 mini-debugger: same protocol, different core)
- [ ] Self-check: rv32ima runs a spec-conformant test suite (riscv-tests) green; GDB session log shows a breakpoint hit + register inspect

## Resources
- RISC-V unpriv spec vol. 1; riscv-tests; QEMU's riscv32 target as behavioral reference

## Exit Criteria
- [ ] rv32ima passes riscv-tests (your implemented subset) — `labs/`
- [ ] Bare-metal firmware boots + GDB stub single-step session — `labs/`

## Links
- [RISC-V spec](https://riscv.org/technical/specifications/)
- [riscv-tests](https://github.com/riscv-software-src/riscv-tests)
