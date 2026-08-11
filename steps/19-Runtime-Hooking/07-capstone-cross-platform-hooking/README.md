# 19-07 · Capstone: One Harness on All Platforms 🚩 M20

**Week:** W31–32 · **Track:** K · **Prev:** [`../06-android-hooking`](../06-android-hooking/README.md)

## Objective
One interception harness — "log calls to a target function, modify one argument, restore" — on ≥5 platforms, and compare.

## Tasks
- [ ] Pick a target function per platform (e.g., the file-write / `getenv` equivalent)
- [ ] Port your own engine to Windows, Linux, macOS, iOS, Android — same harness, your code
- [ ] Frida only to verify your results match
- [ ] Comparison table: mechanism, detectability, privileges needed, persistence
- [ ] Detection writeup: how each would be caught by a memory scanner
- [ ] Defensive mirror: write a scanner that finds your own hooks

## Resources
- Your 19-01–06 outputs

## Exit Criteria
- [ ] **M20:** own from-scratch engine on ≥5 platforms + comparison — `code/` + `notes/`

## Links
- [Frida internals](https://frida.re/docs/hooking/)
- [Your 19-01–06 outputs] — composition step
