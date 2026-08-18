# 24-115 · Own iOS app analyzer — ipa/mach-O static RE, no device needed (stretch)

**Week:** W46+ opt (stretch) · **Track:** P · **Prev:** [`../114-own-image-stabilizer`](../114-own-image-stabilizer/README.md) · **Pairs:** 24-97, 24-109, 15-08, 07-08

## Objective
iOS apps are zip + Mach-O + plists: analyze them statically on your Mac without any device — an analyzer-lite — .ipa unpack (zip — 24-25), binary inventory (main + frameworks/.dylibs, 24-97 walk), Objective-C/Swift class surface (the objc class list + method names from Mach-O sections — the RE goldmine), entitlements/plist decode (24-109), and a report: frameworks used, suspicious strings (py strings, URLs — 24-13 patterns), encryption/obfuscation heuristics (LC_ENCRYPTION_INFO, _DATA obfuscation markers). Targets: your own signed test app, open-source apps, or any app you own the binary to (lab-only, no App Store scraping). This is the no-hardware entry to iOS RE that 07's jailbreak research presupposes.

## Tasks
- [ ] Unpack: zip read (24-25 inflate), bundle layout, Info.plist (24-109) → display name/version/entitlements
- [ ] Mach-O: walk main binary + every dylib (24-97 reuse), map sections, list `__objc_classlist`/`__objc_methlist` → class/method inventory (the selector table)
- [ ] Report: strings extraction (24-13) + URL/flag patterns, encryption flag (LC_ENCRYPTION_INFO), framework graph (imports)
- [ ] Verify: compare your class dump vs `class-dump`/Hopper output on the same binary (oracle) — `labs/`
- [ ] Lab: analyze an open-source app or your own 00-04 Swift build — the full report artifact — `labs/`
- [ ] Writeup: why static iOS RE matters pre-jailbreak (threat intel, malware triage — 12 pairs), obfuscation arms race (strings/LLVM-FLATTEN mirrors) — `notes/`

## Resources
- `class-dump`/`otool` behavior (the manual); your 24-97/24-109/15-08 code as the engine room

## Exit Criteria
- [ ] Analyzer produces a class/method + entitlement report matching oracle tools — `labs/` + `code/`
- [ ] iOS-static-RE writeup — `notes/`

## Links
- [class-dump](https://github.com/nygard/class-dump)
- [Hopper-style tooling notes](https://www.hopperapp.com/)