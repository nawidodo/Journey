# MASTER ROADMAP — Systems Security Engineer

Eighteen tracks. Track A sequential (security core), Track B parallel (hardware/Apple), Track C parallel (graphics: Metal + Vulkan/DX, shaders, SIMD, software renderer), Track D parallel (Windows kernel), Track E parallel (malware dev), Track F parallel (USB security), Track G parallel (cross-OS driver dev), Track H parallel (Android exploitation), Track I parallel (Android malware dev), Track J parallel (rootkit/bootkit, all OSes), Track K parallel (runtime hooking, all OSes), Track L parallel (applied cryptography), Track M parallel (detection engineering + DFIR), Track N parallel (embedded USB device development — the build-half of Track F), Track O (career — post-plan professional completion, Phase 23), Track Q parallel (privilege escalation tool internals, all OSes — Phase 25), Track R parallel (Bluetooth device development — Phase 26), Track S parallel (C2 framework development — Phase 27), Track T parallel (Wi-Fi security — Phase 28, low priority).

## Track A — security core
Foundations → xv6-riscv → exploit basics → Linux → Linux kernel exploits → sandbox escapes → XNU → iOS exploits/jailbreak → browser → chains

## Track B — Apple hardware
Swift + CHIP-8 → NES core → Metal (macOS) → iOS app → stretch: Unicorn CPU emulation as security tool (09).

## Track C — Graphics programming
Concepts → CPU SIMD → Metal compute → memory/coalescing → software renderer (from scratch) → Metal graphics → shaders (MSL/GLSL/HLSL) → textures → async → profiling → Vulkan (MoltenVK) + DX12 stretch → capstone

## Track D — Windows kernel exploitation
Internals/WinDbg → HEVD RE → pool overflow/token stealing → real CVEs → mitigations (KVA shadow, CET, HVCI) → capstone

## Track E — Malware development
PE/APIs → injection/persistence → AMSI/ETW evasion → C2 implant → detection/defense → capstone → advanced in-memory (Sektor7-class)

## Track F — USB security
Stack/protocol → Linux USB driver dev → attack surface → fuzzing/device emulation → iOS/Windows links → capstone

## Track G — Cross-OS driver development
Linux driver craft → hardware-facing (MMIO/DMA/IRQ) → Windows KMDF → macOS DriverKit (dext) → cross-OS fuzzing → real USB device + userspace driver + app → gamepad turbo on an existing controller (hidraw/uinput · ViGEm · IOHIDUserDevice, kernel filter stretch) → capstone on all 3 OSes

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

## Track N — Embedded USB device development (attack-USB hardware)
USB device firmware + HID injection (TinyUSB on RP2040) → own DuckyScript engine → ESP32 WiFi C2 (softAP, web UI, exfil) → stealth/detection + USB-C hardware path → network-adapter pivot (WiFi client→AP bridge, LAN-Turtle mode) → advanced firmware (composite USB, exfil queue, OTA, self-destruct) → fault injection/glitching stretch → capstone: plugandpwn/O.MG-class device; stretch: KiCad inline PCB. Builds the device Track F attacks.

## Track O — Career (post-M23, Phase 23, no week load)
Writeups/portfolio → resume/interview → coordinated disclosure → career-launch capstone (M24). The job-conversion half: publication of labs, diligence on the disclosure process, mock interviews. Runs after the technical plan, so it competes for zero study weeks.

