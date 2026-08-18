# 24-104 · Own dylib injector — dyld interpose + LC_LOAD_DYLIB, codesign wall (stretch)

**Week:** W46+ opt (stretch) · **Track:** P · **Prev:** [`../103-own-macos-process-monitor`](../103-own-macos-process-monitor/README.md) · **Next:** [`../105-own-logarchive-parser`](../105-own-logarchive-parser/README.md) · **Pairs:** 19-04, 24-97, 24-16, 21-06

## Objective
Runtime hooking on mac/iOS (19-04) runs on dyld's loading model: build the injection toolkit — a test binary (your own, lab-only), a dylib you wrote (24-17/12-04 skills), and three injection paths: DYLD_INSERT_LIBRARIES (the env var — interpose symbol stubs, the constructor-run lesson), LC_LOAD_DYLIB patching (append a load command to your own 24-97 parser/patcher output), and bootstrap/linker-level notes. The wall is the point: on hardened binaries (codesign rpath/HARDENED_RUNTIME, SIP), injection fails — your own 24-97 cdHash knowledge explains exactly why — and the lab demonstrates detected-vs-blocked with your 103 monitor watching (pairs 21-06 EDR view).

## Tasks
- [ ] Dylib: write a probe dylib (constructor logging + interpose of a libc func via dyld interpose table, 19-04 patterns)
- [ ] Env path: DYLD_INSERT_LIBRARIES into own test binary → probe runs; the ordering/side-load rules (dyld man — the manual)
- [ ] Patch path: parse own binary (24-97), append LC_LOAD_DYLIB + fix header values; re-sign via own 24-70 CA keys or ad-hoc
- [ ] Wall lab: same injection vs hardened (codesign --options runtime + library validation) binary → blocked; unsigned/HARDENED off → works; 103 monitor shows probe pages mapped — each case's exact error — `labs/`
- [ ] Writeup: why macOS security is loader-centric (amfid, library validation, SIP — 07), EDR hooking tradeoffs (21-06) — `notes/`

## Resources
- dyld man page + Apple loader docs (the manual); your 24-97 parser + 19-04 notes

## Exit Criteria
- [ ] Own test binary injected via env + patched LC, wall behavior reproduced — `labs/` + `code/`
- [ ] Loader-security writeup — `notes/`

## Links
- [HYBRID_DYLIB / dynamic_lookup docs](https://keith.github.io/xcode-man-pages/dyld.1.html)
- [Frida docs (peer)](https://frida.re/docs/)