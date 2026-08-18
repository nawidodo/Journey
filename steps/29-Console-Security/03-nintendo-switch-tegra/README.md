# 29-03 · Nintendo Switch — NVIDIA Tegra X1, TrustZone, RCM/fuses (Phase 29)

**Week:** W41+ opt (niche) · **Track:** U · **Prev:** [`../02-ps4-ps5-orbis-kernel`](../02-ps4-ps5-orbis-kernel/README.md) · **Next:** [`../04-xbox-hypervisor`](../04-xbox-hypervisor/README.md) · **Pairs:** 02-11, 22-09, 24-28, 04-07

## Objective
Switch = Tegra X1 (the ARMv8 you trained on in 24-110/02-11) with a security processor nobody documents and a famously broken early boot path (RCM modes / fuses). Study: BootROM → BCT/secure boot (Nvidia's chain on gen-1, the public protégé: RCM-recovery, fuse blowing for OPP/DRM), the TrustZone userland split (the ARM TZ you met in 16-07/22-09), title/save/firmware signing (NCA/nacp structure — format work like your 24-99/24-109 parsers), and the post-fix escalation: why later units are closed and the economics of it. The lab: model the gen-1 boot chain in your own 02-11/24-28 emulator — a simulated BootROM→BCT verifier with a bit-flip attack surface (the 22-07 fault-injection thinking, simulated), and a format parser for the NCA header (24-99-style box-walk). Own-Switch work lab-only if you own one; everything runs emulated/parsed.

## Tasks
- [ ] Boot-chain model: BootROM → BCT → secure boot → kernel — draw it with keys/fuses (29-01 matrix row)
- [ ] RCM/fuse study: why gen-1 bis was public (the RE on NVIDIA's boot code), what fuses seal later (downgrade protection — your 29-02 note cross-check)
- [ ] Lab: NCA-header parser (magic, crypto context, section table — your 24-99 discipline) + emulated boot-verifier with flip-attack simulation — `labs/` + `code/`
- [ ] Writeup: TrustZone split misuse vs design (why Nintendo's TZ applets were the interesting bird), the fix arms race — `notes/`

## Resources
- Public Switch research (ReSwitched, Switchbrew wiki — the format truth), Tegra TRM docs; your 02-11/24-28/24-99 code

## Exit Criteria
- [ ] NCA parser + emulated boot-verifier lab — `labs/` + `code/`
- [ ] Boot-chain/fuse writeup — `notes/`

## Links
- [Switchbrew](https://switchbrew.org/)
- [ReSwitched](https://reswitched.github.io/)