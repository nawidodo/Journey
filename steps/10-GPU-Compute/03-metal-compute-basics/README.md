# 10-03 · Metal Compute Basics

**Week:** W15–16 · **Track:** C · **Prev:** [`../02-simd-cpu-parallelism`](../02-simd-cpu-parallelism/README.md) · **Next:** [`../04-memory-coalescing-shared-memory`](../04-memory-coalescing-shared-memory/README.md)

## Objective
Your first compute kernels: Metal Shading Language (MSL), buffers, command queue/encoder, dispatch.

## Tasks
- [ ] MSL kernel: `[[thread_position_in_grid]]`, threadgroup size, grid
- [ ] `MTLCommandQueue` → `MTLCommandBuffer` → `MTLComputeCommandEncoder` → commit → wait
- [ ] `MTLBuffer` allocation + copy; `setBytes` vs `setBuffer`
- [ ] Grid-stride loop (safe for threadgroup-sizes that don't divide the workload)
- [ ] Kernel 1: elementwise `x*2`; Kernel 2: dot product with shared reduction

## Resources
- *Metal by Tutorials* (Kodeco) — compute chapter
- Apple docs: "Creating and Dispatching Compute Commands"; Metal Shading Language Spec

## Exit Criteria
- [ ] Dot-product kernel returns correct result vs CPU reference — `code/`
- [ ] Threadgroup-size sweep benchmark (speed vs size) in `notes/`
