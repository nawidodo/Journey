# MASTER ROADMAP — Systems Security Engineer

Thirteen tracks. Track A sequential (security core), Track B parallel (hardware/Apple), Track C parallel (graphics: Metal + Vulkan/DX, shaders, SIMD, software renderer), Track D parallel (Windows kernel), Track E parallel (malware dev), Track F parallel (USB security), Track G parallel (cross-OS driver dev), Track H parallel (Android exploitation), Track I parallel (Android malware dev), Track J parallel (rootkit/bootkit, all OSes), Track K parallel (runtime hooking, all OSes), Track L parallel (applied cryptography), Track M parallel (detection engineering + DFIR), Track N parallel (embedded USB device development — the build-half of Track F).

## Track A — security core
Foundations → xv6-riscv → exploit basics → Linux → Linux kernel exploits → sandbox escapes → XNU → iOS exploits/jailbreak → browser → chains

## Track B — Apple hardware
Swift + CHIP-8 → NES core → Metal (macOS) → iOS app

## Track C — Graphics programming
Concepts → CPU SIMD → Metal compute → memory/coalescing → software renderer (from scratch) → Metal graphics → shaders (MSL/GLSL/HLSL) → textures → async → profiling → Vulkan (MoltenVK) + DX12 stretch → capstone

## Track D — Windows kernel exploitation
Internals/WinDbg → HEVD RE → pool overflow/token stealing → real CVEs → mitigations (KVA shadow, CET, HVCI) → capstone

## Track E — Malware development
PE/APIs → injection/persistence → AMSI/ETW evasion → C2 implant → detection/defense → capstone → advanced in-memory (Sektor7-class)

## Track F — USB security
Stack/protocol → Linux USB driver dev → attack surface → fuzzing/device emulation → iOS/Windows links → capstone

## Track G — Cross-OS driver development
Linux driver craft → hardware-facing (MMIO/DMA/IRQ) → Windows KMDF → macOS DriverKit (dext) → cross-OS fuzzing → real USB device + userspace driver + app → capstone on all 3 OSes

## Track H — Android exploitation
Security model → app attack surface → Binder/kernel → real CVEs → root/bootloader/TEE → capstone chain

## Track I — Android malware development
APK/dex/smali → implant persistence → C2/evasion → fraud vectors → detection/defense → capstone implant

## Track J — Rootkit/bootkit (all OSes)
Principles → Linux LKM/eBPF → Windows driver rootkit → UEFI bootkit → macOS → mobile (iOS/Android) → capstone

## Track K — Runtime hooking (all OSes, from scratch)
Fundamentals → Windows → Linux → macOS → iOS → Android → capstone: your own engine on all (tools as cross-check only)

## Track L — Applied cryptography
Symmetric/modes (padding oracle) → RSA/ECC + attacks → hashes/MACs/side-channels → TLS internals → OS crypto stores (Keychain/SEP/Keystore/BitLocker) → capstone: break a deliberately-weak scheme

## Track M — Detection engineering + DFIR
Memory forensics → log analysis + threat hunting → Sigma/YARA as code → disk/artifact forensics → IR capstone (memory + disk + logs → root cause → detection rule). The blue-team counterweight to the offensive tracks.

## Track N — Embedded USB device development (keystroke-injection hardware)
USB device firmware + HID injection (TinyUSB on RP2040) → own DuckyScript engine → ESP32 WiFi C2 (softAP, web UI, exfil) → stealth/detection + USB-C hardware path → capstone: O.MG-cable clone; stretch: KiCad inline PCB. Builds the device Track F attacks.

---

## Phase 0 · Foundations (W1–3)
- C (K&R ch.1–6): pointers, structs, stack/heap
- *Hacking: The Art of Exploitation* — memory model + exploit mindset
- Git, Make, gdb/lldb
- Swift basics (*The Swift Programming Language*, swift.org)
- **Exit:** linked list in raw C; heap vs stack from memory

