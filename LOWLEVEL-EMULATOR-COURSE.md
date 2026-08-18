# Low-Level Emulator Course — Absolute-Beginner (bare-metal hello → your own PS1 core → the PS5 reality, gated)

Zero low-level knowledge assumed. You need: a Mac/Linux host + QEMU/UTM (a VM you can crash freely). Each unit: concept → do → runnable verification → **lesson check (own words, `notes/bN-quiz.md`)**. No advance without both. ~2–3h/unit, 12 units + capstone ≈ 6–8 weeks. This is the zero-knowledge floor for [`EMULATOR-VM-PATH.md`](EMULATOR-VM-PATH.md) (it takes over at E3) — and the path to the low-level truth already in your repo: **an emulator is a machine you re-assembled with your own hands.**

Compass (re-read when lost): low-level = **the machine has no magic — CPU reads numbers, executes them, touches memory, jumps when told.** Emulation = *you* write a program that plays the machine: fetch instruction → decode → act on fake registers/memory → repeat millions of times. Every unit removes one layer of magic until "how does a console work" has no answer left except "the way you wrote it."

Safety: QEMU/UTM VMs for OS-less experiments; console firmware stays on YOUR machines for the read/research units; no firmware dumping that violates your console's terms (PS5 = study ceiling, see B11); re-create, don't copy.

---

## B0 — hello world with NO operating system
Concept: before OSes, code just boots: the CPU runs bytes starting at a fixed address. Do: write a 512-byte boot sector (NASM/assembly) printing "HELLO" to the VM's display (VGA text mode, `0xb8000`), boot it in QEMU (`qemu-system-x86_64 -fda boot.img`); corrupt it → watch it fail; restore.
Verify: your own boot sector prints HELLO in the VM.
**Lesson check:** what happens between "power on" and "first instruction" — who loads that byte, and from where?

