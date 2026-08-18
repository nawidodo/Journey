# GPU Programming — Absolute-Beginner Course (Metal first, to emulator shaders)

Zero GPU knowledge assumed. You need: a Mac, Xcode (free, App Store), and the **C basics from 00-01** (that's the only prerequisite). Each unit ends with something you **run and see** — no unit passes until its verification line is true. ~1–2h per unit, 10 units ≈ 2–3 weeks. When done, jump into gate S3 of [`GPU-PATH.md`](GPU-PATH.md) — your emulator shader work is already half-done.

Mental model to carry (re-read this when stuck): **a GPU is a calculator with thousands of tiny cores.** The CPU gives each core a job by *index* (`you are thread 7, do row 7`); all cores run the SAME program (a *kernel*) on different data. That's why GPUs win at "do the same math on a huge array" — you pay latency once, then reap bandwidth. Everything below teaches one skill: **write the kernel, shape the data, measure the speed.**

---

## U0 — see the machine (no code)
Do: run `system_profiler SPDisplaysDataType` in Terminal — find your GPU name and VRAM.
Read: Apple's "Metal overview" page (10 min, the words: device, command queue, command buffer, buffer, pipeline, dispatch — the 6 words you'll use forever).
Verify: you can name GPU + VRAM, and say what a *command buffer* is in one sentence.

## U1 — first kernel: add two arrays (Metal compute)
Concept: kernel → pipeline → buffers → dispatch → read back. In Xcode: **New Project → macOS → Command Line Tool** (Language: Swift). Replace `main.swift` with the FULL program below, run (⌘R), see the output `[5.0, 5.0, …]`:

```swift
import Metal

let device = MTLCreateSystemDefaultDevice()!          // 1. the GPU
let src = """
#include <metal_stdlib>
using namespace metal;
kernel void add_arrays(const device float* a [[buffer(0)]],
                       const device float* b [[buffer(1)]],
                       device float* c [[buffer(2)]],
                       uint id [[thread_position_in_grid]]) {
    c[id] = a[id] + b[id];                            // 5. the kernel: every thread does this line
}
"""
let lib = try device.makeLibrary(source: src, options: nil)
let pipeline = try device.makeComputePipelineState(function: lib.makeFunction(name: "add_arrays")!)
let n = 16
let bufA = device.makeBuffer(bytes: [Float](repeating: 2, count: n), length: n*4, options: [])!   // 2. data in
let bufB = device.makeBuffer(bytes: [Float](repeating: 3, count: n), length: n*4, options: [])!
let bufC = device.makeBuffer(length: n*4, options: [])!                                        // 3. data out
let queue = device.makeCommandQueue()!                // 4. the conveyor belt
let cmd = queue.makeCommandBuffer()!
let enc = cmd.makeComputeCommandEncoder()!
enc.setComputePipelineState(pipeline)
enc.setBuffer(bufA, offset: 0, index: 0)
enc.setBuffer(bufB, offset: 0, index: 1)
enc.setBuffer(bufC, offset: 0, index: 2)
enc.dispatchThreads(MTLSize(width: n, height: 1, depth: 1),   // 6. launch all threads
                    threadsPerThreadgroup: MTLSize(width: 16, height: 1, depth: 1))
enc.endEncoding()
cmd.commit(); cmd.waitUntilCompleted()                // 7. GPU executes
let out = bufC.contents().assumingMemoryBound(to: Float.self)
print(Array(UnsafeBufferPointer(start: out, count: n)))
```

Read the numbered comments in order — that IS the whole Metal life cycle for the rest of the course.
Verify: output prints sixteen 5.0s. Change `n` to 1_000_000 — still one line of kernel code; the GPU just does it faster than your CPU could.

## U2 — the thread model (why it's fast)
Concept: grid → threadgroups → threads; `thread_position_in_grid` is the ID. The GPU schedules in groups of ~32-64 at once; your kernel doesn't know or care.
Do: change the kernel to `c[id] = a[id] * 10 + b[id]`, add a `device float* d` buffer that stores `id` as a float (`d[id] = float(id)`), print it and see thread IDs 0…n-1 come back in order.
Verify: you can explain: "I launched n threads; thread id x ran the same instruction on element x."

## U3 — memory 1: buffers and coalescing
Concept: GPU reads nearby addresses 32-at-a-time; *coalescing* = make adjacent threads touch adjacent memory. It's a 10×+ speed difference and the #1 perf rule.
Do: in the U1 program, make kernel `c[id] = a[id*2] + a[id*2+1]` (each thread touches two floats) then a version with a giant stride (`a[id*1000]`) — time both with `CFAbsoluteTimeGetCurrent()` around the dispatch loop (1000 iterations each).
Verify: table in `notes/u3.md` showing strided is slower. You now know the memory rule every GPU programmer swears by.