## Track T — Wi-Fi security (Phase 28, low priority, defer rule applies)
802.11 stack + monitor mode → WPA2/WPA3 attack classes (PMKID, KRACK, Dragonblood) → enterprise EAP relay + captive portal → rogue-AP red-team hosting + MITM + pivot → WIDS/detection mirror → capstone: full engagement + own detection round-trip. The radio twin of Track N (wired pivots) and Track R (air discipline); feeds Track M Wi-Fi detection rules. Own lab airspace only — attack only APs you own.

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
CHIP-8 CLI → NES core (6502 + PPU + APU + mappers) → Metal renderer → iOS app (JIT blocked — interpreter-only). Stretch W18+: CPU emulation as a security tool — Unicorn Engine: shellcode tracing, unpacking, angr backend, unicorn fuzzing harnesses (cross-ref 15-06, 12-03, 05-11). See `09-unicorn-cpu-emulation`.
- W18+ (stretch): GBA emulator — ARM7TDMI core (ARM+THUMB), PPU tilemaps/affine/OBJ, APU, cycle-accurate timing; passes AGB test ROMs + one commercial game to attract mode. See `10-gba-emulator`.
- W18+ (stretch): RISC-V emulator — rv32ima from the spec, bare-metal firmware boots, GDB stub (pairs 15-07 protocol), debug 22-09-class firmware with it. See `11-riscv-emulator`.
- W18+ (stretch): SNES emulator — 65C816 (extend own 6502 core), Mode 7 math, SPC700 audio coprocessor (second CPU lesson). See `12-snes-emulator`.
- W18+ (stretch): Genesis emulator — 68000 (new CISC ISA) + Z80 coprocessor, planar VDP, FM audio. See `13-genesis-emulator`.
- W18+ (stretch): PS1 emulator — MIPS R3000 (delay slots, coprocessors), GTE geometry unit, GPU; the MIPS half pairs 22-09 MIPS firmware RE (write the cross-note). See `14-ps1-emulator`.
- W18+ (stretch): PSP emulator — Allegrex (MIPS R4000-family, extends the 02-14 core), VFPU vector unit, Media Engine dual-core, closed-doc RE (leaked SDK vs GBATEK era). The single-person ceiling; PS2/PS3+ out of scope. See `15-psp-emulator`.
- W18+ (stretch): emulator shaders — NES/SNES PPU rewritten as Metal compute (bit-identical to CPU PPU on test ROMs) + CRT shader pack (scanlines, aperture grille, phosphor). Emulator×GPU crossover. See `16-emulator-shaders`.
- W18+ (stretch): own WebAssembly runtime — decoder/validator → stack-machine interpreter → x86-64 JIT, WASI-style host ABI; break your own sandbox (OOB, stack overrun, host-call confusion). Pairs 08 browser track. See `17-wasm-runtime`.
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
- W14–16 (parallel): network recon — nmap/RustScan, Amass/CT logs/passive DNS, Shodan/Censys surface, scan-detection awareness (pairs 21-07). The pre-exploit half Phase 9's chains assume. See `07-network-recon`.
- **CTF cadence: one CTF per phase (~2/yr) — picoCTF, then HTB / ctftime archive; forces skills at real targets**
- **Exit:** pwn.college labs solo

## Phase 4 · Real kernels: Linux → XNU (W14–20)
- Linux: *Linux Kernel Development* + OSTEP; read `mm/`, `sched/`, `net/`; write a module; read `e1000_main.c`, `virtio_net.c`
- XNU: `apple-oss-distributions/xnu` + Jonathan Levin Vol 1–2 (Mach ports, vm_map, code signing/AMFI)
- Security: Apple *Platform Security Guide* — code signing, entitlements, sandbox, KTRR/PPL/PAC/APRR, SSV
- ARM64 basics (Azeria) — RISC-V transfers; ARM64 specifics for iOS later
- Rust for systems programming (`04-08`, W14–20 parallel): ownership/borrowing/lifetimes, `unsafe` rules and audit, reimplement one Phase 1/4 kernel-adjacent structure in Rust, read the Linux kernel `rust/` subsystem. The memory-safety half of modern kernel work (Linux/Android/Windows all moving).
- W14–20 (parallel): FreeBSD deep dive (`09-freebsd-deep-dive`) — jails (container ancestor), ZFS (snapshot forensics), pf, DTrace (the probe model behind 05-14 eBPF); jail-breakout attempt + ZFS-rollback labs on own VM. Closes the last popular-OS gap.
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
- W26–30 (parallel, stretch): build your own coverage-guided fuzzer — forkserver + shared-memory coverage bitmap + mutator/queue; rediscover a bug libFuzzer found in 05-11 with your own engine. See `12-own-coverage-fuzzer`.
- W20–30 (parallel, stretch): custom QEMU device model — virtio-class MMIO/IRQ/DMA device, own kernel module driver, DMA round-trip; fuzz the device with your own 05-12 fuzzer (intentionally buggy device, found + minimized). See `13-qemu-device-model`.
- W20–30 (parallel, stretch): own eBPF tool — CO-RE/BTF tracer (exec/open/connect → BPF ringbuf → userspace) feeding your 21-06 mini-EDR as telemetry source; one C2-beacon exec alert end-to-end. See `14-own-ebpf-tool`.
- W20–30 (parallel, stretch): own Spectre PoC — mistrained branch + speculative read + cache-timing leak of your own buffer; lfence/masking fix kills leak; where the CPU security boundary really is. See `16-own-spectre-poc`.
**Exit:** re-derive Vermin from scratch

