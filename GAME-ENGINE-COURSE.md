# Game Engine Course — Absolute-Beginner (hello world → your OWN engine running a real game, gated)

Zero engine knowledge assumed. You need: your Mac, Xcode, and a C-family language you're typing in (C or C++ or Swift — pick C++ for the engine-skill transfer; the repo's C (00-01) and GPU course (U1–U6) are the two feeders you'll want done first). Each unit: concept → do → runnable verification → **lesson check (own words, `notes/geN-quiz.md`)** — no advance without both. ~3h/unit, 12 units + capstone ≈ 8–10 weeks. Fun is the point — this is the "why you started" track — but the engineering is real.

Compass (re-read when lost): an engine is **a loop + a scene + a renderer**: while (game not over) { read input → update entities → render }. Everything an engine "is" is a scalable version of those three lines: ECS makes "update entities" fast; renderers make "draw" fast; assets/physics/audio are subsystems hanging off the same loop. Every unit grows that loop without breaking it.

Safety: none beyond normal coding discipline (own machine, own files; the 60fps target and the debugger are your tools, virtualization not needed). Rules apply as in every course: verification + own-words quiz gate each unit; copy only boilerplate (window/shell scaffolding); erase-and-retry once when stuck; 3h timebox.

---

## GE0 — hello world: the game loop
Concept: games aren't programs that run once — they loop at a rate while sampling input and drawing. Do: a terminal loop: print a frame counter and a moving ASCII dot at 60 ticks/sec with `usleep`/timer; count actual FPS against wall-clock.
Verify: your loop prints 60-ish frames/sec with a moving dot; FPS measured honestly.
**Lesson check:** what is the game loop's contract, and why does FPS *vary* even when your loop "runs 60 times a second"?

## GE1 — time is the subtle bug: timesteps
Concept: fixed vs variable stepping — the physics sim must step a fixed dt while the renderer may run at any rate. Do: implement a fixed-timestep accumulator loop (update 60Hz, render as fast as possible); add interpolation between physics states; measure "jitter" (frame-time variance) and compare against a naive `while(1){update();render();}`.
Verify: jitter chart (`notes/ge1.md`) shows the accumulator variant smoother.
**Lesson check:** why must physics step at fixed dt but rendering vary — and what breaks when you skip interpolation?

## GE2 — 2D shell: input, sprites, collision
Concept: movement + collision = the first playable 10 minutes. Do: window (SDL2 or MetalKit or your GPU-course view), a player square + enemy squares: keyboard input, AABB collision, out-of-bounds death/replay; add a stick-figure sprite (16×16 array of colors = your own pixel art) drawn via your renderer later — define the sprite as data now.
Verify: you move, collide, and die/replay; input handling documented.
**Lesson check:** what does input handling do before your update runs — and why does AABB collision reduce to four compares?

## GE3 — the scene: entity-component-system (ECS)
Concept: entities are IDs; components are data arrays; systems are functions over data — the data-oriented engine heart. Do: minimal ECS in C++: `ComponentPool` arrays (position, velocity, sprite), `Entity` = int, `MovementSystem`/`CollisionSystem` each iterate pools; replace your GE2's ad-hoc objects with ECS; add a for-loop spawner (1000 entities, same systems).
Verify: 1000-entity scene runs at target FPS with ECS; same-old-code comparison documented.
**Lesson check:** why do engines split data from objects — and what does "cache-friendly iteration" mean for component pools?

## GE4 — renderer 1: software raster (the truth floor)
Concept: before GPU abstraction, you raster: transform → project → fill. Do: from 10-05: take (or extend) your software rasterizer; draw your GE2/GE3 sprites as textured quads with transform (position/scale/rotation via your own 3×3/4×4 matrix code); keep it at 30+ FPS at 480p — honest budget math.
Verify: your ECS scene renders through your own rasterizer (matrix-math output verified vs a hand-checked case).
**Lesson check:** walk the render path from entity transform to screen pixel — and where does the matrix multiply actually happen?

## GE5 — renderer 2: GPU (Metal), same scene
Concept: the GPU course pays off: same scene, GPU pipeline. Do: port the GE4 draw to your Metal pipeline (from GPU-BEGINNER-COURSE U5/U6): vertex shader takes the transform, fragment samples the sprite texture; keep ECS loop untouched; measure the FPS jump.
Verify: 60+ FPS at 1080p with the same scene; CPU-vs-GPU time table (`notes/ge5.md`).
**Lesson check:** what changed in the pipeline vs GE4 (rasterize where?) and what stayed identical (the scene data)?

