# 10-09 · Async, Queues, Resource Tracking

**Week:** W23–24 · **Track:** C · **Prev:** [`../08-textures-sampling-mipmaps`](../08-textures-sampling-mipmaps/README.md) · **Next:** [`../10-profiling-gpu-frame-capture`](../10-profiling-gpu-frame-capture/README.md)

## Objective
GPU is a separate async engine: CPU encodes, GPU executes later. Understand the command-buffer lifecycle and avoid the classic stalls.

## Tasks
- [ ] Commit-without-wait pattern; `waitUntilCompleted` vs completion handlers
- [ ] Multiple command buffers in flight; why GPU idle time kills FPS
- [ ] `MTLFence`/`hazardTrackingMode`: sync compute→render (a compute pass feeding a render pass)
- [ ] Resource tracking: when can a buffer be reused? (modified vs read-only)
- [ ] Double/triple buffering with a spinning animation

## Resources
- Apple docs: "Synchronizing CPU and GPU Work"; "Managing Resources"
- WWDC: "Metal: Efficiency and Optimization" / "Working with Metal"

## Exit Criteria
- [ ] Animation with 3 command buffers in flight, no stalls (CPU ≤2ms ahead) — `code/`
- [ ] Timeline diagram CPU/GPU overlap — `notes/`

## Links
- [Metal command queues docs](https://developer.apple.com/documentation/metal/command_queues)
- [WWDC Metal sessions](https://developer.apple.com/videos/all-videos/?q=metal)
