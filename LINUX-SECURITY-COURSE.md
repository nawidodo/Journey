# Linux Security — Absolute-Beginner Course (hello world → kernel-level control, gated)

Zero Linux-security knowledge assumed. You need: a Mac/Linux host and a virtual machine (QEMU or UTM, free) — every unit runs in the VM so you can wreck it freely and snapshot-restore. Each unit: concept → do → runnable verification → **lesson check (own words, `notes/kN-quiz.md`)**. No advance without both. ~2h/unit, 12 lessons + capstone ≈ 4–5 weeks. When done you enter [`DESKTOP-OFFENSIVE-PATH.md`](DESKTOP-OFFENSIVE-PATH.md) at W1 — and because Android's kernel IS Linux, this course is also the shared floor for the Android lane.

Compass (re-read when lost): Linux security = **user/kernel mode split → memory corruption in userland → mitigations → kernel module surface → kernel exploits → mandatory access control (LSM)**. Every step asks "which side of a boundary am I crossing, and what enforces it?" The kernel is the prize because userland bugs are game-overs for one process; kernel bugs are game-overs for the machine.

Safety: all attacks against your own VM (snapshot first, restore after); re-create public classes (Dirty COW/Pipe, SUID misconfig) — never against other systems; no 0-day hunting; the kernel is the point, not your hardware.

---

## K0 — hello world inside your first Linux VM
Concept: a Linux box you own, from the console up. Do: create VM (QEMU/UTM, any distro — Debian or Ubuntu), boot to shell; write hello.c with a text editor; `gcc -o hello hello.c && ./hello`; learn `ls -l`, `file hello`, `readelf -h` (ELF 101: magic, entry point, program headers).
Verify: hello runs and `readelf -h` output read aloud (you know the entry address).
**Lesson check:** ELF file vs running process — where does one end and the other begin?

## K1 — processes: the unit of isolation
Concept: fork/exec, pid/uid/gid, /proc = the kernel's self-catalog. Do: in VM: `ps aux`, `cat /proc/self/status` (see your own pid/uid), `cat /proc/cpuinfo`; write a tiny proclist tool (read /proc, print names) in C; kill a process with `kill`.
Verify: your tool lists processes and you killed one you spawned.
**Lesson check:** what protects processes from each other at the OS level, before any exploit?

## K2 — memory: stack, heap, ASLR seen live
Concept: stack (locals, saved return), heap (malloc), and ASLR randomizing it all. Do: two runs of a program printing `printf("%p")` addresses of a local and a malloc'd buffer → addresses differ per run = ASLR; compile with `-fno-pie` → compare; write a `strcpy` overflow crash (VM, guarded).
Verify: you can show ASLR moving stack+heap addresses between runs (before/after table).
**Lesson check:** which addresses ASLR randomizes, and which (if any) stay fixed?

## K3 — syscalls: how userland talks to the kernel
Concept: libc wraps syscall numbers; the kernel executes them on your behalf. Do: `strace ./hello` (every syscall printed — find execve/write/exit); raw syscall in C: `syscall(SYS_write, 1, "hi\n", 3)`; read `man 2 syscalls`; see the vdso/vsyscall pages in `/proc/self/maps`.
Verify: strace output matches your code's calls; raw write prints without libc's `printf`.
**Lesson check:** the journey of one `printf` to the kernel — name every layer it crosses.