## Phase 6 · Sandbox escape (W34–39, parallel with Phases 5/7)
- Linux: seccomp, namespaces; container escapes (runc CVEs, e.g. CVE-2019-5736)
- Apple: App Sandbox / Seatbelt profiles; macOS/iOS sandbox escape techniques
- VMM: hypervisor fundamentals; device-emulation escapes; guest-tools CVEs
- Stretch (W38–39): **write your own hypervisor once** (Hypervisor.framework on Apple Silicon or KVM) — the escapes then read like your own code. See `09-own-hypervisor`.
- W39 (stretch): build a seccomp sandbox — filter DSL → BPF, install on a target, then escape your own filter (pairs 06-02) and close the hole. Both sides of the boundary in one artifact. See `10-seccomp-sandbox-builder`.
- W30–36 (stretch): own malware-analysis sandbox — seccomp + ptrace syscall tracing + tmpfs/network shadowing; detonate own 12-06 implant + 27-06 beacon → flagged, benign → clean; Sigma export. See `11-own-malware-sandbox`.
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
- Optional `08-04`: **JSC/WebKit exploitation** — JavaScriptCore GC/JIT tiers, one known JSC CVE end to end. The engine behind ForcedEntry/Trident/Pegasus; closes the V8→WebKit gap before Phase 9.
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
- W28 (stretch): GPU path tracer — Metal compute ray tracer: spheres → BVH → materials → denoise; converges to CPU reference. The capstone's opposite. See `13-path-tracer`.
- W28+ (stretch): CUDA for AI compute — the era API from a Metal base: thread/block/grid → memory model → warp-shuffle reduction → PyTorch custom kernel (GELU/softmax) in a real training loop; run free Colab T4; Metal/CUDA comparison table. See `14-cuda-ai-compute`.
- W28+ (stretch): own neural network — forward/backprop/SGD from scratch, MNIST ~97% (no frameworks), SIMD/GPU speedup, FGSM adversarial attack + tiny defense on your own net. See `15-own-neural-network`.
- W28+ (stretch): own model quantization — int8 PTQ on your 10-15 net (scale/zero-point, SIMD GEMM, requantization), accuracy/speed table, outlier writeup. See `16-own-model-quantization`.
- **Exit:** **M11** — chosen capstone at target (N-body FPS or 3-way renderer)

## Phase 11 · Windows kernel exploitation — Track D (W29–44, parallel)
- W29–30: **Windows userland exploitation first** (the missing discipline — Phase 3 was Linux, Track D is kernel): SEH overwrite, egg hunters, Windows ROP, CFG/ACG/CET — on OSED-style binaries in the VM. See `11-01-userland-exploit-dev`.
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
- W30 (stretch): anti-forensics — timestomping ($MFT SI vs FN), ShimCache/Amcache/Prefetch/SRUM shimming, event-log gaps, browser history laundering; before/after artifact comparison; the disk mirror of Track M 04. See `08-anti-forensics`.
- W30 (stretch): build a mini-loader — PE parse → map → imports → relocations → entry; reflective variant into a live process; ELF second pass. The 12-07 primitive from scratch. See `09-mini-loader`.
- W30 (stretch): build your own packer — encrypt sections, assembly stub decryptor, new entry; entropy measurement + low-entropy mode; detection round-trip against your own 12-03 checks (own VM). See `10-own-packer`.
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
- W36–38: gamepad turbo — attach to an *existing* USB controller (Xbox-style HID): HID report parsing, turbo state machine (button hold → N Hz auto-fire), per-OS interception + virtual-device injection (Linux hidraw+uinput / Windows Raw Input+ViGEmBus / macOS IOHIDManager+IOHIDUserDevice); kernel filter stretch (Linux HID driver / KMDF HID filter); macOS is userspace-only by design (no third-party kexts on Apple Silicon)
- W38–41: capstone — one driver on Linux + Windows + macOS, driving your real 14-06 USB device; deliberate bug handed to Track D
- **Exit:** **M15** — working driver trio + cross-OS comparison

