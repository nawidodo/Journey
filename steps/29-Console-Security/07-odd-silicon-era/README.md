# 29-07 · The odd-silicon era — PS2/PS3, Saturn, Dreamcast, Jaguar, Vita (Phase 29)

**Week:** W41+ opt (niche) · **Track:** U · **Prev:** [`../06-console-archive-long-tail`](../06-console-archive-long-tail/README.md) · **Next:** none (last) · **Pairs:** 02-14/15, 24-110, 24-111, 05-01

## Objective
The long-tail archive (29-06) skipped the five consoles that bent the rules of CPU design — the era where "one CPU" was a suggestion: **PlayStation 2** (Emotion Engine: MIPS R5900 + 2 vector units + the Graphics Synthesizer), **PlayStation 3** (Cell: 1 PPE + 7 SPE SIMD engines + the LV0/LV1 hypervisor split — the 04-07/29-04 HV lesson before Xbox), **Sega Saturn** (2× SH-2 + 2 VDPs + 68K sound — the dual-CPU cache/arbitration nightmare turbocharged by 20-13 cache thinking), **Dreamcast** (SH-4 + a Windows CE runtime — a console that booted a desktop OS's kernel), **Atari Jaguar** (4 processors, 64-bit marketing with 16-bit reality), **PS Vita** (ARM Cortex-A9 + GPU — the PSP 02-15 lineage grown up). The deliverable: an architecture-diff sheet of these against the ISAs you already speak (6502/68k/ARM/RISC-V/x86 — 24-110), one re-created classic (the Cell SPE "register-file + DMA-tile" programming model as a Metal/CUDA kernel — 24-111/10 discipline), and the writeup on why the industry converged on few-core+GPU anyway (the 02-14 lesson re-read).

## Tasks
- [ ] Diff sheet: for each odd console — CPU(s), vector/SIMD units, co-processor choreography, memory model — vs your known-ISA table — `labs/`
- [ ] Cell re-creation: an SPE-style kernel (explicit DMA, small local store, SIMD lanes) written as a Metal compute kernel (24-111) — the same programming model, one abstraction up — `labs/` + `code/`
- [ ] Hypervisor-diagram rebuild: PS3 LV0/LV1 vs Xbox HV (29-04) vs Switch TZ (29-03) — three generations of "two privilege domains," one principle — `notes/`
- [ ] Writeup: what the odd-silicon era taught (parallelism is hard, but the discipline — choreographing copies — still governs GPUs/ioremap today) — `notes/`

## Resources
- Public PS2/PS3/Saturn/Dreamcast/Jaguar RE writeups (the console research canon), SDK/format docs; your 24-110/24-111/02-15 notes

## Exit Criteria
- [ ] Diff sheet + SPE-style kernel lab — `labs/` + `code/`
- [ ] LV-privilege-domain comparison writeup — `notes/`

## Links
- [PS3 LV0/CFW research (public writeups)](https://www.psdevwiki.com/ps3/)
- [Saturn hardware docs](https://github.com/ffmpegc/wasm/blob/main/docs/SSPP-documentation.pdf) · [Cell Programming tutorial](https://www.ibm.com/developerworks/library/pa-cellpp/)