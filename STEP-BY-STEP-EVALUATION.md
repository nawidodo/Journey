# Step-by-Step Evaluation — what's essential, optional, fun, and why (learner lens)

Tag per step: **E** = essential spine (do, in order, quality > speed) · **O** = optional-but-recommended (pick by goal/direction) · **F** = fun/rest (do between E weeks) · **X** = stretch/skip under time pressure.
Evaluation lens: does each step build on the one before (low-level first: OS ground-up, hardware, the software/hardware seam)? Every E step lists its prerequisite.

---

## Phase 00 · Foundations — all **E** (the load-bearing walls)
- `00-01` **E** C K&R — the C you'll read/write in every later kernel/exploit. Prereq: none.
- `00-02` **E** memory model (Art of Exploitation) — pointers/stack/heap; the model every exploit assumes. Prereq: C.
- `00-03` **E** tooling (git/make/gdb) — the workflow you'll live in. Prereq: 00-01.
- `00-04` **O** Swift basics — macOS-class comfort, later feeds 02-08/24-109. Prereq: any language.

## Phase 01 · xv6 (OS from ground up) — all **E** except last
- `01-01` **E** toolchain — RISC-V cross-compile. Prereq: 00-01.
- `01-02` **E** util — syscall call/return path. Prereq: toolchain.
- `01-03` **E** syscall — first kernel code, the trap path. Prereq: 01-02.
- `01-04` **E** pgtbl — SV39 page tables; **the map every memory exploit assumes**. Prereq: 01-03.
- `01-05` **E** traps — interrupts/exceptions, the kernel/user seam. Prereq: 01-03.
- `01-06` **O** COW fork — memory sharing trick. Prereq: 01-04.
- `01-07` **O** threads — scheduling/locks. Prereq: 01-03.
- `01-08` **O** fs — inodes/logs, the disk-side build-up for 24-15/32/33. Prereq: 01-03.
- `01-09` **E** net (e1000) — the NIC ↔ driver ↔ stack seam; first hardware-software interface. Prereq: 01-03.
- `01-10` **X** stretch TCP echo — virtio; nice-to-have.

## Phase 02 · Emulator (machine understood by rebuilding it) — **E** core, rest O
- `02-01` **E** CHIP-8 — the one-week "I built a CPU" win. Prereq: any language.
- `02-02` `02-03` `02-04` `02-05` **E** NES core — 6502 ISA → PPU (how pixels happen) → APU (how sound happens) → cart mappers. **The heart of "how computers really work": a video game machine demystified.** Prereq: 02-01.
- `02-06` `02-07` **O** macOS SwiftUI shell, Metal renderer — platform glue for 02-08.
- `02-08` **O** NES on iPhone — iOS delivery. Prereq: 02-05.
- `02-09` **O** Unicorn — CPU emulation as a *tool* (shellcode tracing); the RE-practical flip. Prereq: 02-02.
- `02-10` **O** GBA · `02-11` **O** RISC-V · `02-12` **O** SNES · `02-13` **O** Genesis — second/third CPUs; breadth by interest. Prereq: 02-05.
- `02-14` **O** PS1 — 3D raster pipeline; **the GPU-history lesson**. Prereq: 02-05.
- `02-15` **F** PSP — MIPS + the first "console security" reading. Prereq: 02-14.
- `02-16` **F** shaders · `02-17` **O** WASM runtime — JIT/browser sandbox reading. Prereq: any core above.

## Phase 03 · Exploitation fundamentals — **all E**
- `03-01` **E** buffer overflow — the stack pivots here. Prereq: 00-02, 01-03.
- `03-02` **E** ROP — defeating NX; the control-flow grammar. Prereq: 03-01.
- `03-03` **E** heap/UAF — the class that survives today. Prereq: 03-01.
- `03-04` **E** mitigations — ASLR/canary/NX/SMEP/SMAP/PAC; **the arms-race map you'll navigate**. Prereq: 03-01..03.
- `03-05` **E** pwn.college — lab grind where it clicks. Prereq: 03-01..04.
- `03-06` **O** CTF platforms — maintenance practice.
- `03-07` **O** network recon — scanning ethik; light, early.

