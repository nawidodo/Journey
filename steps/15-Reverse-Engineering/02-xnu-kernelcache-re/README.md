# 15-02 · XNU Kernelcache Reverse Engineering

**Week:** W17–18 · **Prev:** [`../01-ghidra-disassembly-workflow`](../01-ghidra-disassembly-workflow/README.md) · **Next:** [`../03-practice-crackmes-flareon`](../03-practice-crackmes-flareon/README.md)

## Objective
Apple's release kernels are stripped and encrypted at rest — the skill that makes Phase 7 (iOS) and Track F (checkm8) possible.

## Tasks
- [ ] Get a decrypted kernelcache (own device, documented decryption tools)
- [ ] `kcache_id`, `jtool`, `kerneldiff`: extract, identify, diff
- [ ] Recover symbols/signatures; map a struct in Mach-O `__DATA`; find a known function via xrefs
- [ ] Reconstruct one IOKit method from an unsymbolicated image
- [ ] Triage one real iOS crash log → the frame the kernelcache explains

## Resources
- Levin Vol 1; `jtool`/`kcache_id` docs; iPhoneWiki kernelcache pages
- Your own Apple device's firmware

## Exit Criteria
- [ ] One function fully reconstructed from a stripped kernelcache — `notes/`

## Links
- [jtool (newosxbook)](http://newosxbook.com/tools/jtool.html)
- [kcache_id](https://github.com/blacktop/kcache_id)
