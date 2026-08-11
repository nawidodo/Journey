# 10-01 · GPU Architecture Concepts

**Week:** W12–13 · **Track:** C · **Prev:** — · **Next:** [`../02-simd-cpu-parallelism`](../02-simd-cpu-parallelism/README.md)

## Objective
Know *why* GPUs are fast before writing any kernel: SIMT, warps/wavefronts, occupancy, memory hierarchy, the real bottleneck (bandwidth, not FLOPs).

## Tasks
- [ ] SIMT model: many threads, lockstep groups, divergence penalty
- [ ] Warps (CUDA) ≈ wavefronts (AMD) ≈ threadgroups (Metal); occupancy math
- [ ] Memory hierarchy: registers → shared/threadgroup → L1/L2 → VRAM; latency vs bandwidth
- [ ] Coalescing concept; compute-bound vs memory-bound kernels
- [ ] Why NES-scale CPUs don't fit GPUs, but matrix ops do

## Resources
- *Programming Massively Parallel Processors* (Kirk/Hwu) ch.1–5 — canonical concepts book, CUDA-flavored, concepts transfer 1:1
- "A Trip Through the Graphics Pipeline" (Fabian Giesen) — free online
- Apple: "Understanding GPU Memory" / WWDC Metal sessions

## Exit Criteria
- [ ] Explain warp divergence + occupancy from memory — `notes/`
- [ ] Given a kernel, classify it compute-bound vs memory-bound — `labs/`
