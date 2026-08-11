# 00-03 · Tooling — Git, Make, gdb/lldb

**Week:** W1–3 · **Track:** A · **Prev:** [`../02-memory-model-art-of-exploitation`](../02-memory-model-art-of-exploitation/README.md) · **Next:** [`../04-swift-basics`](../04-swift-basics/README.md)

## Objective
Daily-driver tooling for every later phase.

## Tasks
- [ ] Git: clone, branch, commit, rebase, stash, submodules
- [ ] Make: targets, variables, pattern rules, phony, multi-file builds
- [ ] gdb: break, step/next, `x/` memory, `bt`, `info registers`, `disas`
- [ ] lldb (macOS): same workflow
- [ ] Build the linked list from step 00-01 with a Makefile; debug a deliberate segfault

## Resources
- `git help`, `man gdb`, lldb quick-start
- K&R (toolchain usage from ch.1)

## Exit Criteria
- [ ] Debug a segfault with gdb/lldb and explain root cause — `labs/`
- [ ] `make` builds a multi-file C project from a `Makefile`