## Phase 04 · Real kernels — **E** core
- `04-01` **E** Linux kernel dev/OSTEP — the big-picture OS survey. Prereq: 01.
- `04-02` **E** read kernel (mm/sched/net) — read the real thing after building the toy. Prereq: 01, 04-01.
- `04-03` **E** write a module — the driver seam you'll exploit in 05/14. Prereq: 04-02.
- `04-04` **O** read drivers (e1000/virtio) — device model. Prereq: 01-09, 04-03.
- `04-05` **E** arm64 basics — the ISA of every modern phone/Mac; **mandatory for iOS/macOS goals**. Prereq: 01, 03.
- `04-06` **O** XNU internals — macOS/iOS kernel; pairs 01 concepts. Prereq: 04-01.
- `04-07` **E** Apple security guide (KTRR/PPL/PAC/APRR/SSV) — **the iOS security base layer**; the trust chain all 07 steps assume. Prereq: 04-05.
- `04-08` **O** Rust for systems — modern option; skip if C is solid.
- `04-09` **O** FreeBSD deep dive — chain fuel for 29-02 (PS4 runs FreeBSD 9).

## Phase 05 · Linux kernel exploitation — **all E** (the payoff of 01+03)
- `05-01..02` **E** modprobe/ret2usr — first real exploits. Prereq: 03, 04-03.
- `05-03` **E** Dirty COW — the classic page-cache race. Prereq: 01-04, 05-01.
- `05-04` **E** kernel ROP — post-mitigation flow control. Prereq: 03-02, 05-01.
- `05-05` **E** Dirty Pipe — modern-era; **do both classics, they're short**. Prereq: 05-01.
- `05-06..07` **O** vermin/kmalloc-cve — heap grooming depth.
- `05-08` **O** eBPF verifier CVE — the verifier reading. Prereq: 05-01.
- `05-09` **O** cross-cache UAF — advanced grooming.
- `05-10` **O** dojo — endurance decks.
- `05-11..15` **O→F** fuzzing/own-coverage-fuzzer/device-model/own-ebpf-tool/own-spectre-PoC — the builder's land, great for fun weeks.
- `05-11` **O** fuzzing — **the skill that turns reading into findings**; promote to E if research-goal.

## Phase 06 · Sandbox/container escape — **E** core
- `06-01` **E** Linux sandbox fundamentals — chroot/namespaces/cgroups. Prereq: 04-01.
- `06-02` **E** seccomp escape challenges — the syscall-filter seam. Prereq: 06-01.
- `06-03` **E** container escape (runc CVEs) — **the boundary that made Kubernetes news**. Prereq: 06-01.
- `06-04` **O** macOS sandbox (Seatbelt) — macOS-specific. Prereq: 04-06.
- `06-05` **O** iOS/macOS escape techniques — pairs 07.
- `06-06` **E** VMM/hypervisor fundamentals — the next isolation layer; **prereq for 24-81 and Xbox HV (29-04)**. Prereq: 06-01.
- `06-07..08` **O** device-emulation/guest-tools escapes — depth.
- `06-09..11` **O→F** own-hypervisor/build-sandbox/sandbox — builder weeks.

## Phase 07 · XNU/iOS exploitation — **E** core for iOS goal, O otherwise
- `07-01` **E** checkm8 — the BootROM bug; **how the chain fires from reset**. Prereq: 04-07, 04-05.
- `07-02` **O** machswap/tfp0 — kernel task port era.
- `07-03` **O** Trident — WebKit→kernel, the chain shape. Prereq: 07-01.
- `07-04` **O** oobPCI — the CVE walk-through.
- `07-05` **E** KFD — the modern-era kernel exploit; **the current iOS jailbreak engine**. Prereq: 03-03, 07-01.
- `07-06` **O** arm64e PAC deep-dive — pointer auth internals.
- `07-07..08` **O** Fugu15/Dopamine — the jailbreak as product; read-after-KFD.

## Phase 08 · Browser — **O** (specialist) unless research-goal
- `08-01` **O** JS engine series — the sandbox everyone attacks.
- `08-02..04` **O** d8 CTF, sandboxed CVE, JSC — applied.
- `08-05` **F** own JS engine exploit — builder capstone; fun if you like browsers.

## Phase 09 · Real chains — **O** capstone read
- `09-01..04` **O** ForcedEntry/Pegasus/Pwn2Own/Syzkaller-XNU — **the world's best written chains; read one per quarter**.
- `09-05` **F** own exploit chain — synthesis project.

## Phase 10 · GPU — **F** unless graphics-goal
- `10-01..12` **F→O** arch/concepts to renderer — the fun track (NES-in-Metal mindset).
- `10-13` **F** path tracer · `10-14..16` **O** CUDA/neural-net/quantization — ML-detector fuel (24-79 builds on 10-15).
- `10-03/10-11` **O** if Metal-heavy goals: CoreText feeds 24-111.