## K4 — the loader: ELF, dynamic linking, LD_PRELOAD
Concept: loading = mapping segments, resolving symbols; dynamic libs + symbol interposition. Do: `readelf -l` (segments → /proc maps mapping), `ldd hello`, `objdump -d` (hello's main disassembly); write parser-lite in C: open hello, print magic (0x7f 'E' 'L' 'F'), entry, program-header count — matches readelf; then `LD_PRELOAD` your own lib that wraps `puts` (print EXTRA line before real puts) on hello.
Verify: parser output matches readelf on the same file; your preload wrapper prints on hello's run.
**Lesson check:** what does the dynamic loader do, and why has LD_PRELOAD always been the easiest offensive injection?

## K5 — your first exploit: stack overflow → control RIP
Concept: overflow the stack → overwrite saved return → CPU jumps where you say. Do: vulnerable C (`strcpy`, `-fno-stack-protector -no-pie`), gdb: break at return, see saved RIP, overwrite it with your chosen address, crash lands there → *controlled crash*; then with NX off, classic ret2shellcode in VM (spawn a shell) — the historical win.
Verify: gdb shows RIP equal to your chosen value; ret2shellcode spawns your shell.
**Lesson check:** which saved value must be overwritten for control-flow hijack, and what was the "shellcode" you executed?

## K6 — the mitigations, then beat them all locally
Concept: canary (stack sentinel), PIE (ASLR for code), NX (no exec stack/heap), RELRO (GOT hardening). Do: rebuild K5 with each on; crash changes per mitigation; then the full local chain: leak canary via format-string bug → leak libc base via GOT read → ret2libc `system("/bin/sh")` — all on your VM.
Verify: `/bin/sh` spawned via the full leak chain; mitigation-symptom-bypass table in `notes/k6.md`.
**Lesson check:** one line each — canary, PIE, NX, RELRO — what each breaks, and which your chain defeated in order.

## K7 — the kernel becomes yours: write a module
Concept: a kernel module runs in kernel mode — your first kernel code. Do: from 04-03: module source `hello_module` (init/exit), Makefile, `insmod`/`rmmod`, `dmesg` shows your strings; module that reads `/proc` data and prints it; `lsmod` shows yours.
Verify: your module's dmesg output + lsmod entry, loaded and unloaded by you.
**Lesson check:** user mode vs kernel mode — what does being in kernel mode let your module do that userland can't?

## K8 — kernel exploitation primer (own VM, public classes)
Concept: same corruption, kernel consequences. Do: re-create a public kernel bug class on your VM — Dirty COW (page-cache race) or Dirty Pipe (splice) — from the *description*, run it, gain euid 0 on your VM; also do the xv6 lab exploit (01-xv6) for the minimal case; read how modprobe_path trick turns write-anywhere into root.
Verify: your re-created PoC prints uid 0 (root) with logs in `labs/`.
**Lesson check:** what did the bug corrupt, and why did that corruption equal root rather than just a crash?

## K9 — privilege escalation everywhere else
Concept: root is a goal; SUID, capabilities, and misconfig are the ladders. Do: find SUID files (`find / -perm -4000`), write a deliberately-vulnerable SUID helper in VM, privesc via it; capability experiment (`setcap cap_net_raw` on your tool, `getcap`); namespace/cgroup isolation demo (your own `unshare` sandbox).
Verify: SUID privesc PoC runs (uid 0 in VM); capability list read.
**Lesson check:** SUID vs capabilities — the same "extra privilege" idea under two mechanisms; say the difference.

## K10 — the mandatory fences: LSM, SELinux, AppArmor
Concept: even root is confined by labels. Do: on your VM, enable AppArmor (or study SELinux on a Fedora/CentOS VM): profile a test binary, watch its denials in dmesg; run a container (podman/docker) and show a mount/process that the container cannot reach — temporary root in container ≠ host root.
Verify: denial logged for your confined binary; container boundary demonstrated.
**Lesson check:** what does an LSM add beyond classic root/permissions, and why is "root in a container" still not host root?

## K11 — as the defender: see the rootkit
Concept: attackers hide; defenders compare. Do: read 18-01 rootkit theory; on your VM plainly observe the kernel's syscall table via a read-only walk (`/proc/kallsyms` or a module reading the table); write a detection: compare what `ps` shows vs raw /proc enumeration (fake "hidden process" via your own mini-rootkit-lab on VM) — your detector catches the mismatch.
Verify: your detector flags your lab hidden-process; writeup of the mismatch signal.
**Lesson check:** what is the fundamental signal every hidden-process technique must leave?

## K12 — CAPSTONE: hello world → kernel-level control, in your words
Prereq: K0–K11 passed. **Close all notes.** Write `labs/linux-capstone.md`: disclosure-grade narrative of YOUR arc — hello (K0) → process model (K1) → ASLR live (K2) → syscall layer (K3) → loader+LD_PRELOAD (K4) → first exploit (K5) → full mitigation chain (K6) → your module (K7) → kernel PoC root (K8) → SUID/caps (K9) → LSM fences (K10) → detection (K11) — plus mitigation-bypass ordering and the patches you'd ship. Re-run K8's PoC once, cold.
**Pass = narrative accurate with artifacts referenced, PoC runs without notes.** Then re-open docs → DESKTOP-OFFENSIVE-PATH W1 (and your Android lane's kernel units were pre-warmed by K7/K8).

---

## Rules
1. Verification + lesson check both true before next unit; quizzes in own words.
2. Copying allowed only in K0/K7 boilerplate (Makefile shape, module skeleton) — everything else from concept; erase-and-retry once when stuck.
3. 2h/unit timebox; stuck past that = previous unit's verification again.
4. Snapshots: VM snapshot before every kernel/exploit unit; restore after. Own hardware only; re-create public classes only.
5. Honest bar: a shipping-Linux 0-day is career research. This course's bar = you can build, trace, parse, inject, crash, defeat every mitigation, write kernel code, re-create a public root, and detect hiding — the competence floor for Linux security work, proven cold at the capstone.

Kernel exploitation deep-dive (bugs YOU plant in YOUR module, in YOUR VM): [`KERNEL-EXPLOIT-COURSE.md`](KERNEL-EXPLOIT-COURSE.md) X0–X12.