## Phase 15 · Reverse engineering (W16–20, parallel)
- W16–17: Ghidra/Binary Ninja — decompiler workflow, xrefs, function recovery
- W17–18: XNU kernelcache RE — `kcache_id`, `jtool`, `kerneldiff`; release kernels are unsymbolicated
- W18–19: practice — crackmes.one, Flare-On archive, microcorruption
- W19–20: patch-diffing CVE workflow — one fix commit per week, root cause from the patch alone
- W20: capstone — triage one real crash log → root cause chain with Ghidra walkthrough
- W19–20 (parallel, stretch): symbolic execution — angr/KLEE/Triton; 3 angr challenge solves, one Triton deobfuscation, KLEE vs libFuzzer comparison. The concolic half of the fuzzing story (05-11). See `06-symbolic-execution`.
- W20 (parallel, stretch): build a mini-debugger — ptrace, int3 breakpoints, single-step, regs/mem dump; reproduce 04-01's overflow under your own debugger. See `07-mini-debugger`.
- W20 (parallel, stretch): build a mini-disassembler — one arch from the manual (x86-64 REX/ModRM/SIB or ARM64 subset), validated against objdump on ≥20 binaries. See `08-mini-disassembler`.
- W20 (parallel, stretch): build a mini-decompiler — stack-frame recovery, CFG structuring (if/while/switch from dominators), usage-based type inference; decompile your own 24-09-compiled functions and diff vs source. See `09-own-decompiler`.
- **Exit:** **M16** — crash → root cause chain from an unsymbolicated image

## Phase 16 · Android exploitation — Track H (W34–44, parallel)
- W34–35: Android security model + app internals (sandbox, SELinux, Binder, permissions)
- W35–37: app attack surface — exported components, WebView, drozer/Frida/objection
- W37–39: Binder + Android kernel surface; KASAN emulator kernel (Phase 5 skills transfer)
- W39–41: real CVEs re-derived — binder UAF (CVE-2019-2215), gpio (CVE-2020-0041), media
- W41–43: rooting, Magisk, bootloader/AVB, TrustZone/TEE
- W43–44: capstone — exploit chain (app vuln → kernel bug → root) on your own AVD/device
- W44+ (stretch): TEE lab (`07-tee-op-tee-lab`) — OP-TEE on QEMU, own trusted app via SMC path; TA fuzz lab shows secure-world crash vs Android survival; SEP comparison writeup.
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
- W20 (stretch): build a mini-TLS — record layer (AEAD vs MAC-then-encrypt), TLS 1.3 handshake (ECDHE, cert verify, transcript hash), interop vs `openssl s_client` both directions + tamper test. See `07-own-tls`.
- W20+ (stretch, pairs 10-14): GPU password cracker — PBKDF2 + bcrypt kernels in CUDA (Colab T4) + Metal; benchmark vs hashcat, why bcrypt is GPU-hostile by design. See `08-gpu-password-cracker`.
- W20 (stretch): ML-KEM (Kyber) from FIPS 203 — NTT, PKE → KEM (FO transform), pass NIST test vectors, interop openssl; harvest-now-decrypt-later writeup. See `09-ml-kem-kyber`.
- W20 (stretch): toy zk-SNARK — R1CS → QAP → KZG commitment → pairing check; prove one statement, reject tampered proofs. See `10-toy-zk-snark`.
- W20 (stretch): own E2EE messaging — X3DH prekey bundles + Double Ratchet (forward secrecy, post-compromise recovery), two local peers; compromise simulation + tamper/replay rejection. See `11-own-e2ee-messaging`.
- W20 (stretch): own password manager — XChaCha20-Poly1305 vault, Argon2id master-key derivation (memory-hard, from 20-08), zeroization + clipboard auto-clear; offline-attack lab vs own 20-08 GPU kernels. See `12-own-password-manager`.
- W20 (stretch): own cache side-channel lab — Flush+Reload / Prime+Probe vs your own non-constant-time compare + AES S-box; recover secret, then constant-time fix flattens timing. See `13-own-cache-side-channel`.
- **Exit:** **M21** — weak scheme broken end-to-end