## Phase 11 · Windows kernel — **O** (specialist), E if Windows-goal
- Core `11-01..04` **O** — userland→internals→driver-RE→pool-overflow; **the HEVD ladder is your Windows 01-xv6**. Then `11-05..07` real CVEs + ROP + capstone.

## Phase 12 · Malware dev — **O** (specialist), F for fun-runners
- `12-01..03` **O** PE/API → injection → AMSI/ETW evasion — pairs RE/detection later.
- `12-04..05` **O** C2 implant + detection — the round-trip.
- `12-06..10` **F** sektor7/anti-forensics/mini-loader/own-packer — fun builder weeks.

## Phase 13 · USB — **O** (hardware-goal only; needs cheap HW)
- Core 13-01..05 **O** — protocol → Linux driver → attack surface → fuzzing; capstone 13-06.

## Phase 14 · Cross-OS drivers — **O** (hardware-goal only)
- 14-01..02 **O** Linux craft → DMA/IRQ/MMIO — **the hardware/software seam formalized**; promote to E if kernel-systems-goal. Rest O/F (KMDF/dext/fuzz/turbo).

## Phase 15 · Reverse engineering — **all E** (the read-side of every write-side)
- `15-01` **E** Ghidra workflow — navigate a binary. Prereq: 00-01, 03-01.
- `15-02` **E** XNU kernelcache RE — first real target; **pairs 04-06/07**. Prereq: 15-01.
- `15-03` **E** crackmes/FlareOn — reps until it's reflex. Prereq: 15-01.
- `15-04` **O** patch-diffing CVE workflow — the analyst's daily tool.
- `15-05` **E** capstone crash-triage — **from crash to root cause; the skill summary**.
- `15-06..09` **O→F** symex/mini-debugger/mini-disassembler/own-decompiler — builder echoes.

## Phase 16 · Android — **O** (mobile-goal), F otherwise
- `16-01..02` **O** model + app surface · `16-03..04` **O** binder/kernel + real CVEs · `16-05..06` **O** rooting/TrustZone + capstone · `16-07` **O** TEE OP-TEE lab — the hardware trust boundary.

## Phase 17 · Android malware — **F** (skip unless redteam-goal)

## Phase 18 · Rootkit/bootkit — **O** (J-track graduation) · all **F** unless offensive-career
- 18-01..07 — read-heavy, mirrors 12/07 content; capstone if you do it.

## Phase 19 · Runtime hooking — **O** (feeds E/D tracks)
- 19-01..06 hooking per-OS · 19-07 capstone — keep the desktop half for malware track; mobile half can slip.

## Phase 20 · Cryptography — **E** core 01–06, rest O/F
- `20-01` **E** symmetric modes — how data actually hides. Prereq: none.
- `20-02` **E** RSA/ECC — the math that signs everything. Prereq: 20-01.
- `20-03` **E** hashes/MAC/side-channels — timing lessons for the year. Prereq: 20-01.
- `20-04` **E** TLS internals — the protocol all network reading assumes. Prereq: 20-02.
- `20-05` **O** OS crypto stores — Keychain/etc; macOS-goal promote to E.
- `20-06` **E** capstone — break a weak scheme. Prereq: 20-01..04.
- `20-07..13` **O→F** own-TLS/GPU-cracker/ML-KEM/zk-SNARK/E2EE/password-manager/cache-side-channel — builder bank.

## Phase 21 · Detection/DFIR — **E** core 01–05 (the blue half), rest O/F
- `21-01` **E** Volatility — memory of live systems read. Prereq: 01-04 model.
- `21-02` **E** log analysis — where hunts start.
- `21-03` **E** Sigma/YARA — rules as code: **pairs your own 24-79 detector**.
- `21-04` **O** disk artifacts — DFIR specifics.
- `21-05` **E** capstone IR scenario — the scenario run.
- `21-06..10` **O→F** mini-EDR/zeek/honeypots/own-yara/own-memory-scanner — builder bank.

## Phase 22 · Embedded — **O** (hardware-goal; cheap-HW needed)
- 22-01..06 core, 22-07 fault-injection **O** (rare gem), 22-08 capstone, 22-09..13 stretch.

## Phase 23 · Career — **O** post-plan (zero load during study)