## Phase 1 · xv6-riscv — MIT 6.S081/6.1810 (W4–9)
- All labs: util, syscall, pgtbl (Sv39), traps, cow, thread, fs, **net**
- **net lab = your NIC driver goal** (E1000 driver in xv6-riscv: TX/RX, ARP, ICMP, ping)
- RISC-V assembly (*The RISC-V Reader*; MIT 6.004; riscv.org spec)
- Stretch: TCP echo on E1000; or virtio-net (xv6 has virtio-blk framework)
- **Exit:** syscall trace `ecall`→kernel→return; xv6 answers ping

## Phase 2 · Track B — emulator on macOS/iOS (W6–18, parallel)
- W6–10: CHIP-8 CLI in C (CPU/instructions)
- W8–14: NES core in portable C (CPU, PPU, APU) — OneLoneCoder, gbdev Pan Docs
- W12–16: macOS app — Swift + SwiftUI shell, **Metal MTKView renderer** (Kodeco *Metal by Tutorials*, WWDC sessions)
- W16–18: iOS app on device (free provisioning or jailbroken device)
- **Caveat:** iOS bans JIT → interpreter-only design (fine for NES; blocks future JIT systems)
- **Exit:** NES runs on iPhone via Metal

## Phase 3 · Exploitation fundamentals (W10–14)
- Buffer overflow, UAF, OOB, type confusion; ROP/JOP, shellcode
- Mitigations: ASLR, canaries, NX, SMEP/SMAP, PAC
- Practice: pwn.college; PwnFunction/LiveOverflow videos; **practice-platform ladder (W11–14, parallel): ROP Emporium (all 8) → pwnable.tw → exploit.education — the reps that make 03-02 fluent**
- **CTF cadence: one CTF per phase (~2/yr) — picoCTF, then HTB / ctftime archive; forces skills at real targets**
- **Exit:** pwn.college labs solo

## Phase 4 · Real kernels: Linux → XNU (W14–20)
- Linux: *Linux Kernel Development* + OSTEP; read `mm/`, `sched/`, `net/`; write a module; read `e1000_main.c`, `virtio_net.c`
- XNU: `apple-oss-distributions/xnu` + Jonathan Levin Vol 1–2 (Mach ports, vm_map, code signing/AMFI)
- Security: Apple *Platform Security Guide* — code signing, entitlements, sandbox, KTRR/PPL/PAC/APRR, SSV
- ARM64 basics (Azeria) — RISC-V transfers; ARM64 specifics for iOS later
- **Exit:** skb lifecycle from memory; KTRR/PPL/PAC attack matrix

## Phase 5 · Linux kernel exploitation ladder (W20–30)
Easy→hard; skill = re-derive writeups with POC closed.

| Lvl | Exploit | Teaches |
|---|---|---|
| 1 | modprobe_path | write-what-where → root, `cred` |
| 1 | ret2usr | return into ring0, `commit_creds` |
| 1 | Dirty COW (CVE-2016-5195) | racing kernel logic |
| 2 | Kernel ROP | SMEP/SMAP bypass |
| 2 | Dirty Pipe (CVE-2022-0847) | page-cache bug |
| 2 | Vermin (CVE-2021-22555) | netfilter OOB → `msg_msg` → ROP (flagship) |
| 3 | CVE-2022-0185 | kmalloc heap grooming |
| 3 | eBPF verifier (CVE-2017-16995 → CVE-2021-3490) | type confusion → bridge to browser JITs |
| 3 | CVE-2023-32233 | cross-cache UAF |

Sources: xairy/linux-kernel-exploitation, pwn.college kernel pwn, ctf-wiki.org, chuj.blog
Practice ladder (parallel, W20–30): Kernel-Exploit-Dojo — Easy challenges before each CVE step, Medium during, re-derive rule. See `10-kernel-exploit-dojo` step.
Fuzzing method (parallel, W26–30): general VR discipline — libFuzzer/AFL++ harness, crash triage (minimize/dedupe/root-cause), **syzkaller on XNU promoted up from Phase 9 optional into this step**. See `05-11`.
**Exit:** re-derive Vermin from scratch

