# 19-06 · Android Hooking

**Week:** W29–31 · **Track:** K · **Prev:** [`../05-ios-hooking`](../05-ios-hooking/README.md) · **Next:** [`../07-capstone-cross-platform-hooking`](../07-capstone-cross-platform-hooking/README.md)

## Objective
The Android hooking stack — ART/dex + native. Rootable AVD only.

## Tasks
- [ ] Own native GOT hook: port your fishhook-equivalent to Android (ELF)
- [ ] Own ARM64 inline hook: trampoline emitter from scratch
- [ ] LSPosed/Frida stay for ART/Java-level — from-scratch ART method hooking is research-grade; note what they patch under the hood
- [ ] Where Play Protect / malware detection flags hook frameworks
- [ ] Cross-ref Track H: hooking as an exploitation primitive

## Resources
- LSPosed docs; bhook/Dobby; Frida; rootable AVD

## Exit Criteria
- [ ] Own native hooks + tool-based Java hooks on AVD — `code/` + `labs/`

## Links
- [LSPosed](https://github.com/LSPosed/LSPosed)
- [bhook (ByteDance)](https://github.com/bytedance/bhook)