## Phase 21 · Detection engineering + DFIR — Track M (W30–36, parallel)
- W30–31: memory forensics — Volatility 3 (pslist/psscan/malfind/net); triage an incident image; see your Track E implant the way a defender does
- W31–32: log analysis + threat hunting — the hypothesis → source → query → validate loop; your own Dojo + implant as the hunt target
- W32–34: detection-as-code — production-quality Sigma + YARA, validation/conversion, FP engineering, adversary-in-the-loop tuning
- W34–35: disk/artifact forensics — timelines, persistence hunting (the Track J list, inverted), carving, evidence hygiene
- W35–36: capstone — one IR scenario across memory + disk + logs → root cause → shipping detection rule
- W36+ (stretch): own mini-EDR — invert Track J's primitives into telemetry (ETW/kernel callbacks/minifilter, auditd/eBPF, macOS ESF) + own YARA memory scanner; your own Track E/J artifacts in a VM must alert on ≥2 OSes. See `06-own-mini-edr` (**M29**).
- W36+ (stretch): network detection — Zeek scripts + Suricata rules on the wire; detect your own Track S beacon and 28-04 rogue AP; the host-vs-network coverage table. See `07-network-detection-zeek`.
- W36+ (stretch): honeypots & deception — Kippo/Cowrie + web honeypot, one attacker session mined into Sigma + Zeek/Suricata rules; the intel loop closed. See `08-honeypots`.
- **Exit:** **M22** — full IR chain + detection rule

---

## Phase 22 · Embedded USB device development — Track N (W28–36, parallel, low priority)
- W28–29: USB device firmware + HID — Raspberry Pi Pico + TinyUSB; enumeration, HID keyboard descriptor, keystroke injection (pico-ducky-class); first time you're the *device*, not the host
- W30: DuckyScript engine — own parser → compiler → HID reports; layouts (US/ISO/JIS); corpus of public payloads runs on your engine
- W31–32: ESP32 port — WiFi softAP + HTTP web UI + payload store (LittleFS) + exfil; over-the-air control, no physical access (O.MG feature set)
- W33: stealth + hardware — how keystroke injection is detected (HID cadence, VID/PID, Sysmon), write the detections for your own device; USB-C CC/power, inline-board path, KiCad stretch
- W34: network-adapter pivot — WiFi client joins victim network + attacker AP with NAT bridging (LAN-Turtle mode); exfil over victim network; pivot + HID in one plug-in
- W35: advanced firmware — composite USB (HID + CDC + vendor), exfil queue w/ retry, multi-payload + encrypted config, signed OTA, self-destruct/panic (plugandpwn feature set)
- W36 (parallel, **stretch**): fault injection/glitching (`07-fault-injection-glitching`) — voltage/EM glitch basics, one working glitch on owned hardware (chipWhisperer-class or Pico DIY), TEE/firmware bypass theory; the hardware complement to checkm8. Costs money, cut first if budget is tight.
- W36: capstone — full chain on one device (plug → auto-run → exfil over AP *and* victim net → pivot → UI) + defender round-trip + writeup
- W36+ (stretch): embedded firmware exploitation — extract router-class firmware (binwalk/squashfs), QEMU-emulate, ROP chain on ARM32/MIPS (no ASLR/NX); own hardware only. See `09-embedded-firmware-exploitation`.
- W36+ (stretch): embedded signals — mic → analog front-end → I²S ADC → FFT/band-pass DSP → log on ESP32; optical laser-pickup stretch (transimpedance amp + demodulate). The analog/DSP layer the plan lacked. See `10-embedded-signals-dsp`.
- W36+ (stretch): DIY radio gadgets — sub-GHz keyfob replay (CC1101), RFID/NFC cloner (PN532, crypto-1 reverse), ultrasonic covert channel (2×ESP32, FFT decode). New radio domains; pairs 22-10 + Track L. See `11-diy-radio-gadgets`.
- W36+ (stretch): DIY CAN bus injector — ESP32 + CAN transceiver, sniff own car (parked), ID map, replay/inject verified frames; why CAN has no sender auth. Own vehicle only. See `12-can-bus-injection`.
- W28–36 (stretch): own RTOS scheduler (`13-own-rtos-scheduler`) — TCBs, priority preemption, semaphores, SysTick/PendSV context switch on QEMU ARM + ESP32; TCB-corruption attack lab → canaries + MPU region.
- **Exit:** **M23** — plugandpwn/O.MG-class device works end-to-end; stretch: fabricated inline PCB

