# 24-55 · Own 3D model loader — OBJ/glTF into your renderer (stretch)

**Week:** W46+ opt (stretch) · **Track:** P · **Prev:** [`../54-own-video-container`](../54-own-video-container/README.md)

## Objective
Your 10-05 renderer and 10-13 path tracer have been drawing primitives — feed them real geometry. Build a 3D model loader: OBJ (the simple format) then glTF (the era format — JSON + binary buffers + accessors), loading positions/normals/UVs, scene graph, and materials into your own renderer. The RE discipline: binary accessors with offsets/spans (pairs 24-27/49, 12-01), and the robustness lab (malformed glTF — the parser-fragility loop).

## Tasks
- [ ] OBJ: vertices/normals/UVs/faces/indices, groups; render a real model (Suzanne — the classic) in your 10-05 renderer
- [ ] glTF: JSON scene graph, .bin buffers, accessors (the byte-span math), mesh primitives, materials (PBR-lite)
- [ ] Integration: into 10-13 path tracer (load a model, trace it — your 24-41 search-engine scale-mindset helps with accessor indexing)
- [ ] Robustness lab: malformed glTF (bad accessor offsets, huge counts, cycle references) — clean failures, no OOM (pairs 24-27/35) — `labs/`
- [ ] Writeup: the glTF-vs-OBJ design (why the era format is a buffer soup), 3D-format RE — `notes/`

## Resources
- glTF 2.0 spec (the manual); the OBJ spec; three.js glTF loader source (peer); your 10-05/10-13 notes

## Exit Criteria
- [ ] OBJ + glTF models render in your own pipeline — `labs/`
- [ ] Malformed-model clean-failure matrix — `labs/` + `notes/`

## Links
- [glTF 2.0 spec](https://registry.khronos.org/glTF/specs/2.0/glTF-2.0.html)
- [three.js](https://threejs.org/)
