# 18-07 · Capstone: Bootkit → Rootkit → Detect 🚩 M19

**Week:** W55–56 · **Track:** J · **Prev:** [`../06-mobile-rootkit-bootkit`](../06-mobile-rootkit-bootkit/README.md)

## Objective
The full offensive capstone on one platform: persist below the OS, hide, evade — then catch it with your own rules.

## Tasks
- [ ] Choose platform (Windows UEFI, Linux, macOS, Android, or iOS)
- [ ] Chain: bootkit/firmware persistence → rootkit (hide + stealth) → survive reboot
- [ ] Write detection: integrity check, behaviour rules, boot-time scan (defensive mirror)
- [ ] Red-team writeup: full chain + what a real defender would catch
- [ ] Tradeoff table: what's detectable, what needs firmware-level defense

## Resources
- Your 18-01–06 outputs

## Exit Criteria
- [ ] **M19:** working chain + catching detections — `labs/` + `notes/`