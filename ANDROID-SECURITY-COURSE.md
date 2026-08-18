# Android Security — Absolute-Beginner Course (hello world → root-primitive re-creation, gated)

Zero Android-security knowledge assumed. You need: a Mac/Linux box, Android Studio (free), and one own Android device later (emulator AVD first — safer, free, identical lessons). Each unit: concept → do → runnable verification → **lesson check (own words, `notes/dN-quiz.md`)**. No advance without both. ~2h/unit, 12 lessons + capstone ≈ 4–5 weeks. When done you enter [`JAILBREAK-ROOT-PATH.md`](JAILBREAK-ROOT-PATH.md) Lane B at R4 — floor already laid.

Compass (re-read when lost): Android's security = **app sandbox (uid) + Binder (IPC choke point) + SELinux (mandatory labels) + Verified Boot (image integrity) + the Linux kernel beneath**. Every attacker step here answers: "which of those am I crossing?" Root = crossing all of them at once.

Safety: emulator (AVD) for every unit; own device only for D10/D11 with Wi-Fi off and a wipe after; you re-create public primitives, never hunt 0-days.

---

## D0 — hello world with its papers
Concept: an APK is a signed zip; Android trusts signature + permissions, not the app. Do: Android Studio → new project → "hello" app → run on **emulator (AVD)**; then `adb` (device bridge) basics: `adb devices`, `adb shell pm list packages`, `adb logcat`; sign the APK yourself: `apksigner` + keystore, read `apksigner verify --print-certs`.
Verify: hello on emulator; you can name your app's UID via `adb shell ps -A | grep` your package.
**Lesson check:** signing vs permissions — what does each gate, in your words.

## D1 — open the APK and see the layers
Concept: APK = DEX (bytecode) + resources + manifest; `aapt` reads the manifest. Do: `unzip -l` your debug APK; `aapt dump badging` → package/permissions/launchable-activity; count the DEX classes: `dexdump` (or `jadx` GUI) and read your hello's main activity bytecode.
Verify: you list your app's permissions and find its launcher activity by manifest dump.
**Lesson check:** what lives in an APK, and why is the manifest the first thing an attacker reads?

## D2 — the bytecode: DEX → smali
Concept: Java/Kotlin compiles to DEX; smali is its readable form; RE starts here. Do: `baksmali` your hello APK, read your main activity's smali; change one string, re-assemble (`smali`), re-sign, install — your modified hello runs (on emulator).
Verify: modified string visible in the running app = you just did app tampering.
**Lesson check:** DEX vs smali vs Java — the transformation chain in your words.

## D3 — runtime & process model: Zygote
Concept: every app forks from Zygote (preloaded ART); each app = one process = one uid = isolated. Do: on emulator, `adb shell ps -A | grep` your app + Zygote; `adb shell cat /proc/<pid>/status` → Uid/Groups; kill/restart your app, watch Zygote spawn it again.
Verify: same pid reused pattern observed, Uid documented.
**Lesson check:** what does an app's uid sandbox protect against, and what doesn't it?

## D4 — Binder: the IPC spine
Concept: Binder = kernel driver moving parcels between processes; intents, services, and HALs all ride it. Do: `adb shell dumpsys activity services` / `dumpsys binder` on the emulator; find your app's binder thread; capture a transaction if you can (`binder` trace via `strace` on a userdebug image — else document the model from dumpsys).
Verify: you name your app's binder endpoints; one transaction traced or modeled.
**Lesson check:** why is Binder "the choke point" of Android IPC, and what would exploiting it give an attacker?

## D5 — the three fences: sandbox, permissions, SELinux
Concept: uid sandbox (coarse), permission model (fine), SELinux (mandatory labels, `u:r:untrusted_app:s0`) — three different mechanisms. Do: on emulator, `adb shell ls -lZ /data/data/`, see app dirs + context labels; read your app's `targetSdk`-granted permissions in manifest; write an app that tries to read another app's private dir → denial logged.
Verify: denial visible in logcat; SELinux context printed for your app's data dir.
**Lesson check:** sandbox vs permission vs SELinux — same word family, different fences; disentangle them.