## Phase 6 · Sandbox escape (W34–39, parallel with Phases 5/7)
- Linux: seccomp, namespaces; container escapes (runc CVEs, e.g. CVE-2019-5736)
- Apple: App Sandbox / Seatbelt profiles; macOS/iOS sandbox escape techniques
- VMM: hypervisor fundamentals; device-emulation escapes; guest-tools CVEs
- Stretch (W38–39): **write your own hypervisor once** (Hypervisor.framework on Apple Silicon or KVM) — the escapes then read like your own code. See `09-own-hypervisor`.
- **Exit:** container escape re-derived (**S1**); VM escape re-derived (**S2**)

## Phase 7 · XNU/iOS exploitation = jailbreak (W35–46)
- checkm8 → machswap/machswap2 (tfp0) → Trident (Quarkslab) → oobPCI (CVE-2022-46689) → KFD (pattern-f) → Fugu15/Landa (PAC)
- ARM64e + PAC deep-dive
- Read Dopamine source: `libjailbreak` → `launchdhook` → `systemhook` → `ChOma`
- **Exit:** KFD re-derived; Dopamine explained end-to-end

## Phase 8 · Browser exploitation (W47+, open-ended)
- saelo.github.io JS engine exploitation series
- Project Zero browser bugs; doyensec course; d8/V8 CTF
- Reuses eBPF type-confusion skills
- **Exit:** exploit known CVE in sandboxed d8

## Phase 9 · Real-world chains (capstone)
- ForcedEntry (P0), Pegasus (Quarkslab), Pwn2Own Mobile
- Optional: original research (syzkaller on XNU)

## Phase 10 · Graphics programming — Track C (W12–28, parallel)
- W12–13: GPU architecture concepts (SIMT, warps, occupancy, memory hierarchy; Kirk/Hwu)
- W13–15: CPU SIMD — SSE/AVX/NEON intrinsics, autovectorization, SoA (Apple Silicon = NEON; x86 VM for AVX)
- W15–16: Metal compute kernels (MSL, dispatch, grid-stride, reduction)
- W16–17: memory — coalescing, threadgroup memory, tiled transpose
- W17–19: software renderer from scratch (tinyrenderer-style: rasterizer, zbuffer, Phong, textures — no GPU)
- W19–20: Metal graphics pipeline deep dive (extends Track B MTKView step)
- W20–22: shader programming — same effect in MSL + GLSL + HLSL; shader perf (occupancy, divergence)
- W22–23: textures, samplers, mipmaps
- W23–24: async, multiple command buffers, fences
- W24–25: profiling — Xcode Metal HUD, GPU frame capture
- W25–27: cross-API — Vulkan via MoltenVK (port the Metal renderer); DX12 stretch on the Windows VM (W30+)
- W27–28: capstone — N-body **or** renderer (software rasterizer + Metal + Vulkan ports)
- **Exit:** **M11** — chosen capstone at target (N-body FPS or 3-way renderer)

## Phase 11 · Windows kernel exploitation — Track D (W29–44, parallel)
- W29–30: **Windows userland exploitation first** (the missing discipline — Phase 3 was Linux, Track D is kernel): SEH overwrite, egg hunters, Windows ROP, CFG/ACG/CET — on OSED-style binaries in the VM. See `11-00-userland-exploit-dev`.
- W30–31: Windows internals + WinDbg (IRQL, pool, `_EPROCESS`/token); userland privesc ladder — 20 techniques, non-admin → SYSTEM [Sektor7 Privesc course]
- W31–33: HEVD RE (Ghidra/IDA; IOCTL dispatch; find all bug classes)
- W33–36: pool overflow → token stealing; SMEP/SMAP
- W36–40: real CVEs (CVE-2021-21551, CVE-2021-31956, BYOVD class)
- W40–43: KVA shadow, CET, HVCI; ROP, ret2dir, data-only
- W43–44: capstone — HEVD re-derived + 1 real CVE variant
- **Exit:** **M12** — 2 HEVD classes + 1 real CVE from scratch