## U4 — memory 2: shared memory (the speedup trick)
Concept: threads in one threadgroup can share a fast scratchpad (`threadgroup` memory). Classic exercise: prefix sum (running total) — do it naively first (each thread sums the array alone), then with threadgroup shared memory.
Verify: both results equal; naive version gets slower as array grows; shared version holds. Write 3 lines in `notes/u4.md` on why.

## U5 — graphics: render a triangle (compute → screen)
Concept: compute is math, graphics is math drawn. Pipeline: vertex shader (position each corner) → rasterizer (fill shape) → fragment shader (color each pixel).
Do: return to the U1 project; add an `MTKView` (MetalKit) in a macOS App template this time (New Project → macOS → App, add `import MetalKit`). Draw one triangle: vertex shader with 3 hardcoded positions, fragment shader returning a solid color. Honestly copy a "MTKView triangle" sample once — this is the one unit where copying is allowed, read every line of the shader until the pipeline is yours.
Verify: a colored triangle on screen; you can name which shader decided *where* vs *what color*.

## U6 — textures: put an image through the GPU
Concept: an image = buffer of pixels; a *texture* = that buffer with format + sampler (how to read it: nearest/linear).
Do: load a PNG as `MTLTexture`, sample it in a fragment shader, draw it full-screen. Then add a one-line effect: `color.rgb = 1 - color.rgb` (invert) and run it.
Verify: image displays, then inverted. You now have "feed pixels in, change pixels, draw them" — the exact shape emulator shaders use.

## U7 — YOUR goal: emulator shader (scanlines/CRT)
Prereq: finish U1–U6 — and your NES core (steps 02-02→02-05) OR Game Boy core (24-117) must boot a ROM to a frame buffer (if not, do EMULATOR-VM-PATH E2 first).
Do: your emulator's PPU output is already an array of RGBA pixels — upload it as a texture (U6 code), draw full-screen, then add: **scanlines** (`if (pixel.y % 2 == 0) color *= 0.5;`), then **CRT curvature** (shift sampling by distance from screen center). You can bypass the NES-to-screen via a static PNG of a single emulator frame while you iterate.
Verify: screen shows your emulated game with scanlines that move with the game; you can toggle the shader on/off with one key. **This is the milestone you asked for.** Log it in `labs/s3.md`.

## U8 — measure it (profiling, not guessing)
Concept: U7 shader runs at "good enough" — now prove it. Run with Xcode's **Metal HUD** (Run → Diagnose → Metal) to get GPU time per frame: change the scanline math until HUD time drops; try `MTLSize(width: 4,...)` texture with nearest vs linear sampling.
Verify: HUD numbers in `notes/u8.md`, one real optimization with before/after HUD time.

## U9 — parallel compute on YOUR data (the LLM-forerunner)
Concept: everything you did to pixels, do to numbers: matrix multiply — the single most important GPU kernel (it IS neural-network inference).
Do: kernel `C[rows][cols] += A[rows][k] * B[k][cols]`, try it on 1024×1024 random matrices CPU-vs-GPU; then re-run with U4's shared-memory trick inside the kernel.
Verify: GPU wins with a real time table; shared version wins again. This kernel is gate S7's ML work pre-warmed.

## U10 — the map: where this sits in your year
You now own the Metal model (U1), memory rules (U3/U4), graphics (U5/U6), the emulator milestone (U7), measurement (U8), and the compute core (U9). Next: enter [`GPU-PATH.md`](GPU-PATH.md) at **S4** (S1–S3 skills are yours) → then S5 Vulkan (same GPU via MoltenVK, you'll feel the "why Metal abstracts" lesson personally) → S6 DX12 reading → S7 LLM inference → S8 capstone. The words from U0 — device, queue, buffer, pipeline, dispatch — now mean something you built.

---

## Rules
1. No unit skip: U-verification line must be true before the next unit.
2. Copying allowed ONLY in U5 (the triangle boilerplate) — everywhere else, write from the concept; erase-and-retry once when stuck (the 24-43 systems-gauntlet method).
3. Timebox each unit 2h; stuck past that = go back one unit and re-run its verification.
4. Own machine only; all examples are on your Mac's GPU via Metal.
5. If your emulator core isn't ready at U7: do the PNG-frame static version and return — don't stall the course on the core.