## Phase 24 · Make-Your-Own — **the E-selected subset is the year's "fun that teaches"**
Tier 0 (do with spine, they ARE the E-builds): `24-01` OS · `24-09` compiler · `24-16` allocator · `24-20` sniffer · `24-17` HTTP.
Tier 1 (pick 6–8 by direction): µkernel `10`, container `13`, filesystem `15`, bootloader `31`, linker `87`, profiler `30`, `32` NTFS/`33` APFS/`99` DMG parsers, `97` Mach-O, `109` plist, `103` macOS monitor, `110` arm64 playground, `115` iOS analyzer, `76`→`98`→`100/101`→`102/108` A/V chain, `94` PDF, `95` IMAP, `96` DNSSEC, `117` Game Boy, `118` io_uring, `119` ETW, `111` Metal.
Tier 2 (fun banks): quantum `28`, chess `37`, raycaster `63`, Sokoban `93`, chiptune `106`, subtitles `107`, cheat-tool `112`, noise-reduction `113`, stabilizer `114`.

## Phase 25 · Privesc — **O** to E if offensive-goal
- 25-01..02 boundary + triage tools; keep macOS/mobile halves for Apple-goal (25-10/11).

## Phase 26/27/28/29 · BT / C2 / Wi-Fi / Console — **O/F niche**
- 26 if hardware-goal, 27 if redteam-goal, 28 low-priority, 29 read-anytime (consumes zero concurrency). Phase 29 goes great with fun weeks (29-06/07 are charming).

---

## The not-packed year (≈10–12 h/wk = 2h/day)

| Quarter | Spine (E) | Builder weeks (F/O) |
|---|---|---|
| Q1 (W1–13) | 00 → 01-xv6 all → 02-01..05 NES | 2 fun weeks: 02-16 shaders / 24-16 allocator |
| Q2 (W14–26) | 03 all → 04-01..07 → 05-01..05 | 2 builders: 24-09 compiler / 24-20 sniffer |
| Q3 (W27–39) | 06-01..06 → 15-01..05 → 20-01..06 → 21-01..05 | 3 builders: 24-17 HTTP / 24-97 Mach-O / 24-103 monitor |
| Q4 (W40–52) | 07-01/05 (Apple) + 29 read-track + chosen niche (11 or 16) | 3 builders: 24-110 arm64 / 24-115 iOS analyzer / A/V chain pick |

That's ≈130 steps (35% of 372) — essential spine + 8–10 builders + fun weeks. Everything O/F beyond that is the second year, not pressure.

## The proof rule — “if I can't build it, I don't understand it”

Every one of the 371 steps already ends in Exit Criteria boxes whose artifact is a build you can run: `labs/`+`code/` by default, `notes/` where the step is legitimately read-based (career, research-archive reading). The capstone is the *closed-book rebuild*: close the tutorial, rebuild the artifact, and then destroy-and-rebuild once more. That is the pass/fail — not time spent. The step is done only when every box is ticked and the artifact exists (the tracker definition in `steps/README.md`).

Multi-platform ground-up coverage is already enforced by phase (all steps reference a prior build):

| World | Covered by |
|---|---|
| OS ground-up | 01-xv6, 24-01 own-OS, 24-10 µkernel, 24-31 bootloader, 24-84 init |
| Kernel | 04, 05, 24-82 gdb stub, 14 drivers |
| Network | 24-20 sniffer → 24-17/19/21/67 → 24-18/23/24/53, 04-08 |
| Database/storage | 24-02 storage engine, 24-26 LSM, 24-51 TSDB, 24-15 fs, 24-32/33/99 parsers |
| Compilers/toolchain | 24-09 C compiler, 24-87 linker, 24-39 package manager, 24-38 editor |
| macOS | 04-06/07, 07, 24-103/104/105/109/110/111 |
| iOS | 07, 24-97/115, 02-08 |
| Linux | 04/05/06, 24-118, 10-14/15 artifacts |
| Android | 16, 17, 07 pairs |
| Windows | 11, 25, 24-119, 24-45 |
| Consoles Chip8→PS5/Xbox | 02 all, 24-117 GB, 29-01..07, 06-06 HV |

Nothing stays read-only except the steps that are explicitly study/archive (career coaching, deep-dive reading) — and those still demand a written artifact as the exit.

## The honest verdict
- **Build-up verified**: each E spine phase cites a prior phase as prereq; low-level ordering holds (C → memory → OS-guts → exploitation → RE → crypto → detection), and the newest Apple/console work sits exactly on the 01/02/04/24 foundations it needs.
- **Not packed**: 130/372 ≈ 35% by the end of year one; the rest is choice, structured by this doc's tags.
- **Fun integrated**: NES (02), GB (24-117), Atari (29-06), chiptune (24-106), cheat tool (24-112), odd-silicon (29-07) — one fun build per quarter minimum.
- **The one missing beat**: Phase 29 has no single "open a console firmware blob and find an exploit primitive" step — emulation/parsers cover the skill, but a scripted DECA/DECI-class lab would seal it. Added `29-08` next round if you want it.