---

## Phase 25 · Privilege escalation tool internals — Track Q (W20+, parallel)
Privesc is the payoff of every exploit phase, and its tools are usually black boxes. This track opens them, all OSes: boundary model (step 01) → linpeas/pspy internals + own mini-scanner (02) → SUID/caps/sudo mechanics (03) → cron/systemd/container/NFS classes (04) → Windows token model (05) → mimikatz from source (06) → potato family: Rogue Potato/PrintSpoofer/GodPotato (07) → WinPEAS/PowerUp/UAC/CVEs (08) → AD: BloodHound/Kerberoast/delegation/impacket (09) → macOS TCC/SIP/task_for_pid/launchd (10) → Android Magisk + iOS jailbreak-as-privesc (11) → capstone: cross-OS triage tool + tool-internals wiki + re-derive mimikatz & Rogue Potato (12).
- **Exit:** **M25** — cross-OS triage tool + ≥10-tool internals wiki + re-derives pass (writeup closed)
- Pairs with Phase 3 (exploit fundamentals), 5 (kernel ladder), 11 (Track D — service misconfigs feed HEVD), 12 (Track E), 16 (Track H), and feeds Track M detection round-trips. Defer rule applies (>2 concurrent tracks).

---

## Phase 26 · Bluetooth device development — Track R (W36–48, parallel, low priority)
Build real Bluetooth devices end to end. Stack theory BR/EDR vs BLE (01) → first BLE peripheral on nRF52840/ESP32-C3, poked from Linux/macOS/Windows/Android (02) → HID-over-GATT gamepad — real hardware for the Track G 14-07 turbo idea (03) → host-side + HCI/air sniffing of your own devices (04) → classic: A2DP audio + SPP serial (05) → pairing/security attacks on own hardware + detection mirror (06) → capstone: BLE HID turbo gamepad with hardware turbo, secure pairing, OTA, on ≥2 OSes (07).
- **Exit:** **M26** — working BLE HID turbo gamepad + security review + sniffed handoff to Track M
- Pairs with Track G (14-07 report map + turbo core), Phase 7 (CoreBluetooth), Track N (embedded build practice), Track F (protocol/sniffing discipline). Low priority; defer under the >2-tracks rule.

---

## Phase 27 · C2 framework development — Track S (W30–38, parallel)
Build your own command-and-control framework, not a Sliver config. Architecture + dissect real C2s (01) → your own wire protocol (framing, AES-GCM, key exchange) + HTTP/S beacon and listener (02) → cross-platform Go agent: tasking, in-memory-only, sleep obfuscation, kill switch (03) → teamserver + operator UI, multi-operator, audit log (04) → DNS / SMB-pipe / WebSocket transports + redirectors, failover (05) → capstone: full op on own VMs then detection round-trip — write the Sigma/YARA/Sysmon rules that catch your own beacon, fix, re-run (06).
- **Exit:** **M27** — own C2 survives its own detection rules, full-op runbook + before/after data
- Prerequisite: Track E core (12-01–04, W18–30) — this phase is the "your own" path Track E 04 points at. Feeds Track M detection targets; pairs Track D (SMB pipes). Lab-only per safety rules.

---

