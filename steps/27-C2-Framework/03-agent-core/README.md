# 27-03 · Agent core — cross-platform implant with tasking

**Week:** W32–34 · **Track:** S · **Prev:** [`../02-wire-protocol-transport`](../02-wire-protocol-transport/README.md) · **Next:** [`../04-teamserver-operator-ui`](../04-teamserver-operator-ui/README.md)

## Objective
The implant that lives on target. Cross-platform (Go) agent: check-in, task execution, output relay, sleep obfuscation, kill switch — reusing Track E's implant/evasion work where it applies.

## Tasks
- [ ] Go agent (static, cross-compile to Windows/Linux/macOS): check-in loop with the protocol from step 02, task fetch, job execution
- [ ] Job types: shell/exec (with output capture), upload/download (chunked, resume), file listing, sleep set, self-terminate; argument validation — no shell-injection footguns
- [ ] In-memory only: no config on disk (compile-in defaults), no log files; self-delete option; fail-closed on bad crypto (step 02)
- [ ] Sleep obfuscation: memory encryption while sleeping (Windows: VirtualAlloc RW → encrypt → Protect → suspend/resume timing; Linux/macOS: mprotect + encrypt) — reuse Track E 04's approach
- [ ] Process injection: on Windows, one inject route for task execution (reuse Track E 02 techniques, e.g. APC/thread hijack) so tasks run in a sacrificial process; Linux/macOS: fork/exec with stdout capture
- [ ] Kill switch: remote self-destruct (server command) + fail-close behavior; test both
- [ ] Anti-analysis basics (links Track E 03): static flag entropy, running under debugger detect — keep minimal, note the tradeoff

## Resources
- Go stdlib (crypto, net), Track E 01–04 outputs, Sliver agent source (`sliver/implant/`), MalDev Academy

## Exit Criteria
- [ ] Agent runs on Windows + Linux VMs, executes tasks, relays results — `labs/`
- [ ] No persistent disk artifacts (config, logs) — verify with `lsof`/`proc` scan — `labs/`
- [ ] Sleep obfuscation observed (memory not plaintext while sleeping) — `labs/`
- [ ] Explain in ≤5 lines your fail-closed behavior and kill-switch path — `notes/`

## Links
- [Sliver implant source](https://github.com/BishopFox/sliver/tree/master/implant)
- [ired.team injection](https://www.ired.team/offensive-security/code-injection-process-injection)
- [MalDev Academy](https://maldevacademy.com/)