## GE6 — into 3D: math, camera, meshes
Concept: 3D = more math: vectors, cross/dot, perspective projection matrix, a camera. Do: implement the 3D math yourself (vec3, mat4, multiply, inverse-lite, project); build cubes from 8 vertices + index list; camera orbit around the cubes (WASD + drag); wireframe render → z-buffered shaded (your 10-05 z-buffer).
Verify: orbiting camera, z-buffer correct (no painter's-algorithm artifacts) at 30+ FPS — scale a 4×4×4 cube field to prove it.
**Lesson check:** what does the perspective matrix do to a point — and why does z-buffering beat sort-and-paint?

## GE7 — models and lights: the scene grows up
Concept: real scenes need loaded meshes + light math. Do: write a minimal OBJ parser (vertices/faces; it's your own format tool), load a low-poly model (a cube or a public-domain low-poly asset), flat-shade it (normal · light direction), add a second light; per-entity material (color/specular-lite).
Verify: OBJ model renders lit, lights adjustable live, material per-entity works.
**Lesson check:** what does flat shading compute per-face — and why per-vertex normals change the look (and the math)?

## GE8 — physics-lite: the bounce that sells the game
Concept: games fake physics: integrate velocity, detect collision, apply impulse — enough to feel real. Do: gravity + jump + platforming collision in your engine: AABB vs AABB with one-way platforms (only land from above), velocity integration with dt (GE1's fixed step), restitution (bounce) + a simple impulse for moving platforms.
Verify: a jumping/landing/bouncing player across platforms feels stable at fixed step (no tunneling at 60Hz for your speeds).
**Lesson check:** what is "integration" doing each frame — and what makes collision *tunneling* happen and how does fixed-dt help?

## GE9 — audio: events, not synthesis
Concept: engines don't sing, they play samples on events: mixer + triggers. Do: load 2–3 sound files (SFX-Recorder or public-domain) via your platform audio (macOS AVAudioEngine or SDL_mixer), play on jump/land/hit events from your systems; volume/DSP-lite (one filter) as extension.
Verify: sounds trigger on the right game events (log-verified), volume control works.
**Lesson check:** why does audio live on events — and what's the coupling risk between audio and the fixed-step loop?

## GE10 — assets & the dev console
Concept: assets are data (not code): textures/levels/configs loaded from files; dev tools = your best engineering feature. Do: sprite/level file format of your own (text or binary you define: level = grid + sprite refs), texture atlas for sprites; dev console overlay (toggle with `) logging FPS, entity count, mid-game toggles; settings save/load.
Verify: your level files load and build scenes at boot; console toggles work live.
**Lesson check:** why do engines define asset formats instead of hardcoding — and what does a dev console buy you during every later hour of work?

## GE11 — netcode as an engine feature (lite but real)
Concept: multiplayer changes authority: server decides, clients predict. Do: UDP loop (socket basics from 24-20): echo + position sync two-instance test on your own machine (two windows or two processes, loopback); authoritative server for your player + client interpolation; mark rollback/prediction as the read-only stretch (honest ceiling note).
Verify: two instances sync positions over loopback UDP; server-authority demo (client cheating attempt ignored).
**Lesson check:** why does the server own the truth — and what does interpolation hide (and not hide)?

## GE12 — CAPSTONE: your engine ships a game
Prereq: GE0–GE11. **Close all notes.** Build ONE complete small game in your engine (your choice: 2D platformer or 3D arena — 10-minute playtime): levels via your GE10 format, your ECS + physics + audio + GPU renderer, dev console bound to a debug key, 60fps target. Re-create the loop+ECS+renderer core cold first (the gauntlet discipline), then finish the game. Write `labs/engine-capstone.md` like a post-mortem: architecture diagram, three best decisions, three regrets, what a second engine would do differently.
**Pass = the game plays end-to-end from your engine without notes; the post-mortem reads like an engineer's.**

---

## Rules
1. Verification + lesson check both true before next unit; quizzes in your own words.
2. Copying allowed only in GE0/GE2 boilerplate (window/shell setup) — loops, ECS, renderers, physics written by you; erase-and-retry once when stuck.
3. 3h/unit; stuck past that = previous unit's verification again.
4. Own machine; honesty about FPS numbers (the profiler is a friend, from GE1 onward).
5. Honest bar: real engines are decades of team work (Unity/Unreal); this course's bar = your own loop→ECS→software+GPU render→physics→assets→audio→lite-net engine that runs a real 10-minute game at 60fps, written and understood cold — the floor for game/engine dev and, not coincidentally, a giant slab of the systems knowledge every other course builds on. The path tracer (10-13) and raycaster (24-63) wait as fun extensions after the capstone.

## Where this lives
Built on the GPU course (U1–U6), Python-path's data ideas, and 10-05's software raster. Pairs EMULATOR work (a console = an engine with fixed hardware; you're now writing both sides of that world).