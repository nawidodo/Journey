# 10-04 · Memory: Coalescing + Threadgroup Memory

**Week:** W16–17 · **Track:** C · **Prev:** [`../03-metal-compute-basics`](../03-metal-compute-basics/README.md) · **Next:** [`../05-software-renderer-from-scratch`](../05-software-renderer-from-scratch/README.md)

## Objective
The performance heart of GPU programming: make memory access patterns that the hardware likes.

## Tasks
- [ ] Coalesced vs strided access; measure the difference (bandwidth test kernel)
- [ ] `threadgroup` memory: declare, `threadgroup_barrier`, fill pattern
- [ ] **Matrix transpose kernel**: naive (stride-storm) vs tiled w/ threadgroup memory
- [ ] Padding to avoid bank conflicts (threadgroup memory)
- [ ] Benchmark all three versions, record in `notes/`

## Resources
- Kirk/Hwu ch.6–7 (tiling, coalescing) — the canonical treatment
- NVIDIA CUDA C Programming Guide (coalescing sections; hardware = your M-series GPU, concepts same)

## Exit Criteria
- [ ] Tiled transpose ≥3× faster than naive — `labs/`
- [ ] Bandwidth numbers recorded (GB/s vs theoretical peak) — `notes/`

## Links
- [GPU Gems 3 ch.39 (coalescing)](https://developer.nvidia.com/gpugems/gpugems3/gpugems3_ch39.html)
- [WWDC: Optimizing Metal performance](https://developer.apple.com/videos/play/wwdc2022/10161/)
