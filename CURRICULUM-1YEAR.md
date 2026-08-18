# One-Year Curriculum — Essential vs Optional, Per-Platform Priorities

Per-step E/O/F tags and the week-by-week schedule: see [`STEP-BY-STEP-EVALUATION.md`](STEP-BY-STEP-EVALUATION.md).
Want to build your own jailbreak/root? The prerequisite-locked gate chain (own-device, re-derive-don't-copy) is [`JAILBREAK-ROOT-PATH.md`](JAILBREAK-ROOT-PATH.md). The full iOS/Android offensive surface beyond kernel (hooking, app RE, traffic, storage, implants, hardware) — also gated — is [`MOBILE-OFFENSIVE-PATH.md`](MOBILE-OFFENSIVE-PATH.md).

How to spend 52 weeks: the spines that must hold, the tracks you pick by interest, the niches that make you rare. This doc marks every phase E (essential core), O (optional-but-recommended), or S (stretch/interest-only). Track P (Make-Your-Own, 109 builds) is the engine room — see the cadence rule below.

## The one-year rule

- **W1–26: follow Track A in order** (Phase 00 → 01 xv6 → 03 → 04/05 → 06 → 07 → 08/09). This is the spine; every later specialty assumes it. Do NOT parallel-track until the spine is through W20.
- **W27–52: keep one platform priority + one build a month.** The platform priorities below are the specializations; Track P picks are the deliverables.
- **Cadence:** 1–2 Track P builds per month, chosen from the priority lists, not randomly. Each build takes one prior career skill (compiler, RE, OS, network) from theory to muscle.
- **Stop rule:** a week with no shipped artifact (writeup, build, lab result) is a lost week. Every step has Exit Criteria — those are the weekly fin.

## Essential (the year fails without these)

| Priority | Steps | Why it's the spine |
|---|---|---|
| Kernel | 01-xv6 (all 10), 04-01..04, 05-01..05 | The lowest trust boundary; everything else hangs off it |
| Exploitation | 03-01..05, 06-01..05, 04-05..07 | The core skill of the discipline |
| RE | 15-01..04 | Reading binaries is the read-half of every write-half |
| Low-level networking | 03-06/07, 04-08/09, 24-20 | Packets are the other trust boundary; own sniffer 24-20 is the milestone |
| Crypto fundamentals | 20-01..04, 20-07 (own TLS) | Every protocol you study after is crypto + formatting |
| Detection | 21-01..04 (writeups) | Know the blue side to hunt the red |
+

## Optional but recommended (pick by goal)

| Goal | Phase / steps |
|---|---|
| Offensive engineer | 11 Windows kernel, 12 malware, 16 Android, 28 Wi-Fi (low priority) |
| Apple engineer | 07 XNU/iOS study, 19-04 macOS hooking, 25-10 macOS privesc |
| Platform-agnostic builder | Phase 24 Track P builds tied to your priorities (see lists) |
| Researcher | Phase 29 console security (W41+ opt, niche), 02 emulators, 20-08..13 crypto labs, 10-16 quantization |

## Priority list: iOS security (own-hardware, no jailbreak needed)

1. `24-97` Mach-O parser (prime: iOS binaries are Mach-O — every other iOS step reads through it)
2. `24-109` plist parser (entitlements/Info.plist/mobileprovision: the gates iOS enforces)
3. `24-115` iOS app static analyzer (ipa/mach-O class surface + entitlements — the no-device RE entry)
4. Phase 07 study (checkm8→dopamine) for the jailed-ecosystem theory (read-only, keep W39–44)
5. `24-106`..`24-107` only if fun: iOS ships A/V — knowledge transfers from your MP4/dash builds
   iPadOS = same kernel + API surface; the iOS steps above transfer 1:1 (Stage Manager/multitasking differ, the security model does not).

## Priority list: macOS security (your daily OS — own-machine labs)

1. `24-103` macOS process monitor (sysctl/libproc: the OS data plane)
2. `24-104` dylib injector (loader trust: amfid/library-validation/codesign wall)
3. `24-105` logarchive parser (unified logging: the DFIR goldmine)
4. `24-99` DMG reader (installer/quarantine attack surface)
5. `19-04`, `25-10` phase steps; `24-111` Metal compute if GPU interests you

## Priority list: kernel (deep)

- 01-xv6 labs are sacred. Then: `05-13` qemu device model, `05-16` Spectre (stretch), 24-01 (own kernel), `24-10` own RISC-V µkernel (on your own emulator), `24-82` kernel gdb stub, `24-84` init system. Asahi-style bringup is the exotic version — hardware-gated, read the engineering blogs (below) for the thinking, don't buy hardware for it.

## Priority list: console / game hardware (own consoles lab-only)

1. `29-01` trust-model matrix (boot chain, key authority, hardening, sandbox, entry surfaces — the questions you ask ANY signed-everything platform)
2. `29-02` PS4/PS5 Orbis: run the kernel's FreeBSD lineage in own VM, re-create the UAF class, harden it — the 2017-era chain made concrete
3. `29-03` Switch Tegra: NCA format parser (24-99 discipline) + boot-verifier flip simulation — TrustZone/RCM/fuses on paper
4. `29-04` Xbox: guest-under-hypervisor isolation proof in your own VM — the HV-as-norm future
5. `29-05` capstone: the full matrix vs iOS (07) + desktop (24-01) — one research-grade deep-dive
   Emulation first (Phase 02); hardware only if you own it. Zero concurrency cost.

## Priority list: low-level networking

- 24-20 sniff, 24-17 HTTP, 24-18 WireGuard, 24-19 DNS, 24-15/26/51 storage-adjacent, 24-53 tailscale (NAT traversal), 24-24 NAT, 24-67 adblock-DNS, then 24-56 scanner, 24-57 IDS, 24-102 HLS (network video), 24-90 rate limiting. Same mental model up and down: parsing bytes at a boundary.

## Niche / unusual specialization (study these, steal their problems)

- **Asahi Linux** (linux-on-Apple-Silicon bringup: reverse engineering a platform nobody documented) — read their blog posts as a template for "how to learn an undocumented system": start with the boot chain, then display/GPU/power. Their GPU driver work is the best public example of kernel-device RE in the wild.
- **Hoshino Lina's channel** (game/tool binary RE: cracking, patching, decompiling with IDA/Ghidra) — the applied version of Phase 15. Do her exercises with your own copies of open-source or abanddonware binaries.
- **John Levine / W. Richard Stevens archive**: the original low-level canonical reads (TCP/IP Illustrated); the network version of the RE reading.
- **Monthly CTF** (03-06): one beginner challenge a week keeps every skill from rotting.

## Topic index — where each discipline lives (keyword → phase/steps)

| Cluster | Phases / steps |
|---|---|
| Memory corruption, heap/stack/UAF, exploit dev | 03, 04-05, 05, 11, 16 (Android), 29-02 |
| Fuzzing, coverage fuzzer, (to)Bug hunting | 05-12, 08, 22-07, 24-118 |
| Obfuscation, evasion, packing | 12-05/10, 15-09, 24-79 |
| Static/dynamic analysis, malware analysis, binary analysis | 15, 12, 21-09/10, 24-97/115 |
| Debugging, mini-debugger, gdb stub | 15-07, 05-capstone, 24-82, 29-02 |
| Assembly, machine code, ABI, calling conventions | 02, 15-08, 24-110, 24-09 |
| Computer architecture, CPU emulation | 02, 24-117, 29-06 |
| OS, kernel dev, drivers, bootloaders | 01, 04, 05-13, 24-01/10/31/82/84, 14-G |
| System programming, compilers, linkers, loaders | 24-09/87/38/39, 15 |
| Executable formats (Mach-O/ELF/PE/APFS/NTFS/DMG/plist/registry) | 24-97/32/33/99/109/45, 24-71 |
| Virtualization, hypervisors, sandboxes, containers | 06, 24-13/81, 29-04, 02-17 |
| Hardware RE, firmware analysis, IoT/embedded, USB | 22, 29, 16-07, 26 |
| Android security, iOS security, mobile root/jailbreak | 16, 07, 07-06, 24-115/110 |
| Rootkits, bootkits, hooking, EDR evasion | 12-08, J track, K track, 24-86, 05-14 |
| Cryptography, TLS, side channels, ZKP, ML-KEM | 20 (07..13), 24-64/70/91 |
| Network low-level, protocols, packet crafting, DNS | 04-08, 24-20/23/19/21/67, 24-56/57 |
| Wireless (Wi-Fi, BLE/BT, radio-adjacent) | 28, 26, 24-116 |
| Windows internals, privesc, ETW, PE | 11, 25, 24-119, 24-45 |
| Linux internals, eBPF, io_uring, kernel exploits | 04/05, 05-14/16, 24-118 |
| macOS internals, dyld, unified logging, TCC | 07, 19, 24-103/104/105/109 |
| GPU compute, graphics, shaders, raytracing | 10, 24-111, 24-63, 02-16 |
| Digital forensics, IR, threat intel, detection/EDR | 21, 24-105/119, 24-79 |
| Game/console security, cheat/RE tooling | 29 (01..06), 24-112, 24-117 |
| C2, malware dev, supply chain, CI/CD security | 27, 12, 24-74/75/78/91 |
| CTF, cryptography contests, hardware wargames | 03-06, 20, 22-07 |

If a keyword from the list isn't here by name, it's a synonym of a row above (e.g. "Incident Response" = 21, "Assembly Language" = 02/15-08/24-110).

## The year's exit test

1. Own kernel boots and serves HTTP (24-01 + 24-17 or 24-10 + 24-05).
2. Own Mach-O parser + plist parser decode an app bundle from your Mac.
3. Own sniffer + IDS survived a replay from your own replayer.
4. One CVEs worth of Phase 11/16/22 study with a working lab.
5. Any two Track P builds you'd show at an interview and explain live.