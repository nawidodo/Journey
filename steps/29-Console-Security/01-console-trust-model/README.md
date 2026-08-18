# 29-01 · Console trust model — the platform security landscape (Phase 29 welcome)

**Week:** W41+ opt (niche) · **Track:** U · **Prev:** none (first) · **Next:** [`../02-ps4-ps5-orbis-kernel`](../02-ps4-ps5-orbis-kernel/README.md) · **Pairs:** 02-14/15, 04-01, 24-01

## Objective
Consoles are the hardest consumer targets: fully accountable hardware, signed everything, no direct debug. Phase 29 is the platform-security capstone of the whole plan — the read-research half of everything 24-01 (own kernel) and 02 (emulators) built. Map the trust model of every current family — PS4/PS5 (Orbis: FreeBSD 9 derived, WebKit browser entry, Hypervisor on PS5, AMD APU), Switch (NVIDIA Tegra X1 — ARMv8, TrustZone, undocumented security processor, RCM/fuses), Xbox One/Series (x86-64 AMD, Hyper-V-derived hypervisor, Secure Kernel) — plus the historical free run (NES→PS2 cheat-era HAXX/ModChip) that made the discipline. The deliverable is the comparison matrix: boot chain, code-signing authority, kernel hardening, userland sandbox, and the *entry surface* of each (web browser, USB, game saves, disc/media). Everything cross-checks your Phase 07 (iOS) and 24-01 thinking — same fuses, same chain-of-trust, different silicon. Own-console work (if any) is lab-only; every family has a public research paper trail.

## Tasks
- [ ] Threat-model sheet: per platform — boot ROM → bootloaders → kernel → runtime, who signs what, who holds keys (fuses vs TEE vs blob), downgrade protection
- [ ] Entry-surface inventory: browser engines on PS4/5 (WebKit age), Switch handheld/network, Xbox media/network — where the public chains started
- [ ] History: the pre-DRM era (EEPROM/button codes), ModChip/solder era, and why the modern all-signed era is qualitatively harder (the 21-10 evasion thinking flipped)
- [ ] Matrix artifact: one table (rows = platforms, cols = boot/auth/hardening/sandbox/entries) — `labs/`
- [ ] Writeup: what consoles teach that PCs don't (a fully hostile owner, no admin, signed-everything) — `notes/`

## Resources
- Public platform-security writeups (fail0verflow, Modern Vintage Gamer history, PS4/5 ROP research); Phase 02 emulator notes (PS1/PSP silicon RE) as the historical baseline

## Exit Criteria
- [ ] Comparison matrix + entry-surface inventory — `labs/` + `notes/`

## Links
- [fail0verflow console research](https://fail0verflow.com/blog/)
- [Modern Vintage Gamer (console security history)](https://www.youtube.com/@ModernVintageGamer)