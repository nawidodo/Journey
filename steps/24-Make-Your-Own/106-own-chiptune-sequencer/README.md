# 24-106 · Own chiptune sequencer — MIDI-lite events, patterns, your synth voice (stretch)

**Week:** W46+ opt (stretch) · **Track:** P · **Prev:** [`../105-own-logarchive-parser`](../105-own-logarchive-parser/README.md) · **Next:** [`../107-own-subtitle-engine`](../107-own-subtitle-engine/README.md) · **Pairs:** 24-40, 24-100, 24-89, 24-14

## Objective
Tracker/DAW magic, reduced to the fun core: sequence music with code — a chiptune sequencer-lite — event model (note on/off, velocity, tempo map — MIDI-style events, pairs 89 timing discipline), pattern grid (steps → rows, loops — the data structure 24-72-style), song assembly (patterns → order list), and a voice: your 24-40 synth as a square/triangle voice with pitch slides (the chiptune sound). Front it with a 24-14 TUI (edit pattern, play, mute) and render to WAV through your 24-100 editor as a master.

## Tasks
- [ ] Model: note events + timing (ticks/tempo, quantize), channel/voice mapping, MIDI-lite semantics (vel/pitch 0-127)
- [ ] Pattern: step-grid editor (rows × steps), pattern loop, transpose; the order list (song structure)
- [ ] Voice: square/pulse with duty + slide (24-40 oscillators), noise percussion (pairs 24-89 source switching), vibrato-lite
- [ ] Song: pattern chaining, jump/restart, per-channel mute; live play via 24-14 keys (24-38)
- [ ] Render: export through 24-100 WAV path; A/B your own covers (record the same notes two ways — waveform diff) — `labs/`
- [ ] Writeup: why the tracker model survived (pattern reuse, live control), MIDI's 1983 design notes — `notes/`

## Resources
- Tracker architecture notes (Renoise/OpenMPT source — peer); your 24-40/24-100/24-89/24-14 code

## Exit Criteria
- [ ] Pattern → song → rendered master with your synth voice — `labs/` + `code/`
- [ ] Design/timing writeup — `notes/`

## Links
- [OpenMPT](https://openmpt.org/)
- [MIDI 1.0 spec](https://midi.org/specifications)