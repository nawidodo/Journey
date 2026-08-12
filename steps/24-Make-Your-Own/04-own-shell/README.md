# 24-04 · Own UNIX Shell

**Week:** W46+ (parallel, optional, stretch) · **Track:** P · **Prev:** [`../03-own-git`](../03-own-git/README.md) · **Next:** [`../05-own-tcp-ip-stack`](../05-own-tcp-ip-stack/README.md)

## Objective
A working shell: tokenizer, parser, exec/fork/wait pipelines, redirection, a couple builtins. You'll do every one of these primitives in Phase 1 (xv6) and Phase 6 (process/sandbox work) — this makes the syscall/process semantics second nature before those phases.

## Tasks
- [ ] Read-eval-loop, whitespace/quote tokenizer + parse — `code/`
- [ ] `fork`/`exec`/`wait` for external commands + `PATH` lookup — `code/`
- [ ] Pipelines `a | b | c` with the process table — `code/`
- [ ] Redirection `< > >>` via `dup2` — `code/`
- [ ] Stretch: job control / `SIGINT` handling — `code/`
- [ ] Debrief: which primitives appear again in Phase 1 xv6 shell lab — `notes/`

## Resources
- "Build your own shell" (tutorials on classic shell parsing) — many walkthroughs
- Your Phase 0 reading on POSIX process APIs

## Exit Criteria
- [ ] Shell runs real pipelines + redirection — `code/`
- [ ] Debrief note — `notes/`

## Links
- [build-your-own-x: own shell](https://github.com/codecrafters-io/build-your-own-x?tab=readme-ov-file#build-your-own-shell)
- [tzuyi / shell tutorial](https://github.com/tokenrove/shell-workshop) (deep-dive)