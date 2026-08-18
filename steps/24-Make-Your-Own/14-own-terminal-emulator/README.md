# 24-14 · Own terminal emulator — pty + escape parser, escape-injection defense (stretch)

**Week:** W46+ opt (stretch) · **Track:** P · **Prev:** [`../13-own-container-runtime`](../13-own-container-runtime/README.md)

## Objective
Build the tool you use daily: pty/termios plumbing, an escape-code parser (CSI/OSC), screen model + renderer. The security payoff is real: terminal escape injection — log-poisoning, OSC 52 clipboard exfil, prompt-injection via attacker-controlled output. Your terminal, your defense (allow-list codes, strip vs passthrough).

## Tasks
- [ ] Pty: fork a shell on a pty (termios, winsize), read → render loop — your first "terminal"
- [ ] Parser: CSI sequences (cursor, colors, SGR), OSC (title, 52 clipboard — the exfil channel), UTF-8; a state-machine parser (the same shape as 02-17 wasm validation)
- [ ] Renderer: screen buffer (rows/cells/attrs), resize handling, scrollback; fast enough to be daily-usable
- [ ] Security: escape-injection test rig — attacker-controlled output with injected OSC 52 / cursor attacks; decision: sanitize/allow-list vs passthrough; document (pairs 21-04 log-forensics angle: logs are the injection vector)
- [ ] Self-check: daily driver for a week; injection rig: clipboard-read blocked, cursor attacks contained

## Resources
- vt100/xterm specs (ECMA-48, xterm docs); the terminal-injection research (Robin Wood et al.); pty(7)/termios man pages

## Exit Criteria
- [ ] Usable terminal (shell + scrollback + resize) — `labs/`
- [ ] Injection rig: attack output → blocked/contained, writeup — `notes/`

## Links
- [ECMA-48](https://www.ecma-international.org/wp-content/uploads/ECMA-48_5th_edition_june_1991.pdf)
- [Terminal injection research](https://github.com/noperator/term-injection)
