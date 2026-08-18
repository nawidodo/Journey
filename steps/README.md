# Journey — Step Scaffold

Step-by-step folders for the **Systems Security Engineer** roadmap in [`../LEARNING_PLAN.md`](../LEARNING_PLAN.md).
One folder per learnable step. Each step's `README.md` holds its objective, tasks, resources, and exit criteria — tick boxes as you go and mirror progress in the tracker below.

Every step has three subfolders:
- `notes/` — writeups, book notes, diagrams
- `code/` — source you write
- `labs/` — lab work, POCs, exploit artifacts

## When is a step done?
A step is **finished** when every box under its `## Exit Criteria` is ticked and the required artifact exists in `labs/` (or `code/`/`notes/` per the criterion). That is your per-step capstone — tick the tracker row below and move to the next step. Steps flagged **🚩 M#** are phase checkpoints (the phase's real capstone — re-derive it with the writeup closed). No exit-criteria boxes ticked = step not done, regardless of time spent.

## Links
Every step's `## Links` section holds the concrete URLs (docs, repos, blogs, practice platforms) for that step — open them before starting, bookmark what you use.

## Structure
| Phase | Contents | Weeks | Track |
|---|---|---|---|
| `00-Foundations` | C, memory model, tooling, Swift | 1–3 | A (+B Swift) |
| `01-xv6-riscv` | MIT 6.S081/6.1810 labs incl. net lab | 4–9 | A |
| `02-Emulator` | CHIP-8 → NES → Metal → iOS app (+ Unicorn 09 + GBA 10 + RISC-V 11 + SNES 12 + Genesis 13 + PS1 14 + PSP 15 + emulator shaders 16 + WASM runtime 17, stretch) | 6–18 | B |
| `03-Exploitation-Fundamentals` | overflow → ROP → heap → mitigations + practice platforms (+ network recon 07, parallel) | 10–14 | A |
| `04-Real-Kernels-Linux-XNU` | Linux kernel → XNU → Apple security (+ Rust 08 + FreeBSD 09, parallel) | 14–20 | A |
| `05-Linux-Kernel-Exploitation` | ladder L1→L3 + Dojo practice + fuzzing methodology + own coverage-guided fuzzer 12 + QEMU device model 13 + eBPF tool 14 (stretch) | 20–30 | A |
| `06-Sandbox-Escape` | seccomp → container escapes → Seatbelt → VM escapes + own hypervisor (stretch) + seccomp sandbox builder (10, stretch) | 34–39 | A |
| `07-XNU-iOS-Exploitation` | checkm8 → KFD → Dopamine | 35–46 | A |
| `08-Browser-Exploitation` | JS engines → d8 CVE → JSC/WebKit (04, optional) | 47+ | A |
| `09-Real-World-Chains` | ForcedEntry, Pegasus, Pwn2Own | capstone | A |
| `10-GPU-Compute` | concepts → CPU SIMD → Metal compute → memory → software renderer → Metal graphics → shaders (MSL/GLSL/HLSL) → textures → async → profiling → Vulkan/DX12 → capstone (+ path tracer 13 + CUDA for AI 14 + own NN 15 + model quantization 16, stretch) | 12–28 | C |
| `11-Windows-Kernel` | userland exploit dev → internals/WinDbg → HEVD → pool/token → real CVEs → mitigations → capstone | 29–44 | D |
| `12-Malware-Dev` | PE/APIs → injection → evasion → C2 → detection → capstone → advanced in-memory (Sektor7-class) → anti-forensics 08 + mini-loader 09 + own packer 10 (stretch) | 18–30 | E |
| `13-USB-Security` | protocol/stack → Linux driver → attack surface → fuzzing/emulation → iOS/Windows links → capstone | 24–32 | F |
| `14-Cross-OS-Driver-Dev` | Linux craft → MMIO/DMA/IRQ → Windows KMDF → macOS dext → cross-OS fuzzing → real USB device + userspace driver + app → gamepad turbo (attach to existing controller, all OSes) → capstone | 28–41 | G |
| `15-Reverse-Engineering` | Ghidra → kernelcache → crackmes/Flare-On → patch-diff → crash triage (+ symbolic execution 06 + mini-debugger 07 + mini-disassembler 08 + mini-decompiler 09, stretch) | 16–20 | — |
| `16-Android-Exploitation` | security model → app surface → binder/kernel → real CVEs → root → capstone (+ TEE lab 07, stretch) | 34–44 | H |
| `17-Android-Malware-Dev` | APK/dex/smali → implant → C2/evasion → fraud → detection → capstone | 40–48 | I |
| `18-Rootkit-Bootkit` | principles → Linux LKM/eBPF → Windows rootkit → UEFI bootkit → macOS → mobile → capstone | 44–56 | J |
| `19-Runtime-Hooking` | fundamentals → own Windows/Linux/macOS/iOS/Android hooks → capstone engine | 20–32 | K |
| `20-Cryptography` | symmetric/modes → RSA/ECC → hashes/MACs/side-channels → TLS → OS crypto stores → capstone (+ own mini-TLS 07 + GPU cracker 08 + ML-KEM 09 + zk-SNARK 10 + E2EE 11, stretch) | 14–20 | L |
| `21-Detection-DFIR` | memory forensics → log analysis/hunting → Sigma/YARA → disk artifacts → IR capstone (+ own mini-EDR 06, M29; + network detection 07 + honeypots 08 + YARA engine 09 + memory scanner 10, stretch) | 30–36 | M |
| `22-Embedded-USB` | USB device firmware + HID → DuckyScript engine → ESP32 WiFi C2 → stealth/hardware → network-adapter pivot → advanced firmware (composite/OTA/self-destruct) → fault injection (07, stretch) → plugandpwn-class capstone (+ embedded firmware exploitation 09 + embedded signals 10 + DIY radio gadgets 11 + CAN bus injector 12 + own RTOS scheduler 13, stretch) | 28–36 | N |
| `23-Career` | writeups/portfolio → resume/interview → coordinated disclosure → career-launch capstone | post-M23 | O |
| `24-Make-Your-Own` | own OS → own DB → own git → own shell → own TCP/IP → own regex → own assembler → micro-transformer (+ own C compiler 09 + own RISC-V microkernel on your 02-11 emulator 10 + own SQLite file format 11 + own SSH server 12 + own container runtime 13 + own terminal emulator 14 + own filesystem 15 + own memory allocator 16 + own HTTP server 17 + own WireGuard-lite 18 + own DNS resolver 19 + own packet sniffer 20 + own BGP speaker 21 + own onion router 22 + own QUIC-lite 23 + own NAT/router 24 + own compression 25 + own LSM-tree 26 + own PNG decoder 27 + own quantum simulator 28 + own Kademlia DHT 29 + own sampling profiler 30 + own bootloader 31 + own NTFS parser 32 + own APFS parser 33 + own mail server 34 + own JPEG decoder 35 + own BitTorrent client 36 + own chess engine 37 + own text editor 38 + own package manager 39 + own audio synth 40 + own search engine 41 + own key-value cache 42 + systems gauntlet 43 + own TPM lab 44 + own registry parser 45 + own MQTT broker 46 + own JS interpreter 47 + own orchestrator 48 + own font renderer 49 + own logic analyzer 50 + own time-series DB 51 + own message queue 52, stretch; security-relevant from-scratch reinforcement) | W46+ opt | P |
| `25-Privilege-Escalation` | privesc tool internals, all OSes — boundary model → linpeas/pspy → SUID/caps/sudo → mimikatz → potato family → WinPEAS/PowerUp → BloodHound/AD → macOS TCC → mobile root (Magisk/jailbreak) → capstone triage tool + re-derive | W20+ parallel | Q |
| `26-Bluetooth-Devices` | BT stack theory → first BLE peripheral (nRF/ESP32) → HID-over-GATT gamepad → host-side + sniffing → classic (A2DP/SPP) → pairing/security → capstone BLE HID turbo gamepad (M26) | 36–48 | R |
| `27-C2-Framework` | C2 architecture → wire protocol + HTTP/S beacon → cross-platform agent → teamserver + operator UI → DNS/SMB/WS transports + redirectors → capstone full op + own-detection round-trip (M27) | 30–38 | S |
| `28-WiFi-Security` | 802.11 stack + monitor mode → WPA2/WPA3 attacks (PMKID/KRACK/Dragonblood) → enterprise EAP relay + captive portal → rogue-AP hosting + MITM + pivot → WIDS detection mirror → capstone rogue-AP engagement + own detection round-trip (M28) | 28–40 | T |