## B1 — the CPU: registers and the fetch-decode-execute loop
Concept: a CPU is a state machine: registers, the PC, memory reads, arithmetic, jumps. Do: in assembly on your VM (or the boot sector): read a value into a register, add, store back to memory; use a label + jump; watch PC change in the debugger (`qemu`'s monitor or gdb).
Verify: your program computes and stores a value; you traced the PC stepping.
**Lesson check:** name the three stages of instruction execution and what "the PC" is for.

## B2 — memory, endianness, and the stack
Concept: memory = numbered bytes; order of bytes matters (little-endian on your VM); the stack is just memory the CPU uses with push/pop. Do: write to specific addresses, read back with different widths (byte/word); print the byte order of a multi-byte value (endianness seen live); push/pop sequence traced in the debugger.
Verify: you show a multi-byte value's bytes reversed (little-endian) and a stack push/pop in your trace.
**Lesson check:** what does little-endian mean, and why does the stack grow "down"?

## B3 — interrupts and the OS seam
Concept: the CPU can be interrupted; the OS hooks those to take back control (syscalls, timers, keyboard). Do: in your VM, install a keyboard/timer interrupt handler that flips a screen color per tick — your boot code now responds to hardware without polling; then read how that same seam became "kernel mode" (the xv6/01-xv6 trap lecture, 30 min).
Verify: timer/keyboard interrupt visibly changes your VM's screen state.
**Lesson check:** polling vs interrupt — say the difference and why the OS world runs on the latter.

## B4 — your first virtual machine: CHIP-8
Concept: a fake CPU you define — the emulator idea in miniature. Do: complete `02-01` chip8: your interpreter fetches CHIP-8's 35-ish opcodes, holds its registers, runs its games at its clock (60 Hz); test ROMs pass.
Verify: 3 test ROMs run (IBM logo draws).
**Lesson check:** in your words, what does an interpreter do that a "real" CPU does at the same three stages (B1)?

## B5 — the first real ISA: 6502
Concept: a real 8-bit CPU: 256-byte zero page, 16-bit PC, the addressing-mode zoo. Do: complete `02-02`: 6502 core, opcode decode, addressing modes, flag math; your core passes the standard 6502 test suite (the oracle — test suites are how low-level people verify machines).
Verify: 6502 functional test suite passes on your core.
**Lesson check:** what is an addressing mode, and why does one instruction "really" need 10 of them?

## B6 — a console appears: NES = CPU + PPU + APU
Concept: a game machine = CPU (logic) + PPU (pixels) + APU (sound) + carts (the weird part). Do: complete `02-03` PPU (scanlines, palettes, sprites) → `02-04` APU (square/triangle/noise channels) → `02-05` cart mappers — then boot a real game ROM on the whole thing (own ROM, legal homebrew or owned cartridge dump).
Verify: a commercial-class game boots and plays on your system.
**Lesson check:** why is the PPU "the other computer" — what runs it, and how do CPU and PPU cooperate per frame?

## B7 — a second ISA on purpose: RISC-V little
Concept: one ISA learned (6502) → another is a comparison exercise: RISC-V = clean, load-store, fixed-width. Do: complete `02-11`: RISC-V RV32I emulator; your CHIP-8/6502 instincts transfer — note the *diff* (no flags, load-store discipline, immediates) in `notes/b7.md`.
Verify: RV32I test binary passes on your emulator.
**Lesson check:** three ways RISC-V differs from 6502 — in your words, why do designers pick one over the other?

## B8 — the machine you're aiming at: PS1's MIPS R3000 + GTE
Concept: 1994's console = MIPS CPU + geometry engine (GTE) + the polygon idea. Do: complete `02-14` foundation stages: MIPS core (aligned loads, delay slots, coprocessor 0) → GTE matrix/vector math (fixed-point) → the "3D" really being triangles transformed by matrices. This is the hard, slow unit — the reward is the whole mental leap.
Verify: MIPS core runs R3000 test code; GTE transforms a cube; wireframe polygon appears.
**Lesson check:** what is a "delay slot", and why do fixed-point numbers matter when there's no FPU?

## B9 — pixels without a GPU: the software rasterizer
Concept: a 3D game frame = for each triangle → project → fill per-pixel. Do: from `10-05` software renderer: take your wireframe cube from B8, fill its triangles, add z-buffering, texture-map one face — all CPU (no Metal yet; the GPU course later accelerates exactly this).
Verify: filled, z-buffered, textured cube renders from your code.
**Lesson check:** rasterization pipeline — name the stages from triangle to lit pixels.

## B10 — the assembly: your own PS1 core, working subset
Prereq: B0–B9. Finish EMULATOR-VM-PATH **E4's** definition: complete the PS1 core — MIPS + GTE + rasterizer + timing + controller/CD read — to the *working subset* bar: boots a demo/homebrew, passes your test suites, holds frame rate.
Verify: your core boots to 3D gameplay on your machine (or documented partial with honest limits per E4).
**Lesson check:** the three hardest integration points you hit (probably timing, DMA, or dependency order) — written up as a debug case apiece.

## B11 — the PS5 reality: read, research, roadmap (study ceiling)
Concept: PS5 = Zen2 CPU + RDNA2 GPU + a real OS (Orbis) + encrypted firmware — emulating it needs vendor GPU documentation that doesn't exist publicly, plus months of team effort even then. Do (own console, read/research only): complete the `29-02` Orbis reading; research community PS5 project status and the *technical* blockers (no public RDNA2 ISA docs, no decrypted firmware keys, DRM); write your platform analysis: boot chain → hypervisor → GPU → what ANY emulator-writer would need next.
Verify: `labs/b11.md` — a research paper (1–2 pages): the blockers, the state of the art, and your honest roadmap from PS1-subset to PS5.
**Lesson check:** name three concrete missing pieces that make PS5 emulation "team-years away" — and which one would move the needle most.

## B12 — CAPSTONE: low-level, in your words
Prereq: B0–B11. **Close all notes.** Choose ONE proof: (a) PS1 core boots a second game/ demo you never ran before (goes one step past B10); or (b) a brand-new minimal core (e.g., Atari 2600 6507 subset from `29-06`'s smallness) written cold. Write `labs/lowlevel-capstone.md`: your arc bare-metal-hello → machine-builder → PS5-roadmap, the three lessons that surprised you most, and the roadmap you'd follow next (EMULATOR-VM-PATH E5–E7: RISC-V + hypervisor + JIT).
**Pass = capstone core runs without notes and the writeup reads like an engineer's, not a student's.**

---

## Rules
1. Verification + lesson check both true before next unit; quizzes in own words — the low-level discipline is honesty about what *you* understand.
2. Copying allowed only in B0/B3 boilerplate (boot-sector scaffold, IDT stub) — cores are written from concept; erase-and-retry once when stuck.
3. 2–3h/unit; stuck past that = previous unit's verification again.
4. QEMU/UTM for bare-metal; console dumps only your own; PS5 = study ceiling (B11 sets the honest boundary); re-create, don't copy.
5. Honest bar: PS5 emulation is a multi-year team project with vendor-doc blockers; this course's bar = YOU wrote a working 6502, RISC-V, and PS1-subset emitter, and you can write the PS5 blocker paper that a real emulator team would find correct — that's the floor for console/OS reverse-engineering work, proven cold at the capstone.