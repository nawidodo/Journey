# 24-111 · Own Metal compute — your neural net on your GPU, the macOS lane (stretch)

**Week:** W46+ opt (stretch) · **Track:** P · **Prev:** [`../110-own-arm64-playground`](../110-own-arm64-playground/README.md) · **Next:** [`../112-own-game-cheat-tool`](../112-own-game-cheat-tool/README.md) · **Pairs:** 10-15, 10-16, 20-08, 24-63

## Objective
Phase 10 runs CUDA on a Colab T4; your Mac's GPU is Metal. Port your 10-15 neural net to a Metal compute pipeline — the platform-priority complement: device/command-queue setup, compute kernel (the shader language, threads/grid — 10-15 layer math as a `kernel void`), buffer management (MTLBuffer, the 24-16 allocator thinking scaled to VRAM), and the honest benchmark: forward pass on CPU vs your Metal kernel vs CUDA-notes from 10-15 — the per-platform table (24-30 measurement). Extend: quantized weights (10-16) in Metal — why Apple GPUs like fp16/bf16 (the hardware-aware quantization lesson). Works 100% on your own Mac, no Colab.

## Tasks
- [ ] Setup: MTLDevice/CommandQueue/CommandBuffer encode/dispatch pattern (the GPU command flow — pairs 24-48 pipelines)
- [ ] Kernel: matmul + activation as compute shaders, threadgroup tuning (the occupancy/threads math — pairs 10-13)
- [ ] Data: buffers for weights/activations, the copy discipline (CPU↔GPU — where the time goes, 24-30)
- [ ] Benchmark: MNIST forward pass CPU vs Metal vs (25-08 notes) — the table + one chart — `labs/`
- [ ] Quantize: 10-16 int8/fp16 weights in Metal — accuracy-vs-speed table on your GPU — `labs/`
- [ ] Writeup: Metal vs CUDA mental model (explicit command buffers vs streams), why macOS GPU matters for on-device ML (CoreML's hardware) — `notes/`

## Resources
- Metal Programming Guide + shader language spec (the manual); tinygrad's Metal backend (peer — the portable pattern); your 10-15/10-16 code

## Exit Criteria
- [ ] Net runs on own GPU with measured speedup table — `labs/` + `code/`
- [ ] Metal-vs-CUDA + quantization writeup — `notes/`

## Links
- [Metal Programming Guide](https://developer.apple.com/metal/Metal-Programming-Guide.pdf)
- [tinygrad OpenAI/Metal backends](https://github.com/tinygrad/tinygrad)