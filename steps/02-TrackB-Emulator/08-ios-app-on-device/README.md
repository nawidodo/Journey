# 02-08 · iOS App — NES on Device 🚩 M4

**Week:** W16–18 · **Track:** B · **Prev:** [`../07-metal-mtkview-renderer`](../07-metal-mtkview-renderer/README.md) · **Next:** [`../../05-Linux-Kernel-Exploitation/01-modprobe-path`](../../05-Linux-Kernel-Exploitation/01-modprobe-path/README.md)

## Objective
Port to iPhone and run on hardware. **Milestone M4: NES on iPhone.**

## Tasks
- [ ] Port the Metal renderer to an iOS MTKView target
- [ ] Touch input → controller mapping (on-screen pad)
- [ ] Free provisioning: sign with personal team, run on device
- [ ] (Alternative for later: jailbroken device workflow)
- [ ] Perf pass: keep 60 fps interpreter-only

## Caveat
iOS bans JIT → the interpreter-only design is correct and sufficient (this is why CHIP-8/NES work but future JIT systems won't).

## Resources
- Apple *iOS App Programming Guide* (deployment)
- Free provisioning docs (Apple Developer)
- Kodeco *Metal by Tutorials* (iOS chapters)
- Pikuma — [PS1 Programming with MIPS Assembly & C](https://pikuma.com/courses) (post-NES "complex emulator" stretch: learn the console to emulate it — its GPU is a primitive rasterizer you can implement with Track C `10-05` software-renderer skills)

## Exit Criteria
- [ ] **M4: NES runs on iPhone at full speed**
