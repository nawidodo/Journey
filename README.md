# Journey

Self-paced roadmap to **Systems Security Engineer** — exploit development, kernel internals, malware dev, and defense. Built as 15 parallel learning tracks mapped to numbered phases, one folder per learnable step, with a master plan, per-phase checkpoints, and gap analysis.

## What's inside

| Path | Purpose |
|---|---|
| [`LEARNING_PLAN.md`](LEARNING_PLAN.md) | **Master roadmap** — tracks A–N, phases 0–22, week-by-week plan, milestones 🚩 M1–M23, load-control notes. Start here. |
| [`steps/`](steps/) | The work — one folder per step, in order. |
| [`steps/README.md`](steps/README.md) | Step conventions, structure table (phase → contents → weeks → track), milestone tracker. |
| [`steps/23-Career`](steps/23-Career) | Post-plan career phase (Track O): writeups/portfolio, resume/interview, coordinated disclosure, career-launch capstone. |
| [`RECOMMENDATIONS.md`](RECOMMENDATIONS.md) | Gap analysis & how the plan evolved (what was added and why). |

## How to navigate

1. Read `LEARNING_PLAN.md` — pick your track (A = security core, B = Apple hardware, C = graphics, D = Windows kernel, E = malware dev, F = USB, G = cross-OS drivers, H = Android exploitation, I = Android malware, J = rootkits, K = runtime hooking, L = crypto, M = detection/DFIR, N = embedded USB devices).
2. Work a phase folder in `steps/NN-*` — steps numbered `NN-MM`, capstone last.
3. Every step = `README.md` (Objective / Tasks / Resources / Exit Criteria / Links) + three subfolders:
   - `notes/` — writeups, book notes, diagrams
   - `code/` — source you write
   - `labs/` — lab work, POCs, exploit artifacts
4. Tick the Exit Criteria boxes → mark the tracker → next step. Steps flagged 🚩 M# are phase checkpoints: re-derive them with the writeup closed.

## Conventions

- **Step done** = all Exit Criteria ticked + artifact in `labs/` (or per-criterion location). No boxes ticked = not done.
- **Links** = concrete URLs per step (docs, repos, blogs, practice platforms) — open before starting.
- **Phase numbering** = chronological for Track A (00–09), parallel tracks beyond (10–22, run any time); track is metadata in each README header.

## Safety

This is offensive-security study material (exploits, malware, implants). All lab work targets VMs, sandboxes, CTF platforms, and hardware you own. No real-target testing — scope and authorization are prerequisites, as noted in the steps themselves.
