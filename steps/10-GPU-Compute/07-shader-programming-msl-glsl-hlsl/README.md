# 10-07 · Shader Programming: MSL + GLSL + HLSL

**Week:** W20–22 · **Track:** C · **Prev:** [`../06-metal-graphics-deep-dive`](../06-metal-graphics-deep-dive/README.md) · **Next:** [`../08-textures-sampling-mipmaps`](../08-textures-sampling-mipmaps/README.md)

## Objective
Shaders are kernels with a fixed I/O contract. Write the same effect in all three languages — the transferable skill.

## Tasks
- [ ] Stages: vertex → rasterizer → fragment; tessellation/geometry; compute; what each stage receives/returns
- [ ] MSL (via Metal): vertex/fragment functions, buffers, `thread_position_in_grid`
- [ ] GLSL: same effect on desktop GL (or WebGL); `in`/`out`, `gl_Position`, uniforms
- [ ] HLSL: same effect in DX12 on the Windows VM (Track D) — or read + syntax-map
- [ ] One post-effect (edge-detect / bloom / grayscale) in all three
- [ ] Compute shader that reads/writes a texture (GPGPU inside the graphics API)
- [ ] Shader perf: occupancy, register pressure, divergence, texture access patterns

## Resources
- Per-API docs; Shadertoy for practice; your 10-06 cube + 10-05 rasterizer as baselines

## Exit Criteria
- [ ] Same effect in MSL + GLSL + HLSL — `code/`
- [ ] Perf notes: what moves occupancy in each — `notes/`