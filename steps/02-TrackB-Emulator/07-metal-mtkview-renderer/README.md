# 02-07 · Metal — MTKView Renderer 🚩 M3

**Week:** W12–16 · **Track:** B · **Prev:** [`../06-macos-swiftui-shell`](../06-macos-swiftui-shell/README.md) · **Next:** [`../08-ios-app-on-device`](../08-ios-app-on-device/README.md)

## Objective
GPU render the NES framebuffer via Metal. **Milestone M3: NES via Metal on macOS.**

## Tasks
- [ ] MTKView setup; device/queue/library/pipeline state
- [ ] Upload NES 256×240 framebuffer as texture (MTLTexture, BGRA)
- [ ] Fullscreen quad vertex/fragment shaders; aspect-correct scaling (8:7)
- [ ] Present vsync-synced (CAMetalLayer vsync, `draw(in:)`)
- [ ] Optional: nearest-neighbor/scanline filters in shader

## Resources
- Kodeco *Metal by Tutorials*
- WWDC Metal sessions (2017–2019; MTKView, textures)
- Apple Metal documentation

## Exit Criteria
- [ ] **M3: NES frame rendered via Metal on macOS** — `code/`
