# 29-05 · Capstone — console platform matrix, the across-Silicon deep-dive (Phase 29)

**Week:** W41+ opt (niche) · **Track:** U · **Prev:** [`../04-xbox-hypervisor`](../04-xbox-hypervisor/README.md) · **Next:** none (last) · **Pairs:** 01-04, 21-06, 24-01

## Objective
Phase 29 capstone: synthesize the four platform studies into one artifact — the full matrix (boot chain, key authority, hardening, sandbox, entry surfaces, historical openings) for PS4/5 vs Switch vs Xbox One/Series vs iOS (07) vs desktop (24-01) — and one deep-dive re-creation: pick the single most instructive class (PS4-era UAF-to-kernel, Tegra boot-verifier flip, or HV isolation proof) and write it up as if you were the researcher (motivation → hypothesis → fuzzing/recon → chain → patch timeline → defense). The point is the transfer: after this you can land on ANY signed-everything platform and know exactly which questions to ask — the specialist skill the year's exits test for. Own-hardware labs stay lab-only; the deliverable is intellectual, portable.

## Tasks
- [ ] Matrix artifact: full comparison table, every row sourced to the phase notes/labs — `labs/`
- [ ] Deep-dive: one re-created chain-class from 29-02/03/04, written as a research narrative + working lab artifact — `labs/` + `notes/`
- [ ] Defense synthesis: what each platform's fix teaches (freelist hardening, fuses, HV isolation, signed-everything) and the one pattern all of them share — the "hostile owner" model vs 24-01 desktop reality
- [ ] Exit interview sheet: 10 questions a console-security employer would ask (trust chain, entry surface, why UAF persists) with answers grounded in your labs — `notes/`

## Resources
- All four phase steps + your 24-01/04-07/06-06 notes; public researcher talks as the caliber target

## Exit Criteria
- [ ] Full matrix + one research-grade deep-dive — `labs/` + `notes/`

## Links
- [fail0verflow blog](https://fail0verflow.com/blog/) · [ReSwitched](https://reswitched.github.io/) · [Switchbrew](https://switchbrew.org/)