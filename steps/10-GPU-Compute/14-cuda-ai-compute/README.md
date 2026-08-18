# 10-14 · CUDA for AI compute — the era API, from a Metal base (stretch)

**Week:** W28+ stretch · **Track:** C · **Prev:** [`../13-path-tracer`](../13-path-tracer/README.md)

## Objective
Every serious AI/ML stack runs CUDA. Track C gave you Metal + Vulkan — same hardware model, different names. CUDA is the missing API: threads/blocks/grid → memory model → atomics/reduction → a PyTorch custom kernel (the AI hook). No NVIDIA hardware locally: free Colab T4 or rented GPU.

## Tasks
- [ ] CUDA C: kernel launches, thread/block/grid, grid-stride loop; map each concept onto your 10-03 Metal kernel (the cross-mapping is the lesson — SIMT is SIMT)
- [ ] Memory model: global/shared/constant/registers, coalescing, bank conflicts — re-run your 10-04 tiled transpose in CUDA, compare notes vs Metal threadgroup
- [ ] Atomics + reduction: the classic; then warp-shuffle reduction (the CUDA-specific trick)
- [ ] AI hook: PyTorch custom CUDA kernel (extension) — a fused op (e.g., GELU + bias add, or a softmax) called from a real training loop; profile with ncu (Nsight Compute)
- [ ] Self-check: same kernel in Metal + CUDA, perf comparison table (T4 vs Apple Silicon); custom kernel matches PyTorch reference output

## Resources
- NVIDIA CUDA C++ Programming Guide; CUDA samples; PyTorch extension docs; your 10-03/04 notes (the carry-over)

## Exit Criteria
- [ ] CUDA kernels (transpose + reduction + warp-shuffle) working on Colab T4 — `labs/`
- [ ] PyTorch custom kernel in a training loop, ncu profile + Metal/CUDA comparison table — `labs/` + `notes/`

## Links
- [CUDA C++ guide](https://docs.nvidia.com/cuda/cuda-c-programming-guide/)
- [PyTorch custom ops](https://pytorch.org/tutorials/advanced/cpp_extension.html)
