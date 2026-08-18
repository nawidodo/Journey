# 29-06 · Console archive — the long tail: retro families, handhelds, micro/PC-handhelds (Phase 29)

**Week:** W41+ opt (niche) · **Track:** U · **Prev:** [`../05-capstone-platform-matrix`](../05-capstone-platform-matrix/README.md) · **Next:** none (last) · **Pairs:** 02-02/12/14/15, 24-117, 04-07

## Objective
The covered families (29-02 PS4/5, 29-03 Switch, 29-04 Xbox, 24-117 GB, Phase 02 NES/SNES/Genesis/PS1/PSP/GBA) leave a long tail of platforms nobody emulates for study. Sort them into five classes and mine each for one transferable lesson — the coverage matrix is the artifact, one own-emulator from the dead family is the build:
1. **Discrete-logic TV consoles** (Magnavox Odyssey¹/², early Pong-class) — pre-CPU: the game was the hardware. Lesson: the CPU came late; what changes when the "program" is wiring.
2. **6502/68000-retro families** (Atari 2600/5200/7800/Lynx, ColecoVision, Intellivision, Sega Master System, Game Gear, Neo Geo Aes/CD/Pocket(-Color), WonderSwan(-Color), TurboGrafx-16/PC Engine/PC Engine CD, Virtual Boy) — small CPUs + custom video chips. Lesson: the 6502/68000 core family you already speak (02-02) — each is a register-set delta + a wild PPU.
3. **Media-tray consoles** (3DO, Panasonic 3DO, Philips CD-i, Commodore CD32) — CD-era "multimedia appliances" on ARM/68k. Lesson: DRM/media-lockout started here; the 20-08/20-09 crypto-reading discipline maps directly.
4. **Nintendo 3D+ era** (N64, GameCube, Wii, Wii U, DS, 3DS, vitality of the Switch 29-03 study) — 3D cores + media+signed firmware. Lesson: the 02-14/15 (PS1/PSP) GPU/rendering and 29-03 signing progression — the family arc is one story.
5. **Micro / PC-adjacent handhelds** (Ouya, Steam Deck, Lenovo Legion Go, ASUS ROG Ally, MSI VCS, NVIDIA SHIELD-family) — Android/x86/Linux consoles: a PC with a launcher, or an Android box with a controller. Lesson: SteamOS = Linux + TPM/secure-boot (04-07), Proton/Wine compatibility layer (the 07-06 translation discipline) — the Asahi-style "undocumented platform, document it" template applied to a consumer box you own.
6. **Cloud/VR** (PlayStation VR/VR2, GeForce NOW) — hardware-gated (VR) and cloud-only (NOW): study-note rows only, marked ceiling — no lab.
The build: **Atari 2600 emulator-lite** — 6507 (6502 minus decimal mode — reuse your 02-02 core delta), the TIA video/audio chip (the raster discipline of 02-03/24-117), and one cartridge (Adventure-class) booting to its loading screen.

## Tasks
- [ ] Coverage matrix: family → CPU → video/audio chip → emulator path → one transferable lesson — the full long-tail table, every row sourced — `labs/`
- [ ] Class writeups: one paragraph per class above (discrete-logic → media-tray → 3D-era → micro/PC) — what each taught the industry — `notes/`
- [ ] Build: Atari 2600 emulator-lite (6507 core delta + TIA + cart loader), test ROM boots, manual screen check — `labs/` + `code/`
- [ ] PC-handheld study: SteamOS secure-boot/TPM + Proton translation layer (04-07/07-06 reuse) from public docs — the Asahi-style doc-reading exercise — `notes/`
- [ ] Ceiling rows: PS VR/VR2, GeForce NOW marked hardware/cloud-gated — study notes only — `notes/`

## Resources
- MAME source (the metadata truth), Atari 2600 TIA docs, SteamOS/Steam Deck public documentation, your 02-02/02-12/24-117 cores

## Exit Criteria
- [ ] Long-tail coverage matrix + class writeups — `labs/` + `notes/`
- [ ] Atari 2600 emulator-lite boots a test ROM — `labs/` + `code/`

## Links
- [Atari 2600 TIA](https://www.randomterrain.com/atari-2600-memories-tia.html)
- [MAME](https://www.mamedev.org/) · [SteamOS](https://store.steampowered.com/steamos/)