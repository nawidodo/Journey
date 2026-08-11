# 10-06 · Metal Graphics Pipeline (deep dive)

**Week:** W19–20 · **Track:** C · **Prev:** [`../05-software-renderer-from-scratch`](../05-software-renderer-from-scratch/README.md) · **Next:** [`../07-shader-programming-msl-glsl-hlsl`](../07-shader-programming-msl-glsl-hlsl/README.md)

## Objective
Revisit your Track B MTKView step with full understanding: render pipeline stages, why it's a pipeline, where compute fits.

## Tasks
- [ ] Vertex → rasterizer → fragment stages; which are programmable, which aren't
- [ ] `MTLRenderPipelineState`, render pass descriptors, `MTLClearColor`
- [ ] Vertex buffers + uniforms (UBO vs push); depth buffer, MSAA, backface culling
- [ ] Draw a 3D object (cube) with perspective + depth
- [ ] Same scene rendered with a compute-shader post-pass (e.g. grayscale/fake bloom)

## Resources
- *Metal by Tutorials* — render pipeline chapters (this *is* the book's core)
- Giesen "A Trip Through the Graphics Pipeline" — read the pipeline-stages article here

## Exit Criteria
- [ ] Depth-tested, MSAA 3D cube — `code/`
- [ ] Pipeline-stage diagram drawn by hand — `notes/`
