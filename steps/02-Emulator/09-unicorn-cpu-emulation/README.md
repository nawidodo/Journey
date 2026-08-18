# 02-09 · CPU emulation as a security tool — Unicorn Engine (stretch)

**Week:** W18+ (stretch) · **Track:** B · **Prev:** [`../08-ios-app-on-device`](../08-ios-app-on-device/README.md)

## Objective
You built emulators for fidelity. Flip it: emulation as an *instrument* — run and trace attacker code without hardware, no OS, no interrupts. Unicorn is angr's default CPU backend; this is the bridge from Track B to RE (15), malware (12), and fuzzing (05-11).

## Tasks
- [ ] Unicorn basics: `UC_ARCH_X86_64` / `UC_ARCH_ARM`, memory mapping, `uc_mem_write/read`; why it's an *engine* (no MMU/IO/peripherals — and why that makes it fast)
- [ ] Shellcode trace: load your 04-01 `execve` shellcode, `hook_code` every instruction, log register deltas; verify the syscall path by hand
- [ ] Unpacking: a self-decrypting loop (e.g., XOR stub) — run until the decrypt loop finishes, dump the decrypted region; the primitive real unpackers use (pairs Track E 03 evasion, 15 RE)
- [ ] angr integration: confirm angr runs your 15-06 challenge on Unicorn's backend; where angr's execution *is* Unicorn and where it diverges (SimProcedures)
- [ ] Fuzzing harness: a unicorn-based harness for one parser function (cross-ref 05-11); compare speed vs QEMU full-system
- [ ] Contrast writeup: emulator-as-tool vs full-system (QEMU) vs your NES core — fidelity vs speed vs instrumentation — `notes/`

## Resources
- Unicorn docs + examples; angr docs (backend); awesome-emulators (x86 emulators section)

## Exit Criteria
- [ ] Shellcode trace + one unpacking dump + angr-under-Unicorn solve — `labs/`
- [ ] Tool-vs-full-system comparison table — `notes/`

## Links
- [Unicorn Engine](https://www.unicorn-engine.org/)
- [awesome-emulators](https://github.com/DerekTurtleRoe/awesome-emulators)
- [angr (cross-ref 15-06)](../15-Reverse-Engineering/06-symbolic-execution/README.md)
