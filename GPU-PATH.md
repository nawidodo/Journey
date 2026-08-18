# GPU Programming Path — Metal → Vulkan → DX12, parallel compute → shaders → LLM inference

Absolute beginner? Start with the hands-on course first: [`GPU-BEGINNER-COURSE.md`](GPU-BEGINNER-COURSE.md) (U0–U10, each unit runnable, lands with a shader inside your emulator). This path takes over from gate S4.

Goal: own GPU programming on every major API, ordered easiest-first (Metal on your Mac → Vulkan via MoltenVK on the same GPU → DX12 as reading), applied to three real goals: **parallel processing, emulator shaders, LLM compute**. Same contract: gate unlocks next only when runnable artifact exists; no skip; own hardware.

Prereq: **G0 partially** — C (00-01) + one graphics-facing build (02-05 NES or 24-117 Game Boy recomended; the EMULATOR-VM lane E1–E2 is the ideal feeder). xv6-level OS knowledge not needed.

---

## S1 — what a GPU actually is
Finish: `10-01` GPU architecture (SM/warp, memory hierarchy, the scheduling model) → `10-02` CPU SIMD (SSE/AVX/NEON — the same wide-thinking, CPU-grade) → `10-05` software renderer from scratch (you rasterize without a GPU — the baseline your GPU work will beat).
**Gate artifact:** `labs/s1.md` — a software triangle/framebuffer you wrote + SIMD vector add benchmark vs scalar (numbers in `code/`+`notes/`). → unlocks S2.

## S2 — Metal compute (easiest-first choice, it's native on your Mac)
Finish: `10-03` Metal compute basics (buffers, grid/threadgroup, first kernel) → `10-04` memory coalescing + threadgroup memory (the perf-shaped lesson) → `24-111` Metal compute (your own full compute project).
**Gate artifact:** `labs/s2.md` — a real parallel kernel of yours (filter/prefix-sum/add) run on your GPU with a CPU-vs-GPU benchmark table. → unlocks S3.

## S3 — graphics pipeline + shaders (the emulator-shader goal)
Finish: `10-06` Metal graphics pipeline deep dive → `10-07` shader programming MSL+GLSL+HLSL (one language, three spelling) → `10-08` textures/samplers/mipmaps → **`02-16` emulator shaders now**: apply GPU to your NES/PS1 core (scanline/CRT, upscaling, blur) —
**Gate artifact:** `labs/s3.md` — your emulator core's PPU/GPU output passes through a shader you wrote (before/after frames + why-it-looks-right writeup). → unlocks S4.

## S4 — concurrency & measurement discipline
Finish: `10-09` async/queues/resource-tracking → `10-10` profiling (Xcode Metal HUD + frame capture — the profiler habit from the EMULATOR lane) → `24-30` sampling profiler (own tool, CPU-side of the same coins).
**Gate artifact:** `labs/s4.md` — your S3 shader profiled: the frame budget breakdown, one bottleneck fixed with evidence. → unlocks S5.

## S5 — Vulkan (cross-API, same GPU via MoltenVK)
Finish: `10-11` cross-API: Vulkan (MoltenVK) + DX12 — the explicit-API model, its cost, its control. Implement one Vulkan compute pass on your Mac (MoltenVK) and port your S2 kernel.
**Gate artifact:** `labs/s5.md` — S2 kernel running under Vulkan-MoltenVK on same GPU + a Metal-vs-Vulkan ergonomics/perf comparison table. → unlocks S6.

## S6 — DX12 (reading level — no Windows GPU on this machine)
Finish: `10-11` DX12 half (already in cross-API read) + studying `11-02` Windows internals only far enough to place DX12 (WDDM, command lists vs queues). Honest ceiling: DX12 hands-on needs a Windows box with a GPU — stays a **study-note ceiling row** until then (same rule as CUDA T4).
**Gate artifact:** `labs/s6.md` — writeup: how a kernel you wrote would map onto DX12 D3D12 (resource barriers, root signatures) — code-read, not run. → unlocks S7.

## S7 — LLM/ML compute on your GPU
Finish: `10-15` own neural network (MNIST — the model) → `10-16` own model quantization (int8/fp16 — the memory-bandwidth lesson) → `10-14` CUDA AI API (read-level: CUDA is the `10-14` T4 cloud ceiling; your compute work is Metal — the kernels transfer conceptually, mark `10-14` as study note). Optionally `24-92` FFT visualizer (data-parallel rep) + `20-08` GPU password cracker (the attack-side of the same hardware).
**Gate artifact:** `labs/s7.md` — MNIST trained or inferred on GPU kernels YOU wrote (10-15 rewritten in Metal, int8 via 10-16), CPU-vs-GPU-vs-quantized benchmark table. → unlocks S8.

## S8 — capstone: close-notes GPU project (choose one)
Prereq: S1–S7. Pick (a) **renderer**: `10-13` path tracer (BVH + GPU) or extend your PS1 core's GPU to shader-assisted rendering; (b) **ML**: your Metal int8 inference engine serving a real model; (c) **parallel**: 24-92-flavored real-time FFT/audio GPU tool. Tutorials closed; one extension of your own; benchmark vs your S1 baseline.
**Gate artifact:** `labs/s8.md` — project runs, benchmark + frame/throughput evidence, one "what I'd ship next" writeup. Pass = built without opening a tutorial.

## Rest-week fun builds: `02-16` shader variations on your emulator cores · `10-12` nbody capstone (the classic parallel demo) · `29-06` GPU assist for Atari 2600 TIA scaling · `24-117` GB core + GPU scanline shader.

---

## Rules
1. Same no-skip artifact contract; own Mac = Metal + MoltenVK Vulkan; DX12 + CUDA-T4 stay reading ceilings (no GPU passthrough hardware here).
2. Timeline (not packed, 8–10 h/wk): S1–S2 ≈ months 2–4 (pairs EMULATOR E3/E4 window), S3–S4 ≈ months 4–7 (emulator shaders right when your PS1 core exists), S5 ≈ month 7–8, S6 ≈ month 8–9 (reading), S7 ≈ months 9–11, S8 ≈ month 12.
3. The "easiest first" order is deliberate: Metal's concise API builds the model; Vulkan's explicitness then teaches *why* Metal abstracts; DX12 reading mounts on both. No API jumps cold.
4. Realistic bar: a benchmarked kernel + one shipped project + honest read-level ceiling — the professional GPU-programmer proof.

## Where this lives
`steps/` unchanged (route: 10-01..16, 24-111, 02-16, 24-30/92, 20-08). Feeds back into the other lanes: EMULATOR shaders (S3), LLM compute meets 12-07/24-79 detection ML, path tracer = fun-year build.