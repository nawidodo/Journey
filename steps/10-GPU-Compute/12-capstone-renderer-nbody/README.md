# 10-12 · Capstone: N-Body or Renderer 🚩 M11

**Week:** W27–28 · **Track:** C · **Prev:** [`../11-cross-api-vulkan-moltenvk-dx12`](../11-cross-api-vulkan-moltenvk-dx12/README.md)

## Objective
Everything together. Pick the path you enjoyed:
- **(a) N-body simulation** — compute kernel, threadgroup shared memory, textures, async, profiling
- **(b) Renderer** — your software rasterizer (10-05) → Metal (10-06) → Vulkan (10-11); SIMD path as baseline

## Tasks
- [ ] (a) Naive O(N²) N-body in MSL (gravity, softening) — correctness vs CPU baseline
- [ ] (a) Tiled pairwise pass with threadgroup memory; render positions via compute → vertex buffer (fence-synced)
- [ ] (a) Benchmark naive vs tiled, N = 2^k for k = 8..16; hit target (e.g. 16k bodies ≥60fps)
- [ ] (b) Textured scene: software rasterizer correctness baseline → Metal port → Vulkan port
- [ ] (b) Compare the three implementations (correctness + perf); SIMD software path as reference
- [ ] Either way: profile with HUD; write the bug → optimization breakdown

## Resources
- Kirk/Hwu N-body chapter; Apple "Metal N-Body" sample; your 10-05/06/11 outputs

## Exit Criteria
- [ ] **M11:** chosen capstone hits target (N-body FPS or 3-way renderer) — `labs/`
- [ ] Writeup — `notes/`
