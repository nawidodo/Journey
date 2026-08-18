# Emulator & VM Path — write your own machine, from CHIP-8 to PS1 to hypervisor

Goal: build machines from scratch — a working PS1 (PlayStation) emulator and a real virtualization layer — so the CPU/GPU/timing/agent boundary stops being a black box. Same contract: **gate unlocks next only when artifact exists**; no skip; own machine; artifact = runnable (boots a ROM/demo + test-suite pass), not prose.

Prereq: **G0 (JAILBREAK-ROOT-PATH.md): C (00-01), memory model (00-02)**. The xv6 lane helps timing/OS-interface intuition but is not required here.

---

## E1 — the first machine: CHIP-8
Finish: `02-01` chip8-cli — full fetch/decode/execute, input, timers, display.
**Gate artifact:** `labs/e1.md` — CHIP-8 interpreter runs 3 test ROMs (IBM logo + games), keymap documented. → unlocks E2.

## E2 — the real thing: NES (6502 system, complete)
Finish: `02-02` 6502 CPU → `02-03` PPU → `02-04` APU → `02-05` cart mappers/ROM loader — the full console: instruction set, how pixels happen, how sound happens, how banking happens.
**Gate artifact:** `labs/e2.md` — a commercial-class ROM boots to gameplay on your core + the 6502 test- suite passes. → unlocks E3.

## E3 — second CPU, pipeline reality: GBA or SNES
Finish: pick one: `02-10` GBA (ARM7TDMI: ARM+Thumb states, timers, DMA) or `02-12` SNES (65C816 + Mode 7 + SPC700 coprocessor). Both teach the "more than one thing at once" lesson.
**Gate artifact:** `labs/e3.md` — second core boots its test ROM; writeup: what changed from 6502 (state math, bus multiplex, coprocessor choreography). → unlocks E4.

## E4 — 3D: the PS1 (the target you named)
Finish: `02-14` PS1 — MIPS R3000 core → GTE (geometry) → polygon rasterizer → GPU/display pipeline → SPU audio → CD/controller DMA. This is THE project: a complete 1994 console.
**Gate artifact:** `labs/e4.md` — your PS1 core boots a demo/homebrew to 3D gameplay (or a documented partial with honest limits), CPU+GTE+GPU each verified against a public test suite. → unlocks E5.

## E5 — the VM layer: virtualization, not just emulation
Finish: `02-11` RISC-V emulator (small clean ISA — the "new emulator on new silicon" mindset, a.k.a. the Asahi template) + `06-06` VMM/hypervisor fundamentals (the privilege-layer machine your kernel will live under) + `24-81` unikernel (single-app guest: size/boot/attack-surface table).
**Gate artifact:** `labs/e5.md` — your RISC-V emulator boots a kernel you wrote (24-10 µkernel pairing optional but ideal); a VM/hypervisor threat-model diagram of your own PS1 core as the "guest". → unlocks E6.

## E6 — emulation as tool & debugged machine
Finish: `02-09` Unicorn (CPU emulation as RE/security tooling: shellcode tracing, unpacking, fuzzing harnesses) + `02-17` WASM runtime (interpreter→JIT transition: the performance jump, cache discipline) + `24-82` kernel gdb stub (attach a debugger to a machine via serial — the same trick for your cores) + `24-30` sampling profiler (measure your emulator where it burns).
**Gate artifact:** `labs/e6.md` — your PS1 core profiled (where cycles go), one bug found via your own trace/gdb tooling, written up as a debugger's case. → unlocks E7.

## E7 — capstone: the machine you own
Prereq: E1–E6. **Close all notes.** Rebuild-or-refinish your PS1 core to: boots its demo/game, passes your chosen test suites, and runs at ≥ the frame rate on your hardware. Add one of: JIT (a page of your interpreter compiled), save-state, or a gdb-style debugger view (registers/watches/breakpoints) on your core.
**Gate artifact:** `labs/e7.md` — the machine + benchmark + one extension + the "what I'd do different" writeup. Pass = you boot it with the tutorial closed.

## Rest-week fun builds (no gate, no pressure)
`24-117` Game Boy core (LR35902 + your 6502-adjacent instincts) · `29-06` Atari 2600 core (6507 = 6502 delta, TIA) · `02-16` shaders for your GPU pipelines · `10-16` quantization if your 3D dives into fast-math.

---

## Rules
1. Same as the other path docs: no gate skip; runnable artifacts; own machine; re-derive (test suites as oracle, not solutions).
2. Timeline (not packed, ~8–10 h/wk): E1–E2 ≈ months 2–5, E3–E4 ≈ months 5–9 (PS1 is the big lift), E5–E6 ≈ months 8–11, E7 ≈ month 12.
3. Realistic bar: a PS1 emulator is a 10k–50k-line project — the honest target is a *working subset* (boot + ≥1 game + tests pass + frame-rate target), not every game. That subset is the professional proof.
4. If performance frustrates: the discipline is 24-30 profiling, not guessing — the profiler gate (E6) exists to force the skill before the capstone.

## Where this lives
`steps/` unchanged (route: 02-01..17, 24-10/81/82, 06-06, 24-117, 29-06). Pairs the desktop lane's own-OS (24-01) — the two paths converge at "you wrote the CPU and the OS that runs on it".