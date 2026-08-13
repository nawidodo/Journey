# 25-11 · Android + iOS privesc — Magisk internals, kernel bugs, jailbreak-as-privesc

**Week:** W20+ parallel · **Track:** Q · **Prev:** [`../10-macos-privesc`](../10-macos-privesc/README.md) · **Next:** [`../12-capstone-tool-triage`](../12-capstone-tool-triage/README.md)

## Objective
Mobile root is privesc against SELinux/verified-boot (Android) and the sandbox/codesign/SEP stack (iOS). Understand the tools that do it — Magisk on Android, the jailbreak lineage on iOS — and how they differ from a normal user→root jump.

## Tasks
- [ ] Android security model as boundaries: app sandbox (uid/gid), SELinux domains (`untrusted_app` vs `platform_app` vs `system` vs `su`), Zygote, AVB verified boot, `ro.secure`/`ro.debuggable`
- [ ] Magisk internals: what "systemless" means — `magiskboot` patches the boot ramdisk (not `/system`), the `magiskinit` phase-1 stub, patching `sepolicy` (custom rules loaded early), the `magiskd` daemon + `su` (uid `root` but *SELinux* `magisk` domain), why it survives OTA; read the source layout `native/`, `jni/`
- [ ] Classic Android root routes: `adb root` (userdebug builds), kernel bugs — CVE-2020-0041 (gpio), binder UAF CVE-2019-2215, `perf_event_open` (CVE-2016-5195 lineage) — link Phase 16 Track H depth; `run-as` on debuggable apps
- [ ] iOS: jailbreak = privesc chain against: sandbox (seatbelt), codesign (AMFI), kernel (PAC/APRR/KTRR), SEP; the tool lineage — checkm8 bootrom → tfp0 (`task_for_pid` kernel port) → KFD (pattern-f) → Fugu15/Landa (PAC) — Phase 7 owns the depth, this step links the *privesc interpretation*: each stage is a boundary crossed
- [ ] The mobile difference: why Android root is "just" SELinux bypass + gid 0, but iOS root is meaningless without jailbreak (codesign still blocks); the "trusted execution" stack SEP/TEE as the final boundary
- [ ] Lab: on your own unlocked device or AVD — root it with Magisk, then inspect: `su` context (`id` under `magisk` domain), which SELinux domains gained privileges, what AVB state changed — `labs/` (device you own only, per Journey safety rules)

## Resources
- Magisk source + topjohnwu blog posts
- Phase 7 (iOS) and Phase 16 (Android) steps for CVE depth
- AOSP security docs, SELinux policy reference

## Exit Criteria
- [ ] Own device rooted via Magisk, SELinux context audit documented — `labs/`
- [ ] Explain in ≤5 lines why iOS root ≠ jailbreak but Android root ≈ SELinux bypass — `notes/`
- [ ] Trace one mobile privesc CVE (binder UAF or checkm8) from bug to boundary crossed — `notes/`

## Links
- [Magisk](https://github.com/topjohnwu/Magisk)
- [Magisk architecture (topjohnwu)](https://topjohnwu.github.io/Magisk/)
- [CVE-2020-0041 (gpio) analysis](https://github.com/Chw41/CVE-2020-0041)
- [ipwndfu (checkm8)](https://github.com/axi0mX/ipwndfu)
- [kfd](https://github.com/felix-pb/kfd)
- [AOSP security docs](https://source.android.com/docs/security)