## Checkpoint tracker (from LEARNING_PLAN.md)
- [ ] **M1** trace xv6 syscall → `01-xv6-riscv/03-lab-syscall`
- [ ] **M2** xv6 answers ping → `01-xv6-riscv/09-lab-net-e1000`
- [ ] **M3** NES via Metal on macOS → `02-Emulator/07-metal-mtkview-renderer`
- [ ] **M4** NES on iPhone → `02-Emulator/08-ios-app-on-device`
- [ ] **M5** KTRR/PPL/PAC matrix → `04-Real-Kernels-Linux-XNU/07-apple-security-guide-ktrr-ppl-pac-aprr-ssv`
- [ ] **M6** pwn.college solo → `03-Exploitation-Fundamentals/05-pwn-college-labs`
- [ ] **M7** Vermin re-derived → `05-Linux-Kernel-Exploitation/06-vermin-cve-2021-22555`
- [ ] **S1** container escape re-derived → `06-Sandbox-Escape/03-container-escape-runc-cves`
- [ ] **S2** VM escape re-derived → `06-Sandbox-Escape/07-device-emulation-escapes`
- [ ] **M8** KFD re-derived → `07-XNU-iOS-Exploitation/05-kfd-pattern-f`
- [ ] **M9** Dopamine explained → `07-XNU-iOS-Exploitation/08-dopamine-source-libjailbreak-launchdhook-systemhook-choma`
- [ ] **M10** d8 CVE exploited → `08-Browser-Exploitation/03-sandboxed-d8-cve`
- [ ] **M11** chosen capstone at target → `10-GPU-Compute/12-capstone-renderer-nbody`
- [ ] **M12** 2 HEVD classes + 1 real CVE → `11-Windows-Kernel/06-capstone-hevd-cve`
- [ ] **M13** implant survives own Sigma rules → `12-Malware-Dev/06-capstone-implant`
- [ ] **M14** USB attack + own detection rules → `13-USB-Security/06-capstone-usb-attack`
- [ ] **M15** driver on Linux+Windows+macOS → `14-Cross-OS-Driver-Dev/08-capstone-cross-os-driver`
- [ ] **M16** crash → root cause chain → `15-Reverse-Engineering/05-capstone-crash-triage`
- [ ] **M17** Android root from exploit chain → `16-Android-Exploitation/06-capstone-android-root-chain`
- [ ] **M18** Android implant survives own detections → `17-Android-Malware-Dev/06-capstone-android-implant`
- [ ] **M19** rootkit/bootkit chain + own detections → `18-Rootkit-Bootkit/07-capstone-rootkit-bootkit`
- [ ] **M20** own from-scratch hook engine on ≥5 platforms → `19-Runtime-Hooking/07-capstone-cross-platform-hooking`
- [ ] **M21** weak crypto scheme broken end-to-end → `20-Cryptography/06-capstone-crypto-weak-scheme`
- [ ] **M22** memory+disk+logs → root cause → detection rule → `21-Detection-DFIR/05-capstone-ir-scenario`
- [ ] **M23** plugandpwn/O.MG-class attack-USB device (injection + pivot + exfil) → `22-Embedded-USB/08-capstone-attack-device`
- [ ] **M24** portfolio live + one disclosed advisory + resume v2 → `23-Career/04-capstone-career-launch`
- [ ] **M25** cross-OS privesc triage tool + 10-tool internals wiki + mimikatz/potato re-derived → `25-Privilege-Escalation/12-capstone-tool-triage`
- [ ] **M26** BLE HID turbo gamepad (hardware turbo + secure pairing + OTA, ≥2 OSes) → `26-Bluetooth-Devices/07-capstone-turbo-pad`
- [ ] **M27** own C2 framework survives its own detection rules (multi-transport, full op, before/after) → `27-C2-Framework/06-capstone-full-op`
- [ ] **M28** Wi-Fi capstone — rogue AP + client compromise + pivot on own lab, caught by own detection rules → `28-WiFi-Security/06-capstone-rogue-ap`
- [ ] **M29** own mini-EDR detects its own implant/rootkit artifact on ≥2 OSes, FP-engineered → `21-Detection-DFIR/06-own-mini-edr`

