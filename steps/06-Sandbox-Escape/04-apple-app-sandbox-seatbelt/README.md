# 06-04 · Apple App Sandbox — Seatbelt

**Week:** W35–37 · **Track:** A · **Prev:** [`../03-container-escape-runc-cves`](../03-container-escape-runc-cves/README.md) · **Next:** [`../05-ios-macos-sandbox-escape-techniques`](../05-ios-macos-sandbox-escape-techniques/README.md)

## Objective
Apple's sandbox: seatbelt profiles, MACF enforcement, entitlements — the boundary every iOS app lives behind.

## Tasks
- [ ] Architecture: `sandboxd`, `Sandbox.kext`/MACF, profile evaluation (operations + filters)
- [ ] Profile language: `(version 1)`, `(allow ...)`, `(deny ...)`, path filters, `(require ...)`
- [ ] `sandbox-exec` / `sandbox_init`; container dirs; extension APIs
- [ ] Entitlements: `com.apple.security.app-sandbox`, `...network.client`, temp/extension keys
- [ ] Write a profile that denies everything except one path; prove it with `sandbox-exec` — `labs/`
- [ ] Read xnu `security/` sandbox (seatbelt) sources — `notes/`

## Resources
- Apple *App Sandbox Design Guide*
- XNU `sandbox/` (seatbelt) source
- Levin Vol 2 (MACF/sandbox)
- `sandbox-exec` man page

## Exit Criteria
- [ ] Explain profile → operation → verdict pipeline from memory — `notes/`
- [ ] Working `sandbox-exec` demo — `labs/`

## Links
- [App Sandbox design guide (archive)](https://developer.apple.com/library/archive/documentation/Security/Conceptual/AppSandboxDesignGuide/)
- [P0 sandbox posts](https://googleprojectzero.blogspot.com/)