## Phase 12 · Malware development — Track E (W18–30, parallel)
- W18–19: PE format + Windows APIs (write your own PE parser)
- W19–21: injection (3×) + persistence (3× + deep set: WMI, COM, IFEO, AppInit)
- W21–23: AV/EDR evasion — encrypted shellcode, AMSI, ETW, unhooking, direct syscalls; sandbox/VM detection
- W23–25: C2 + implant (beacon, sleep obfuscation, Sliver)
- W25–26: detection/defense — Sysmon, Sigma rules against your own implants
- W26–28: capstone — full implant, survives its own detections
- W28–30: advanced in-memory + evasion (Sektor7-class) — reflective PE/DLL + COFF loaders, 32/64-bit migration, hidden storage, exception-driven control flow, custom RPC-like tasking, userland process hiding
- **Exit:** **M13** — implant passes its own Sigma rules end-to-end

## Phase 13 · USB security — Track F (W24–32, parallel)
- W24–25: USB protocol + host stack — enumerate a device from wire capture
- W25–26: Linux USB driver dev — write a driver (probe/URB), gadget framework
- W26–27: attack surface — BadUSB, descriptor confusion, usbfs access control, USB CVEs; defense per class
- W27–29: fuzzing + device emulation — act as malicious device in VM, trigger a real driver bug
- W29–30: iOS + Windows USB links — checkm8 (DFU bootrom bug), Windows USB driver bug classes
- W30–32: capstone — USB attack + its own detection rules (Sysmon/Sigma/macOS logs)
- **Exit:** **M14** — attack works end-to-end and own detections catch it

