# 24-68 · Own terminal multiplexer — tmux-lite on your terminal emulator (stretch)

**Week:** W46+ opt (stretch) · **Track:** P · **Prev:** [`../67-own-adblock-dns`](../67-own-adblock-dns/README.md) · **Next:** [`../69-own-webauthn-authenticator`](../69-own-webauthn-authenticator/README.md) · **Pairs:** 24-14, 24-38

## Objective
Your 24-14 terminal emulator renders one shell; tmux gives you ten. Build a multiplexer: session/window/pane model, each pane its own pty (reuse 24-14 pty discipline), scrollback buffer, status bar, keybindings (24-38 input loop). Then the hard part: **layout negotiation** — how tmux spreads a pane tree across a terminal of unknown size (split/reflow math, the SGR stuff) — and copy/paste + scrollback. Finished when you can detach, reattach, and split your own shell spread.

## Tasks
- [ ] Core: session/pane registry, pty per pane (24-14), input routing (focus pane), rendering the pane grid to one terminal
- [ ] Layouts: split (h/v), resize, reflow on terminal resize (the negotiation math); panes keep state
- [ ] Scrollback: ring buffer per pane (24-42 ideas), copy mode + selection, paste to pane
- [ ] Lifecycle: detach/attach (server alive without client), status bar with pane titles
- [ ] Self-check: detach, close SSH (24-12), reattach — everything intact; split into 2x2 and run distinct work in each

## Resources
- tmux source + man (the manual); your 24-14/24-38 code

## Exit Criteria
- [ ] Multiplexer with panes/scrollback/detach-reattach — `labs/` + `code/`
- [ ] Resize-reflow + layout notes — `notes/`

## Links
- [tmux](https://github.com/tmux/tmux)
- [tmux man](https://man.openbsd.org/tmux)