## Phase 28 · Wi-Fi security — Track T (W28–40, parallel, low priority)
The radio surface Track F (USB) and Track R (BT) never covered — 802.11 is a bigger real-world target than either. Monitor mode + one USB adapter is the only hardware cost. Stack theory + capture (01) → WPA2/WPA3 attacks: PMKID/handshake crack, KRACK, Dragonblood downgrades (02) → enterprise: EAP relay (hostapd-wpe/EAPhammer), captive portal, probe-triggered evil twin (03) → rogue-AP red-team hosting: hostapd+dnsmasq+iptables NAT, MITM, Wi-Fi→wired pivot (04) → detection mirror: WIDS/clock-skew/rogue-AP classes, own Sigma/Suricata air rules (05) → capstone: full engagement + detection round-trip (06).
- **Exit:** **M28** — rogue AP + client compromise + pivot on own lab, caught by own detection rules (before/after data)
- Pairs Track N 05 (network-adapter pivot — the wired twin), Track R (air/sniffing discipline), Track M (detection round-trip). Low priority; defer under the >2-tracks rule. Own lab airspace only.

## Load control & priorities (W20+ crowded middle)
- **Priority: A (core) > C (graphics) > D (Windows) > G (feeds D) > H (Android) > K (feeds E) > F > E > I > B > T (Wi-Fi, low).** Pick one mobile platform primary; the other goes secondary (re-run later).
- **>2 concurrent tracks in a week → defer, don't grind.** Track J (W44–56) is the exception — most prior tracks are done; treat it as the graduation project.
- First cuts if behind: Track I (Android malware — Track E concepts transfer), Track B stretch (APU, extra mappers), and Track N (embedded USB — needs cheap hardware, no deadline pressure), incl. the 22-07 fault-injection stretch (hardware cost). Track T (Wi-Fi, Phase 28) is low priority by design — cut before T. Track K's desktop half (W20–27) feeds Track E — keep it; its mobile half can slip. Track O (career, Phase 23) runs post-M23 — zero load on the study window.
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
| 14–20 | Linux → XNU + Rust (08) | — |
| 14–20 | — | Applied crypto (Track L) |
| 14–16 | — | Network recon (04-07) |
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
| 28–36 | — | Embedded USB device dev + fault injection stretch (Track N) |
| 30–44 | — | Windows kernel (Track D) |
| 34–39 | Sandbox escapes (parallel) | — |
| 35–46 | iOS exploits / jailbreak / Dopamine | — |
| 34–44 | — | Android exploitation (Track H) |
| 40–48 | — | Android malware dev (Track I) |
| 44–56 | — | Rootkit/bootkit (Track J) |
| 30–38 | — | C2 framework (Track S) |
| 36–48 | — | Bluetooth device dev (Track R) |
| 28–40 | — | Wi-Fi security (Track T, low priority) |
| 47+ | Browser → chains | — |
| post-M23 | — | Career: portfolio/disclosure/interviews (Track O) |

**Total:** ~18 months focused / ~25 moderate to jailbreak+root literate (+ GPU/Windows/driver/Android/rootkit/hooking/crypto/DFIR tracks). Browser: open-ended. Phase 23 (career) runs after — the plan ends when skills convert, not when the labs do. Make-Your-Own (Phase 24, Track P) is optional W46+ — run only if other tracks are ahead (stretch adds: own C compiler 09, own RISC-V microkernel on your 02-11 emulator 10, own SQLite file format 11, own SSH server 12, own container runtime 13, own terminal emulator 14, own filesystem 15, own memory allocator 16, own HTTP server 17, own WireGuard-lite 18, own DNS resolver 19, own packet sniffer/crafter 20, own BGP speaker 21, own onion router 22, own QUIC-lite 23, own NAT/router 24, own compression 25, own LSM-tree 26, own PNG decoder 27, own quantum simulator 28, own Kademlia DHT 29, own sampling profiler 30, own bootloader 31, own NTFS parser 32, own APFS parser 33, own mail server 34, own JPEG decoder 35, own BitTorrent client 36, own chess engine 37, own text editor 38, own package manager 39, own audio synth 40, own search engine 41, own key-value cache 42, systems gauntlet 43, own TPM lab 44, own registry parser 45, own MQTT broker 46, own JS interpreter 47, own orchestrator 48, own font renderer 49, own logic analyzer 50, own time-series DB 51, own message queue 52). Track Q (privesc, W20+) and Track R (BT devices, W36+) are parallel add-ons — defer under the >2-concurrent-tracks rule.

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
- [ ] M23: plugandpwn/O.MG-class attack-USB device works end-to-end (W36, Track N)
- [ ] M24: career launch — portfolio live + ≥1 disclosed advisory + resume v2 (post-M23, Track O)
- [ ] M25: cross-OS privesc triage tool + ≥10-tool internals wiki + mimikatz/potato re-derived (W20+ parallel, Track Q)
- [ ] M26: BLE HID turbo gamepad — hardware turbo + secure pairing + OTA, ≥2 OSes (W46, Track R)
- [ ] M27: own C2 framework survives its own detection rules — multi-transport, full op, before/after (W38, Track S)
- [ ] M28: Wi-Fi capstone — rogue AP + client compromise + pivot on own lab, caught by own detection rules (W40, Track T)
- [ ] M29: own mini-EDR detects its own implant/rootkit artifact on ≥2 OSes, FP-engineered (W36+, Track M)

