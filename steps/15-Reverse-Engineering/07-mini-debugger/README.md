# 15-07 · Build a mini-debugger — ptrace, breakpoints, single-step (stretch)

**Week:** W20 parallel (stretch) · **Track:** — · **Prev:** [`../06-symbolic-execution`](../06-symbolic-execution/README.md)

## Objective
You've driven gdb/lldb through every exploit and RE step — build one. ptrace-based debugger in C: attach, breakpoints, single-step, registers/memory inspect. The systems-density of a debugger is a semester compressed.

## Tasks
- [ ] ptrace model: `PTRACE_ATTACH/TRACEME`, `PEEKDATA/POKEDATA`, `GETREGS`, `SINGLESTEP`, waitpid event loop; why ptrace is the boundary (pairs Track K hooking — the same seam)
- [ ] Breakpoints: software (int3/0xCC swap + save/restore) vs hardware (`dr0–3`); trap handling in the event loop
- [ ] Core loop: load binary → set breakpoint on `main` → run → step → dump regs + memory; symbol resolution from ELF symtab
- [ ] Extras if cheap: watchpoints, `continue`, disassembly of current instruction (pairs 15-08), attach to a running process
- [ ] Self-check: your debugger reproduces one 04-01 exercise step-for-step (overflow → RIP overwrite observed in your own regs dump)

## Resources
- `man 2 ptrace`; Hoglund's debugging material; "How debuggers work" (old Eli Bendersky series); your 04-01/04-02 pwntools notes

## Exit Criteria
- [ ] Working mini-debugger: breakpoint → single-step → regs/mem dump on an ELF — `labs/` (code + one session log)
- [ ] Reproduced 04-01 overflow under your own debugger — `labs/`

## Links
- [ptrace(2)](https://man7.org/linux/man-pages/man2/ptrace.2.html)
- [Eli Bendersky — How debuggers work](https://eli.thegreenplace.net/tag/debugging)
