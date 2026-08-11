# 08-04 · Optional — Original Research: syzkaller on XNU

**Week:** capstone+ · **Track:** A (optional) · **Prev:** [`../03-pwn2own-mobile`](../03-pwn2own-mobile/README.md) · **Next:** —

## Objective
Turn skills into original findings: fuzz XNU with syzkaller.

## Tasks
- [ ] Build syzkaller for macOS (`syz-fuzzer` on macOS/VM, syz-manager)
- [ ] Corpus: XNU/IOKit syscalls + mach traps
- [ ] Triage crashes; build minimal reproducers
- [ ] If a bug is found: report / write a PoC — `labs/`

## Resources
- `google/syzkaller` docs (macOS support)
- XNU source (apple-oss-distributions)

## Exit Criteria
- [ ] Fuzzing loop running + ≥1 triaged crash (bug or benign panic)