## Resources master list
- Books: K&R, *Art of Exploitation*, CS:APP, OSTEP, *Linux Kernel Development*, *TCP/IP Illustrated* V1, *RISC-V Reader*, *Mac OS X/iOS Internals* V1–2 (Levin), *Metal by Tutorials*, *Swift Programming Language*, *Programming Massively Parallel Processors* (Kirk/Hwu), *Windows Internals* (Russinovich), *Practical Malware Analysis* (Sikorski), *Practical Reverse Engineering* (Dang), *Practical Binary Analysis* (Andriesse), *Reverse Engineering for Beginners* (Yurichev), *Android Security Internals* (Elenkov), *Android Hacker's Handbook*, *The Android Malware Handbook* (Diogène), *Rootkits: Subverting the Windows Kernel* (Hoglund/Butler), *The Rootkit Arsenal* (Blunden), *Serious Cryptography* (Aumasson), *Cryptography Engineering* (Ferguson/Schneier), *The Art of Memory Forensics* (Ligh)
- Sites: MIT 6.S081, pwn.college, ROP Emporium, pwnable.tw, exploit.education, ctf-wiki.org, xairy list, Project Zero, Quarkslab, pattern-f, gbdev.io, OSDev wiki, saelo.github.io, Hacking with Swift, Apple Metal docs + WWDC sessions, "A Trip Through the Graphics Pipeline" (Giesen), awesome-emulators (DerekTurtleRoe), Unicorn Engine docs (`02-09`), HEVD (HackSysTeam), ired.team, maldevacademy.com, Sliver docs, loldrivers.io, Sigma repo, USB in a Nutshell, USB Made Simple, Linux kernel USB docs, usbguard, Facedancer/GreatFET, syzkaller USB, USBView/USBPcap, WDK/KMDF samples, Apple DriverKit docs + WWDC sessions, virtio spec, Ghidra, jtool/kcache_id/kerneldiff, crackmes.one, Flare-On archive, AOSP security docs + Android security bulletins, Frida, objection, drozer, apktool/jadx/smali, Play Protect, Mobile Threat Catalogue, BlackLotus analyses (ESET, SentinelOne), Diamorphine, UEFI spec, efi-mimikatz, QEMU/OVMF, MinHook/Detours, fishhook, LSPosed/Xposed, bhook/Dobby, LD_PRELOAD + dyld interpose docs, cryptopals.com, cryptohack.org, *The Fuzzing Book* (Zeller), Trail of Bits Fuzzing 101, Volatility 3 docs, corelan.be + FuzzySecurity (Windows userland expl), Apple Hypervisor.framework docs, The Rust Book + Rustonomicon (`04-08`), WebKit source + Project Zero WebKit posts (`08-04`), ChipWhisperer docs (`22-07`), CERT/CC disclosure guidance + GitHub security advisories (Phase 23), aircrack-ng, KRACK + Dragonblood papers (Vanhoef), hostapd-wpe/EAPhammer, wifiphisher, bettercap, WIDS clock-skew research (Phase 28), angr + Triton + KLEE docs (`15-06`), ETW (Pavel Yosifovich), Falco/Tetragon, Apple Endpoint Security docs (`21-06`), ForensicArtifacts repo (`12-08`), binwalk + IoTGoat + Damn Vulnerable Router (`22-09`)
- Code: xv6-riscv, torvalds/linux, apple-oss-distributions/xnu, felix-pb/kfd, opa334/Dopamine, checkra1n

