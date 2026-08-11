# 19-04 · macOS Hooking

**Week:** W25–27 · **Track:** K · **Prev:** [`../03-linux-hooking`](../03-linux-hooking/README.md) · **Next:** [`../05-ios-hooking`](../05-ios-hooking/README.md)

## Objective
Mach-O-specific interception — dyld interpose, fishhook, ObjC swizzle. Host-safe (userspace).

## Tasks
- [ ] Own interpose library: `__interpose` section + `DYLD_INSERT_LIBRARIES` (dyld primitive; your shim is from scratch)
- [ ] Reimplement fishhook's technique: rebind Mach-O lazy/non-lazy symbol pointers via dyld APIs
- [ ] Own ObjC swizzler helper (`method_exchangeImplementations` wrapper + lookup)
- [ ] Code-signing implications: library validation, entitlements
- [ ] Frida as cross-check only; detect hooks via `dyld_all_image_infos`, image-list comparison

## Resources
- facebook/fishhook; dyld interpose docs; Frida on macOS

## Exit Criteria
- [ ] fishhook + swizzle demos — `code/`

## Links
- [fishhook (Facebook)](https://github.com/facebook/fishhook)
- [dyld interpose docs](https://opensource.apple.com/source/dyld/) — search 'interpose'
