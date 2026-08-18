# 10-13 · GPU path tracer — Metal compute, BVH, materials, denoise (stretch)

**Week:** W28 stretch · **Track:** C · **Prev:** [`../12-capstone-renderer-nbody`](../12-capstone-renderer-nbody/README.md)

## Objective
The capstone's opposite: instead of rasterizing real-time, *simulate light*. Metal compute ray tracer — spheres → BVH → materials → denoise. Every Track C concept (coalescing, occupancy, shared memory, divergence) shows up as a correctness/perf cliff. The classic "fun" GPU project.

## Tasks
- [ ] Core: camera rays, sphere intersection, recursive-ish shading, accumulation over samples — CPU reference first (tiny, correct), then port to Metal compute kernel
- [ ] BVH: build (CPU or GPU) + traverse in-kernel; the divergence lesson — warp-coherent traversal, stack-based vs restructured
- [ ] Materials: diffuse/glossy/refractive, env lighting; one scene with depth-of-field + soft shadows (area light sampling)
- [ ] Denoise: aovs (albedo/normal) → simple denoiser or temporal accumulation; the "noise is variance, variance is samples" argument
- [ ] Self-check: scene renders, converges to reference (CPU path tracer diff < threshold); frame-time per-sample measured

## Resources
- PBRT (the reference); "Ray Tracing in One Weekend" (the minimal path); your 10-03/04/07 notes (dispatch, memory, divergence)

## Exit Criteria
- [ ] GPU path tracer renders a converged scene matching CPU reference — `labs/`
- [ ] BVH + ≥2 material types + denoise — `labs/` (one demo image + timing log)

## Links
- [Ray Tracing in One Weekend](https://raytracing.github.io/)
- [PBRT](https://www.pbrt.org/)
