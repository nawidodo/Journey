# 10-11 · Cross-API: Vulkan (MoltenVK) + DX12

**Week:** W25–27 · **Track:** C · **Prev:** [`../10-profiling-gpu-frame-capture`](../10-profiling-gpu-frame-capture/README.md) · **Next:** [`../12-capstone-renderer-nbody`](../12-capstone-renderer-nbody/README.md)

## Objective
One renderer, three APIs. Vulkan is the cross-platform one (Android/Linux/Windows); macOS has no native Vulkan — MoltenVK translates it to Metal. DX12 is Windows/Xbox-only (stretch, on the Track D VM).

## Tasks
- [ ] Why three APIs: Metal (Apple), Vulkan (Khronos), DX12 (Microsoft); where each ships
- [ ] Vulkan via MoltenVK: instance → physical/logical device → swapchain → render pass → pipeline → command buffer
- [ ] Port your Metal cube/renderer to Vulkan; note explicit vs implicit synchronization
- [ ] Descriptor sets vs pipeline state objects vs render passes (Vulkan's model)
- [ ] DX12 stretch (Windows VM, W30+): command allocators, root signatures, descriptor heaps
- [ ] Comparison table: ownership, sync model, shader language, debugging tooling, deployment

## Resources
- vulkan-tutorial.com; MoltenVK docs; Microsoft DX12 docs
- Your 10-06 Metal renderer

## Exit Criteria
- [ ] Same scene in Metal + Vulkan; DX12 stretch on the Windows VM — `code/`
- [ ] 3-API comparison — `notes/`