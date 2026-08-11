# 10-02 · CPU SIMD: SSE / AVX / NEON

**Week:** W13–15 · **Track:** C · **Prev:** [`../01-gpu-architecture-concepts`](../01-gpu-architecture-concepts/README.md) · **Next:** [`../03-metal-compute-basics`](../03-metal-compute-basics/README.md)

## Objective
Data-level parallelism on the CPU — the twin of GPU SIMT, and the substrate of the software renderer later. Apple Silicon = NEON; the x86 VM (Track D) = AVX.

## Tasks
- [ ] SIMD model: one instruction, N lanes; SSE (128-bit) vs AVX/AVX2 (256-bit) vs NEON (128-bit); which ISAs your machines have
- [ ] Autovectorization first: write hot loops the compiler vectorizes; confirm in disassembly (godbolt)
- [ ] Intrinsics from scratch: load/store, add/mul, dot product, horizontal reduction, lane masking
- [ ] Data layouts: SoA vs AoS; 16/32-byte alignment; why gather/scatter kills SIMD
- [ ] Benchmarks: saxpy, dot product, 4×4 matrix multiply — scalar vs SSE vs AVX vs NEON
- [ ] Branchless: min/max/clamp via masks instead of branches
- [ ] When it hurts: misaligned, small workloads, control flow — know when scalar wins

## Resources
- Intel Intrinsics Guide; ARM NEON docs
- godbolt.org for autovectorization experiments
- "SIMD at Insomniac Games"; Agner Fog optimization guides

## Exit Criteria
- [ ] Benchmarks scalar vs SIMD + SoA rewrite, ≥2× on a real workload — `code/` + `notes/`

## Links
- [Apple Accelerate docs](https://developer.apple.com/documentation/accelerate)
- [Agner Fog optimization manuals](https://www.agner.org/optimize/)