## Phase 14 · Cross-OS driver development — Track G (W28–41, parallel)
- W28–29: Linux driver craft — char device, ioctl, wait queues (beyond Phase 4's module)
- W29–31: hardware-facing — MMIO, DMA, IRQ; platform/PCI driver against a virtual device
- W31–33: Windows KMDF/WDM — device objects, IRPs, IOCTL dispatch, WinDbg; the write-half of Track D's HEVD bugs
- W33–35: macOS DriverKit (dext) — kexts are dead on Apple Silicon; IOUserClient methods
- W35–37: cross-OS patterns + fuzzing your own drivers (syzkaller / IOCTL harness / dext fuzz)
- W37–38: real USB device + userspace driver + app — Raspberry Pi Pico (TinyUSB) vendor-class device, IOKit + libusb userspace drivers, SwiftUI control app
- W38–41: capstone — one driver on Linux + Windows + macOS, driving your real 14-06 USB device; deliberate bug handed to Track D
- **Exit:** **M15** — working driver trio + cross-OS comparison

## Phase 15 · Reverse engineering (W16–20, parallel)
- W16–17: Ghidra/Binary Ninja — decompiler workflow, xrefs, function recovery
- W17–18: XNU kernelcache RE — `kcache_id`, `jtool`, `kerneldiff`; release kernels are unsymbolicated
- W18–19: practice — crackmes.one, Flare-On archive, microcorruption
- W19–20: patch-diffing CVE workflow — one fix commit per week, root cause from the patch alone
- W20: capstone — triage one real crash log → root cause chain with Ghidra walkthrough
- **Exit:** **M16** — crash → root cause chain from an unsymbolicated image

## Phase 16 · Android exploitation — Track H (W34–44, parallel)
- W34–35: Android security model + app internals (sandbox, SELinux, Binder, permissions)
- W35–37: app attack surface — exported components, WebView, drozer/Frida/objection
- W37–39: Binder + Android kernel surface; KASAN emulator kernel (Phase 5 skills transfer)
- W39–41: real CVEs re-derived — binder UAF (CVE-2019-2215), gpio (CVE-2020-0041), media
- W41–43: rooting, Magisk, bootloader/AVB, TrustZone/TEE
- W43–44: capstone — exploit chain (app vuln → kernel bug → root) on your own AVD/device
- **Exit:** **M17** — working chain + a detection rule for it

## Phase 17 · Android malware development — Track I (W40–48, parallel)
- W40–41: APK/dex/smali internals — hand-patch a method, rebuild, resign
- W41–43: implant core — persistence, AccessibilityService abuse, dynamic loading
- W43–45: C2 + evasion — Play Protect/MTK, anti-emulator/anti-debug (Track E concepts, Android mechanics)
- W45–46: fraud vectors — banking overlays, SMS intercept, ad fraud; one family kill-chain
- W46–47: detection/defense — YARA + network IoCs against your own implant
- W47–48: capstone — dropper+implant that evades, plus rules that catch it
- **Exit:** **M18** — implant + own detections

## Phase 18 · Rootkit/bootkit — Track J (W44–56, parallel)
- W44–45: rootkit principles — hide primitives, hook locations, trust boundaries per OS
- W45–47: Linux — LKM + eBPF rootkit (syscall/ftrace/seq_file hooks); detect with auditd/integrity checks
- W47–49: Windows — kernel callbacks, DKOM, minifilter (on your Track G KMDF); PatchGuard limits
- W49–51: UEFI bootkit — ESP/NVRAM persistence, Secure Boot bypass (BlackLotus/LoJax class); OVMF VM only
- W51–53: macOS — legacy kext + DriverKit-era dext rootkit, ESF, SSV limits
- W53–55: mobile — Android (kernel module + boot-image patch) and iOS (post-jailbreak + checkm8 bootkit)
- W55–56: capstone — one platform: bootkit persistence → rootkit → detection evasion + own rules
- **Exit:** **M19** — working chain + catching detections

## Phase 19 · Runtime hooking — Track K (W20–32, parallel) — **from scratch first; MinHook/Frida/LSPosed only as cross-check**
- W20–21: hooking fundamentals — hook classes, trampolines, trust boundary, detectability, what a from-scratch engine needs
- W21–23: Windows — build your own IAT patcher + x64 detour engine (trampoline, hook list, unhook); MinHook/Detours as comparison; feeds Track E AMSI/ETW
- W23–25: Linux — own LD_PRELOAD shim, own GOT rebinder (ELF relocations), own ptrace hook; eBPF/uprobes = kernel layer (you write the BPF, not the runtime)
- W25–27: macOS — own interpose lib, reimplement fishhook's symbol-pointer rebinding, own ObjC swizzler; Frida as cross-check
- W27–29: iOS — port your engine; in-app swizzle; minimal tweak where the hooking is yours (theos only builds/signs)
- W29–31: Android — own native GOT + ARM64 inline hook (trampoline); LSPosed/Frida stay for ART/Java-level (from-scratch ART hooking is research-grade)
- W31–32: capstone — your own engine on ≥5 platforms (same harness), Frida only to verify, memory-scanner detections
- **Exit:** **M20** — own from-scratch engine on ≥5 platforms + comparison

## Phase 20 · Applied cryptography — Track L (W14–20, parallel)
- W14–15: symmetric ciphers + modes — AES/ChaCha, ECB→GCM; **padding-oracle attack built by hand**; CryptoPals sets 1–2
- W15–16: RSA/ECC — math, textbook-RSA attacks (small e/d, Håstad, Wiener), ECDSA nonce-reuse key recovery; CryptoPals 5–6
- W16–17: hashes/MACs — length extension, HMAC, **timing side channels** (constant-time compare, attack a victim)
- W17–18: TLS 1.2/1.3 internals — handshake decoded from a live capture; Bleichenbacher/POODLE/CRIME/ALPACA history
- W18–19: OS crypto stores — Keychain/Data Protection/SEP, Android Keystore/StrongBox, DPAPI/BitLocker, LUKS/TPM — the "where keys live" map Phase 7/Track D/H consume
- W19–20: capstone — build a deliberately-weak multi-layer scheme, break it end-to-end, re-design it properly
- **Exit:** **M21** — weak scheme broken end-to-end

## Phase 21 · Detection engineering + DFIR — Track M (W30–36, parallel)
- W30–31: memory forensics — Volatility 3 (pslist/psscan/malfind/net); triage an incident image; see your Track E implant the way a defender does
- W31–32: log analysis + threat hunting — the hypothesis → source → query → validate loop; your own Dojo + implant as the hunt target
- W32–34: detection-as-code — production-quality Sigma + YARA, validation/conversion, FP engineering, adversary-in-the-loop tuning
- W34–35: disk/artifact forensics — timelines, persistence hunting (the Track J list, inverted), carving, evidence hygiene
- W35–36: capstone — one IR scenario across memory + disk + logs → root cause → shipping detection rule
- **Exit:** **M22** — full IR chain + detection rule

---

## Phase 22 · Embedded USB device development — Track N (W28–34, parallel, low priority)
- W28–29: USB device firmware + HID — Raspberry Pi Pico + TinyUSB; enumeration, HID keyboard descriptor, keystroke injection (pico-ducky-class); first time you're the *device*, not the host
- W30: DuckyScript engine — own parser → compiler → HID reports; layouts (US/ISO/JIS); corpus of public payloads runs on your engine
- W31–32: ESP32 port — WiFi softAP + HTTP web UI + payload store (LittleFS) + exfil; over-the-air control, no physical access (O.MG feature set)
- W33: stealth + hardware — how keystroke injection is detected (HID cadence, VID/PID, Sysmon), write the detections for your own device; USB-C CC/power, inline-board path, KiCad stretch
- W34: capstone — full chain on one device (plug → auto-run → exfil → UI) + defender round-trip + writeup
- **Exit:** **M23** — O.MG-class device works end-to-end; stretch: fabricated inline PCB

---

## Load control & priorities (W20+ crowded middle)
- **Priority: A (core) > C (graphics) > D (Windows) > G (feeds D) > H (Android) > K (feeds E) > F > E > I > B.** Pick one mobile platform primary; the other goes secondary (re-run later).
- **>2 concurrent tracks in a week → defer, don't grind.** Track J (W44–56) is the exception — most prior tracks are done; treat it as the graduation project.
- First cuts if behind: Track I (Android malware — Track E concepts transfer), Track B stretch (APU, extra mappers), and Track N (embedded USB — needs cheap hardware, no deadline pressure). Track K's desktop half (W20–27) feeds Track E — keep it; its mobile half can slip.
- W34–46 is the worst squeeze (iOS Phase 7 + Track D + Track H + sandbox + DFIR capstone): stagger capstones, lean on the defer rule.

---

## Combined timeline

| Weeks | Track A | Track B |
|---|---|---|
| 1–3 | Foundations + Swift | — |
| 4–9 | xv6-riscv (incl. net lab) | — |
| 6–10 | — | CHIP-8 CLI |
| 8–14 | — | NES core |
| 10–14 | Exploit fundamentals | — |
| 12–16 | — | Metal macOS app |
| 12–28 | — | Graphics (Track C) |
| 14–20 | Linux → XNU | — |
| 14–20 | — | Applied crypto (Track L) |
| 16–18 | — | iOS app |
| 16–20 | — | Reverse engineering |
| 18–30 | — | Malware dev (Track E) |
| 20–30 | Linux kernel exploits | — |
| 20–32 | — | Hooking (Track K) |
| 24–32 | — | USB security (Track F) |
| 26–30 | Fuzzing method (A, parallel) | — |
| 28–41 | — | Driver dev all OS (Track G) |
| 29–30 | — | Windows userland expl (Track D pre) |
| 30–36 | — | Detection/DFIR (Track M) |
| 28–34 | — | Embedded USB device dev (Track N) |
| 30–44 | — | Windows kernel (Track D) |
| 34–39 | Sandbox escapes (parallel) | — |
| 35–46 | iOS exploits / jailbreak / Dopamine | — |
| 34–44 | — | Android exploitation (Track H) |
| 40–48 | — | Android malware dev (Track I) |
| 44–56 | — | Rootkit/bootkit (Track J) |
| 47+ | Browser → chains | — |

**Total:** ~18 months focused / ~25 moderate to jailbreak+root literate (+ GPU/Windows/driver/Android/rootkit/hooking/crypto/DFIR tracks). Browser: open-ended.

## Checkpoints
- [ ] M1: trace xv6 syscall (W9)
- [ ] M2: xv6 answers ping (W9)
- [ ] M3: NES via Metal on macOS (W16)
- [ ] M4: NES on iPhone (W18)
- [ ] M5: KTRR/PPL/PAC matrix (W20)
- [ ] M6: pwn.college solo (W14)
- [ ] M7: Vermin re-derived (W26)
- [ ] S1: container escape re-derived (W37)
- [ ] S2: VM escape re-derived (W39)
- [ ] M8: KFD re-derived (W44)
- [ ] M9: Dopamine explained (W46)
- [ ] M10: d8 CVE exploited (open)
- [ ] M11: N-body or renderer at target (W28, Track C)
- [ ] M12: Windows — 2 HEVD classes + 1 real CVE (W44, Track D)
- [ ] M13: implant survives its own Sigma rules (W28, Track E)
- [ ] M14: USB attack + own detection rules (W32, Track F)
- [ ] M15: driver on Linux + Windows + macOS (W41, Track G)
- [ ] M16: crash log → root cause chain (W20, RE)
- [ ] M17: Android root from exploit chain (W44, Track H)
- [ ] M18: Android implant survives own detections (W48, Track I)
- [ ] M19: rootkit/bootkit chain + own detections (W56, Track J)
- [ ] M20: own from-scratch hook engine on ≥5 platforms (W32, Track K)
- [ ] M21: weak crypto scheme broken end-to-end (W20, Track L)
- [ ] M22: memory+disk+logs → root cause → detection rule (W36, Track M)
- [ ] M23: O.MG-clone cable works end-to-end (W34, Track N)

## Resources master list
- Books: K&R, *Art of Exploitation*, CS:APP, OSTEP, *Linux Kernel Development*, *TCP/IP Illustrated* V1, *RISC-V Reader*, *Mac OS X/iOS Internals* V1–2 (Levin), *Metal by Tutorials*, *Swift Programming Language*, *Programming Massively Parallel Processors* (Kirk/Hwu), *Windows Internals* (Russinovich), *Practical Malware Analysis* (Sikorski), *Practical Reverse Engineering* (Dang), *Practical Binary Analysis* (Andriesse), *Reverse Engineering for Beginners* (Yurichev), *Android Security Internals* (Elenkov), *Android Hacker's Handbook*, *The Android Malware Handbook* (Diogène), *Rootkits: Subverting the Windows Kernel* (Hoglund/Butler), *The Rootkit Arsenal* (Blunden), *Serious Cryptography* (Aumasson), *Cryptography Engineering* (Ferguson/Schneier), *The Art of Memory Forensics* (Ligh)
- Sites: MIT 6.S081, pwn.college, ROP Emporium, pwnable.tw, exploit.education, ctf-wiki.org, xairy list, Project Zero, Quarkslab, pattern-f, gbdev.io, OSDev wiki, saelo.github.io, Hacking with Swift, Apple Metal docs + WWDC sessions, "A Trip Through the Graphics Pipeline" (Giesen), HEVD (HackSysTeam), ired.team, maldevacademy.com, Sliver docs, loldrivers.io, Sigma repo, USB in a Nutshell, USB Made Simple, Linux kernel USB docs, usbguard, Facedancer/GreatFET, syzkaller USB, USBView/USBPcap, WDK/KMDF samples, Apple DriverKit docs + WWDC sessions, virtio spec, Ghidra, jtool/kcache_id/kerneldiff, crackmes.one, Flare-On archive, AOSP security docs + Android security bulletins, Frida, objection, drozer, apktool/jadx/smali, Play Protect, Mobile Threat Catalogue, BlackLotus analyses (ESET, SentinelOne), Diamorphine, UEFI spec, efi-mimikatz, QEMU/OVMF, MinHook/Detours, fishhook, LSPosed/Xposed, bhook/Dobby, LD_PRELOAD + dyld interpose docs, cryptopals.com, cryptohack.org, *The Fuzzing Book* (Zeller), Trail of Bits Fuzzing 101, Volatility 3 docs, corelan.be + FuzzySecurity (Windows userland expl), Apple Hypervisor.framework docs
- Code: xv6-riscv, torvalds/linux, apple-oss-distributions/xnu, felix-pb/kfd, opa334/Dopamine, checkra1n

