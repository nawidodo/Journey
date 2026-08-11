# 10-08 · Textures, Samplers, Mipmaps

**Week:** W22–23 · **Track:** C · **Prev:** [`../06-metal-graphics-deep-dive`](../06-metal-graphics-deep-dive/README.md) · **Next:** [`../09-async-queues-resource-tracking`](../09-async-queues-resource-tracking/README.md)

## Objective
Texture memory (the GPU's other fast path) + sampling: filter modes, mipmaps, why textures are not plain buffers.

## Tasks
- [ ] `MTLTexture` creation, upload (blit), sampling in shader
- [ ] Filter modes: nearest/linear, mipmap generation + `mipmapFilter`
- [ ] Address modes (repeat/clamp/mirror); anisotropic filtering
- [ ] Why texture memory has its own cache + swizzled layout (vs linear buffers)
- [ ] GPU-side image filter chain: blur → sharpen → posterize (compute, using textures)

## Resources
- *Metal by Tutorials* — texture chapters
- Kirk/Hwu ch. on texture memory (GPU-side caching model)

## Exit Criteria
- [ ] Texture-sampled quad with mipmaps + anisotropic — `code/`
- [ ] Filter chain runs on GPU, before/after images in `notes/`
