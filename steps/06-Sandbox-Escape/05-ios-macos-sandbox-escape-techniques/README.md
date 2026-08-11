# 06-05 · iOS/macOS Sandbox Escape Techniques

**Week:** W36–37 · **Track:** A · **Prev:** [`../04-apple-app-sandbox-seatbelt`](../04-apple-app-sandbox-seatbelt/README.md) · **Next:** [`../06-vmm-hypervisor-fundamentals`](../06-vmm-hypervisor-fundamentals/README.md)

## Objective
How Apple's sandbox actually falls: kernel bugs, TCC abuse, extension/container confusion. Bridges straight into jailbreaking (Phase 07).

## Tasks
- [ ] Kernel-bug-driven escapes: sandbox exit via XNU memory corruption (audit Project Zero iOS reports)
- [ ] TCC bypasses: `kTCCService`-style prompts, TCC db manipulation
- [ ] Container-path + file-coordination escapes (older variants)
- [ ] Entitlement/extension abuse as a sandbox-escape vector
- [ ] Map the canonical jailbreak flow: **sandbox escape + tfp0 + root** — `notes/`
- [ ] Writeup of one real escape chain (e.g., a P0 iOS bug report) — `notes/`

## Resources
- Project Zero iOS sandbox-escape writeups
- XNU source (sandbox, MACF, TCC-related)
- Apple *Platform Security Guide* (sandbox chapter — revisit M5)

## Exit Criteria
- [ ] One real iOS sandbox escape analyzed end-to-end — `notes/`
