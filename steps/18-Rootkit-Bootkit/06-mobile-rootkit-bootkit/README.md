# 18-06 · Mobile Rootkit/Bootkit: Android + iOS

**Week:** W53–55 · **Track:** J · **Prev:** [`../05-macos-rootkit-kext-dext`](../05-macos-rootkit-kext-dext/README.md) · **Next:** [`../07-capstone-rootkit-bootkit`](../07-capstone-rootkit-bootkit/README.md)

## Objective
The mobile variants — persistence in boot images and kernel modules, jailbreak-style. Reuses Track H/I + Phase 7 (checkm8).

## Tasks
- [ ] Android: kernel-module rootkit on rooted AVD; hide via sepolicy + system_server hooks
- [ ] Android bootkit: modify boot image (the Magisk-inverse — malicious init/kernel patch)
- [ ] iOS: post-jailbreak persistence; checkm8 (A5–A11) as a bootrom-level bootkit — survives re-flash
- [ ] TrustZone/SEP boundary: what survives and what doesn't
- [ ] Detect: Play Integrity/DeviceCheck, runtime integrity, IoC triage

## Resources
- Your Track H/I outputs; Magisk source; Phase 7 checkm8 materials

## Exit Criteria
- [ ] One mobile platform rootkit + bootkit demo — `labs/`