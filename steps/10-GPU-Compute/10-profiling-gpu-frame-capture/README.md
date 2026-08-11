# 10-10 · Profiling: Xcode Metal HUD + Frame Capture

**Week:** W24–25 · **Track:** C · **Prev:** [`../09-async-queues-resource-tracking`](../09-async-queues-resource-tracking/README.md) · **Next:** [`../11-cross-api-vulkan-moltenvk-dx12`](../11-cross-api-vulkan-moltenvk-dx12/README.md)

## Objective
The skill that separates "writes kernels" from "fast kernels": measure first, optimize second, never guess.

## Tasks
- [ ] Enable Metal HUD (`MTLCreateSystemDefaultDevice` app + Xcode "View → Debug Area → Show Metal HUD"): FPS, CPU/GPU time
- [ ] Xcode GPU frame capture: per-drawcall timings, shader profiling, bottleneck coloring
- [ ] Profile your transpose kernel from step 04: is it memory-bound? (bandwidth ~peak = yes)
- [ ] Find one real stall in your async demo; fix it; prove it with before/after HUD numbers
- [ ] Occupancy check: does increasing threadgroup size actually help? (sweep)

## Resources
- Apple: "Debugging Metal Workloads" (Xcode docs), WWDC "Getting Metal Performance Right"
- Metal Performance HUD fields reference (Xcode)

## Exit Criteria
- [ ] Before/after frame times for one optimized pass — `labs/`
- [ ] You can name what the HUD numbers mean — `notes/`