## D6 — native layer: NDK, JNI, and a crash
Concept: apps (and exploits) use native code via JNI; native = memory corruption territory. Do: add a tiny NDK C function to your hello (returns a string); observe the JNI bridge; write a native `strcpy` overflow in your C, crash it (emulator, guarded), see the tombstone in `adb logcat`/`/data/tombstones`.
Verify: tombstone captured with fault address + your overflow size visible.
**Lesson check:** why does native code expand the attack surface beyond Java?

## D7 — your first controlled crash (arm64 local)
Concept: overwrite the saved return address → control `pc`. Do: compile your D6 crash with `-fno-stack-protector -no-pie`, run under `lldb` (or `gdb`-equivalent) on emulator, overwrite the link register / pc with an address YOU choose; controlled crash proven (same skill as Apple course A6 — transfer it).
Verify: lldb shows pc (or x30) equal to your chosen value.
**Lesson check:** which saved value must be overwritten for control flow, and why x30 on arm64?

## D8 — the mitigations that stopped you
Concept: canary, PIE/ASLR, RELRO (GOT hardening), then SELinux as the *kernel-side* fence on top. Do: rebuild D7 WITH canary+PIE → crash changes; `cat /proc/<pid>/maps` on your app → ASLR randomization visible across runs; one local bypass: leak the canary in your own debug session and defeat it.
Verify: before/after table (`notes/d8.md`) — mitigation → symptom → your bypass.
**Lesson check:** one line each — canary, ASLR, RELRO, and why SELinux still limits a fully exploited app.

## D9 — kernel connect: Android IS Linux
Concept: the real prize is the kernel — Android's kernel is Linux with vendor patches (check gs/android.golang... no — AOSP kernel repos / Android Common Kernel). Do: on your emulator, `adb shell uname -a`, walk `/proc`; port your xv6 kernel-lab (01-xv6) exploit or the Linux-kernel CVE of your choice's *class* into a QEMU Linux VM (deeper: 05-01..05); read where Android's Linux diverges (binder, ashmem historic).
Verify: kernel-version string found + one kernel-context exercise solved in VM.
**Lesson check:** same overflow in userland vs kernel — what changes about consequences and mitigations?

## D10 — boot chain & Verified Boot
Concept: bootloader → AVB (Android Verified Boot) → dm-verity (read-only verified partitions) → vbmeta. Root must defeat or board this chain. Do: emulator: `adb shell avbctl get-verity` (or boot log walk on emulator), find dm-verity config; read your device/AVD's bootloader-lock status (`fastboot oem device-info` on the emulator image if allowed — else document from source): study Android's boot stage diagram.
Verify: you name each link and what it verifies, from source-produced artifact or boot log.
**Lesson check:** what does dm-verity protect and what happens the moment a root tampers a system partition?

## D11 — your root-primitive re-creation (the honest lab)
Concept: competence = re-create. Do: on your own device (or fully-documented emulator), re-write the *class* of a public root primitive (KernelSU-init-style or a documented vendor-gap/CVE class) from the description — NOT source. Wi-Fi off; device wiped after.
Verify: `labs/d11.md` — primitive ran (or denial+root-adjacent evidence), stage-by-stage notes.
**Lesson check:** stages of your primitive and why that order.

## D12 — CAPSTONE: hello world → root, in your words
Prereq: D0–D11 passed. **Close all notes.** Write `labs/android-capstone.md`: disclosure-grade narrative of YOUR arc — hello (D0) → tampered APK (D2) → uid sandbox (D3/D5) → first controlled crash (D7) → canary bypass (D8) → kernel context (D9) → AVB chain (D10) → your re-created primitive (D11) — plus mitigation-bypass ordering and patch timeline you'd propose. Re-run D11's PoC once, cold.
**Pass = narrative accurate with artifacts referenced, PoC runs without notes.** Then re-open docs → JAILBREAK-ROOT-PATH Lane B R4.

---

## Rules
1. Verification + lesson check both true before next unit; quizzes in own words.
2. Copying allowed only in D0/D1 tool input (apksigner/aapt command shapes) — everything else from concept; erase-and-retry once when stuck.
3. 2h/unit timebox; stuck past that = previous unit's verification again.
4. Emulator-first, own device only for D10/D11, Wi-Fi off, wipe after runs.
5. Honest bar: a shipping-Android 0-day is career research. This course's bar = you can build, sign, tamper, decompile, trace, sandbox, crash, bypass, boot-image-audit, and re-create a public root primitive — the competence floor for Android security roles. Proven cold at the capstone.