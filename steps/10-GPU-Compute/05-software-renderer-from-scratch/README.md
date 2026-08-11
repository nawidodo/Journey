# 10-05 · Software Renderer From Scratch

**Week:** W17–19 · **Track:** C · **Prev:** [`../04-memory-coalescing-shared-memory`](../04-memory-coalescing-shared-memory/README.md) · **Next:** [`../06-metal-graphics-deep-dive`](../06-metal-graphics-deep-dive/README.md)

## Objective
The renderer the GPU hides from you. Pure-C rasterizer — no Metal, no GPU. This is why the pipeline steps in 10-06 will make sense.

## Tasks
- [ ] Line + triangle rasterization: edge function / barycentric coverage
- [ ] Depth buffer; painter's algorithm; z-fighting
- [ ] Transforms: rotate/scale/translate, perspective projection, camera view matrix
- [ ] Shading: flat → Gouraud → Phong
- [ ] Textures: UV mapping, nearest vs bilinear, perspective-correct interpolation
- [ ] Framebuffer → PPM/PNG; render a textured OBJ model
- [ ] SIMD-accelerate the inner raster loop (link to 10-02)
- [ ] Map each piece to the GPU pipeline (this feeds 10-06)

## Resources
- tinyrenderer (ssloy) — the canonical walkthrough
- Pikuma — [3D Computer Graphics Programming](https://pikuma.com/courses) (45h, this exact step: software rasterizer in C); [Raycasting Engine Programming with C](https://pikuma.com/courses) as optional warmup
- Scratchapixel; Giesen "A Trip Through the Graphics Pipeline" (rasterizer article here)

## Exit Criteria
- [ ] Textured OBJ rendered by your own rasterizer — `code/` + `labs/`
- [ ] Diagram: software vs GPU pipeline stages — `notes/`