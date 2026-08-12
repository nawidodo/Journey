# 22-07 · Fault Injection & Glitching (hardware side-channel) — stretch

**Week:** W36 (parallel, optional) · **Track:** N · **Prev:** [`../06-advanced-firmware`](../06-advanced-firmware/README.md) · **Next:** [`../08-capstone-attack-device`](../08-capstone-attack-device/README.md)

## Objective
First pass at the hardware side of syssec: voltage/EM glitching basics, a working glitch on a target you own, and the TEE/firmware attacker model (checkm8 is a bootrom *software* bug — this is the *other* way in). Stretch, not core — hardware cost is the gate.

## Tasks
- [ ] Theory: fault classes (clock/voltage/EM), fault models, countermeasure categories (redundancy, integrity checks, error detection) — `notes/`
- [ ] Build or buy a glitcher: chipWhisperer-class, or DIY Pico-based (Track N hardware + a MOSFET/crowbar circuit) — `labs/`
- [ ] One working glitch on a target you own (dev board firmware decision bypass, e.g. skip a check/boot guard) — `labs/`
- [ ] Map result: glitch timing/width window, target age, what failed vs succeeded — `notes/`
- [ ] Read how glitching appears in real chains (mobile/secure-boot bypass research; checkm8-adjacent fault work)

## Resources
- chipWhisperer docs + tutorials (NewAE); Colin O'Flynn's glitch courses/videos
- Your 22-01…22-06 builds and lab machine

## Exit Criteria
- [ ] One working fault on owned hardware + timing window documented — `labs/` + `notes/`
- [ ] Countermeasures and mitigations written up — `notes/`

## Links
- [ChipWhisperer](https://github.com/newaetech/chipwhisperer)
- [NewAE glitch documentation](https://chipwhisperer.readthedocs.io/)
- [Pico glitcher projects (DPA/glitch)](https://github.com/newaetech/chipwhisperer/tree/develop/hardware/victims)