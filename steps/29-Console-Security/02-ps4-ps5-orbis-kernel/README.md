# 29-02 · PS4/PS5 Orbis — FreeBSD-derived kernel, the WebKit→kernel chains (Phase 29)

**Week:** W41+ opt (niche) · **Track:** U · **Prev:** [`../01-console-trust-model`](../01-console-trust-model/README.md) · **Next:** [`../03-nintendo-switch-tegra`](../03-nintendo-switch-tegra/README.md) · **Pairs:** 04-08, 05-01, 24-20, 24-19

## Objective
PS4 shipped a heavily modified FreeBSD 9 ("Orbis" kernel) — the same lineage your 04-08 freebsd study and 24-01 kernel lab came from. Dissect the public chain history: the 2017–18 era WebKit renderer 0-days → ROP → kernel object corruption (the free()/use-after-free classics, exactly 04-05 class), plus the post-2020 PS5 shift (latest firmware = signed + HV + newer kernel — much harder public ground). The lab is the reconstruction: boot an era-appropriate FreeBSD 9 in your own VM and re-create the *vulnerability class* — same-family heap/UAF exploit against your own test kernel/driver (own VM, own code, lab-only), then write the defense (04-05 mitigation notes, heap hardening) that would have stopped it. PS4/PS5 exploitation itself stays study-only: own console optional, never someone else's.

## Tasks
- [ ] Orbis study: what Sony changed in FreeBSD 9 (removed modules?, patched syscalls, added kernel ASLR/guard — diff vs vanilla 9), the firmware/research paper trail (fail0verflow/TheFloW writeups)
- [ ] Chain archaeology: WebKit bug → sandbox escape → kernel bug → ROP → code exec; where each link lived (browser/userland/kernel — your 03/04/05 synthesis)
- [ ] Lab: own VM FreeBSD 9 (or clone) — write a UAF-class PoC against your own test driver, exploit it, then harden (freelist/check) and re-test — the defense loop — `labs/` + `code/`
- [ ] PS5 delta: HV intro, newer kernel, why public chains went quiet (the 04-09 lesson: complexity wins defense)
- [ ] Writeup: the Orbis timeline — what a modern paid/bug-bounty-era PS5 chain would have to thread — `notes/`

## Resources
- fail0verflow, TheFloW (PS4), known PS4 entries; your 03-01..05, 04-05/08, 24-20 notes

## Exit Criteria
- [ ] UAF-class PoC + hardening re-test on own VM — `labs/` + `code/`
- [ ] Orbis chain-timeline writeup — `notes/`

## Links
- [fail0verflow PS4](https://fail0verflow.com/blog/2014/console-hacking-2013-ps4/)
- [PS4 research (TheFloW)](https://github.com/TheOfficialFloW/)