# 03-05 · ARM64 Basics (Azeria)

**Week:** W17–18 · **Track:** A · **Prev:** [`../04-read-drivers-e1000-virtio-net`](../04-read-drivers-e1000-virtio-net/README.md) · **Next:** [`../06-xnu-internals-levin-v1v2`](../06-xnu-internals-levin-v1v2/README.md)

## Objective
ARM64 assembly — RISC-V knowledge transfers; needed for all iOS work.

## Tasks
- [ ] AArch64 registers (x0–x30, SP, LR, NZCV), ABI/calling convention
- [ ] Load/store, addressing modes; stack frames, prologue/epilogue
- [ ] Branching, condition codes; system instructions (EL, DAIF)
- [ ] Memory model basics (barriers, exclusive access)
- [ ] Hand-disassemble a small ARM64 binary (`otool`/`objdump`) — `labs/`

## Resources
- Azeria Labs — ARM64 series
- Apple *Writing ARM64 Code for Apple Platforms*

## Exit Criteria
- [ ] Hand-disassemble + explain a 20-instruction ARM64 function — `labs/`

## Links
- [Azeria Labs ARM64 series](https://azeria-labs.com/arm64-assembly-part-1/)
- [Armv8-A reference manual](https://developer.arm.com/documentation/ddi0487/latest)
