# 24-38 · Own text editor — the classic systems build (stretch)

**Week:** W46+ opt (stretch) · **Track:** P · **Prev:** [`../37-own-chess-engine`](../37-own-chess-engine/README.md) · **Next:** [`../39-own-package-manager`](../39-own-package-manager/README.md)

## Objective
The rite of passage (kil-style): a text editor — raw terminal mode (pair with your 24-14 terminal emulator: now you own both ends of the pty), text buffer (gap buffer — the data structure), search, syntax highlight, save/load. The syssec tie-in: every editor is a file-parser, and parsing is the attack surface (pairs 24-27 PNG, 12-01 PE) — plus it becomes your daily driver, which is the real test.

## Tasks
- [ ] Terminal: raw mode, key input, screen repaint (ANSI — you wrote the parser in 24-14, now emit it)
- [ ] Buffer: gap buffer (why editors use it), insert/delete/undo, cursor movement
- [ ] Features: search (reuse 24-06 regex), syntax highlight (a tiny lexer), multi-file, save/load
- [ ] Robustness lab: open hostile files (huge lines, CRLF vs LF, invalid UTF-8, embedded escape sequences — your 24-14 injection lesson from the other side) — clean handling — `labs/`
- [ ] Self-check: write a week's worth of notes in your own editor; the dogfood test

## Resources
- "Build Your Own Text Editor" (the manual); kilo source (peer); your 24-06/24-14 notes

## Exit Criteria
- [ ] Usable editor: buffer/search/highlight/undo — `labs/`
- [ ] Hostile-file clean-handling lab — `labs/` + `notes/`

## Links
- [Build Your Own Text Editor](https://viewsourcecode.org/snaptoken/kilo/)
- [kilo](https://github.com/antirez/kilo)
