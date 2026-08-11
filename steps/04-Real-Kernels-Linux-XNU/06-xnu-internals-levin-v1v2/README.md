# 03-06 · XNU Internals — Levin Vol 1–2

**Week:** W18–19 · **Track:** A · **Prev:** [`../05-arm64-basics-azeria`](../05-arm64-basics-azeria/README.md) · **Next:** [`../07-apple-security-guide-ktrr-ppl-pac-aprr-ssv`](../07-apple-security-guide-ktrr-ppl-pac-aprr-ssv/README.md)

## Objective
Apple's kernel: Mach IPC, vm_map, code signing, AMFI.

## Tasks
- [ ] Mach ports, messages, IPC model
- [ ] `vm_map` — virtual memory, zones
- [ ] BSD layer over Mach (proc, tasks)
- [ ] Code signing, entitlements, AMFI in XNU
- [ ] Read against `apple-oss-distributions/xnu` source

## Resources
- *Mac OS X and iOS Internals* Vol 1–2 (Jonathan Levin)
- `apple-oss-distributions/xnu` on GitHub

## Exit Criteria
- [ ] Notes: Mach IPC + `vm_map` structures — `notes/`
- [ ] Explain where AMFI checks run

## Links
- [newosxbook.com (Levin)](http://newosxbook.com/)
- [XNU source (apple-oss-distributions)](https://github.com/apple-oss-distributions/xnu)
