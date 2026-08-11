# 19-05 · iOS Hooking

**Week:** W27–29 · **Track:** K · **Prev:** [`../04-macos-hooking`](../04-macos-hooking/README.md) · **Next:** [`../06-android-hooking`](../06-android-hooking/README.md)

## Objective
The tweak model — how jailbreak tweaks actually hook, and Frida's non-jailbreak path. Own spare device only.

## Tasks
- [ ] Port your fishhook-equivalent + detour engine to iOS (arm64)
- [ ] Own Objective-C runtime swizzling in-app
- [ ] Minimal tweak where the hooking is yours: theos only compiles/signs, you write the MSHookFunction-equivalent on your own engine
- [ ] Sideloaded Frida as cross-check on a non-jailbroken device
- [ ] Anti-Frida / jailbreak-detection arms race (feeds Phase 7 mindset)

## Resources
- ElleKit docs; theos; Frida docs; your spare iPhone

## Exit Criteria
- [ ] Own engine + swizzle demo; tweak if a device is available — `code/` + `labs/`

## Links
- [libjailbreak](https://github.com/opa334/libjailbreak)
- [Frida iOS docs](https://frida.re/docs/ios/)
