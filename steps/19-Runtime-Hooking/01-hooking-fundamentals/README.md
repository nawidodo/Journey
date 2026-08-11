# 19-01 · Hooking Fundamentals

**Week:** W20–21 · **Track:** K · **Prev:** — · **Next:** [`../02-windows-userland-hooking`](../02-windows-userland-hooking/README.md)

## Objective
The one skill, five platform mechanisms. When hooking is a feature (EDR, devtools) vs a bug (malware).

## Tasks
- [ ] Hook classes: IAT/EAT, inline/trampoline (detour), breakpoint (INT3/VEH), callback/notification, interpose/swizzle, LD_PRELOAD
- [ ] Anatomy of an inline hook: overwrite prologue, save original, trampoline, restore
- [ ] From-scratch primer: what a hook engine needs — code patching, trampoline, hook list, thread safety, unhook; which parts are OS primitives vs your code
- [ ] Where the OS hooks itself: EDR, ESF, Sysmon, kernel callbacks (map to Track J)
- [ ] Detectability: what each hook looks like to a memory-integrity scanner
- [ ] Trust boundary: userland vs kernel vs hypervisor hooks

## Resources
- Detours docs; hooking writeups (Phrack, VX-Underground); your RE phase notes

## Exit Criteria
- [ ] One-page "hook taxonomy per OS" map — `notes/`

## Links
- [Frida docs](https://frida.re/docs/hooking/)
- [LiveOverflow: hooking overview](https://www.youtube.com/watch?v=Q5qY8V3cB_o)