## Path
Track A sequential: `00` → `01` → `03` → `04` → `05` → `06` → `07` → `08` → `09`
Track B parallel (start after W6, alongside xv6): `02`
Track C parallel (start W12, alongside Track B Metal app): `10`
Track D parallel (start W30, alongside Phase 5 tail): `11`
Track E parallel (start W18, alongside Phase 3/4; advanced step feeds Track D): `12`
Track F parallel (start W24, alongside Phase 5/6): `13`
Track G parallel (start W28, alongside Phase 5/6): `14`
Reverse engineering parallel (start W16, alongside Phase 4 tail; feeds Track D/F/Phase 7): `15`
Track H parallel (start W34, alongside Phase 7/Track D): `16`
Track I parallel (start W40, Android malware; Track E concepts transfer): `17`
Track J parallel (start W44, capstone of the offensive tracks; reuses D/G/H/Phase 7): `18`
Track K parallel (start W20, feeds Track E evasion + Track J kernel hooks): `19`
Track L parallel (start W14, alongside Phase 4 kernel reading): `20`
Track M parallel (start W30, the defensive counterweight to Track D/F): `21`
Track N parallel (start W28, low-priority, after Track F protocol basics; builds the device Track F attacks): `22`
Track O (start post-M23, career — writeups/portfolio, resume/interview, coordinated disclosure): `23`
Track P (start W46+, optional, make-your-own systems from scratch mapped to build-your-own-x): `24`
Track Q parallel (start W20+, pairs with Phase 3/5/11/12/16 — privesc is the payoff of every exploit phase; defer rule applies): `25`
Track R parallel (start W36, alongside Phase 7/Track G; BT is the wireless twin of Track N — pairs with 14-07 turbo + Phase 7 CoreBluetooth; low priority, defer rule applies): `26`
Track S parallel (start W30, after Track E core W18–30 — its implant/evasion skills are prerequisites; feeds Track M detection targets): `27`
Track T parallel (start W28, low priority; Wi-Fi security — radio twin of Track N; pairs Track F/R/M; defer rule applies): `28`

Start: [`00-Foundations/01-c-knr-ch1-6`](00-Foundations/01-c-knr-ch1-6/README.md)
