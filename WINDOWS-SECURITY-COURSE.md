# Windows Security — Absolute-Beginner Course (hello → kernel exploit → Active Directory, gated)

Same Windows VM lab but malware/C2-focused twin course: [`WINDOWS-MALWARE-COURSE.md`](WINDOWS-MALWARE-COURSE.md).
Research side — write your OWN Mimikatz/Impacket-class tools instead of running them: [`AUTH-TOOLING-COURSE.md`](AUTH-TOOLING-COURSE.md).

Zero Windows-security knowledge assumed. You need: a host with 16+ GB RAM and two VMs — one Windows 10/11 eval (free from Microsoft, iso) and one Windows Server eval (for the Active Directory units); UTM/QEMU/Parallels all work. No GPU needed. Each unit: concept → do → runnable verification → **lesson check (own words, `notes/nN-quiz.md`)**. No advance without both. ~2h/unit, 12 lessons + capstone ≈ 5–6 weeks. When done you enter [`DESKTOP-OFFENSIVE-PATH.md`](DESKTOP-OFFENSIVE-PATH.md) at W5 with the HEVD ladder pre-warmed.

Compass (re-read when lost): Windows security = **token (who you are) + ACLs (what you may) + integrity levels (the fence) + the kernel/driver plane (the prize) + Active Directory (the kingdom behind the workstation)**. Cross one layer at a time; kernel bugs and AD misuse both end in "control the whole machine/domain".

Safety: eval ISOs only; your own VMs only, snapshots before every exploit unit and restore after; the AD lab is a private domain (isolated virtual network, host firewalled off); re-create public classes — never attack other systems. All tools run inside the lab.

---

## N0 — hello world and the PE you just made
Concept: Windows runs PE files; even hello is signed/layered. Do: install Windows eval VM → PowerShell `'hello'` → C hello via Build Tools (or MinGW) → `dumpbin /headers hello.exe` (or `objdump -x`) → see DOS header, PE magic, entry point, section table → `Get-AuthenticodeSignature` on a system exe.
Verify: you read your hello's PE headers aloud (entry point + section names).
**Lesson check:** PE file vs running process — what maps in, what runs?

## N1 — processes, tokens, integrity
Concept: every process carries a token (user SID + privileges) and an integrity level (the real fence on modern Windows). Do: `tasklist` / `Get-Process`, `whoami /user /groups /priv` (your SIDs + privileges), `icacls` on a folder, check your UAC/integrity: `whoami /groups | findstr Integrity` — Medium vs High.
Verify: you read and name your own token's SIDs + privileges + integrity level.
**Lesson check:** token vs ACL vs integrity level — disentangle the three.

## N2 — PE layout: read the binary yourself
Concept: PE = headers + sections (`text` code, `data` globals, `rdata` constants/imports); RVA math. Do: write parser-lite (PowerShell or C): open hello.exe, print magic `MZ`, PE signature `PE\0\0`, machine type, section count + names + sizes — must match dumpbin; then dump the import table (`data-directory[1]`) and name the DLLs hello imports.
Verify: parser output matches dumpbin on the same file.
**Lesson check:** what lives in .text vs .rdata, and why do imports live in a table?

## N3 — the native layer: ntdll, syscalls, API hooking
Concept: Win32 APIs wrap native syscalls in ntdll; every API call is an attack surface. Do: `procmon` (free Sysinternals) trace of hello — see registry/file/network events; find ntdll's `NtCreateFile` stub (x64dbg or dumpbin disasm); call one native API from your C directly (`NtQuerySystemInformation` via ntdll) and print output.
Verify: procmon trace read; your native call prints real system data.
**Lesson check:** Win32 API vs ntdll native API — the call chain and why hooking ntdll is the classic evasion/injection point.

## N4 — memory and your first controlled crash
Concept: stack overflow → overwrite saved return → control RIP (x64: the return slot). Do: vulnerable C (`strcpy`, build with `/GS- /DYNAMICBASE:NO`), run in VM, crash; open in x64dbg, set breakpoint at the return, overwrite the saved return-address slot with YOUR address; crash lands there — controlled crash.
Verify: x64dbg shows RIP equal to your chosen value.
**Lesson check:** what must be overwritten for control flow on x64, and where does it live on the stack?

## N5 — the mitigations, and what each kills
Concept: /GS canary, /DYNAMICBASE (ASLR), /NXCOMPAT (DEP), CFG (indirect-call checks). Do: rebuild N4 with each flag on; crash changes per flag (tables in `notes/n5.md`); reproduce the *same* control with `/GS- /DYNAMICBASE:NO /NXCOMPAT:NO` → legacy-style overflow wins on your VM only.
Verify: mitigation-symptom table; your controlled crash works on the all-offs build only.
**Lesson check:** one line each — GS, ASLR, DEP, CFG — what each prevents.

## N6 — tokens and privilege escalation before the kernel
Concept: root on Windows isn't uid 0 — it's Tokens/privileges + service/ACL misconfig. Do: `whoami /priv` (SeDebugPrivilege story); in VM: set up a deliberately-weak service (service running as SYSTEM, weak DACL) → `sc qc`, modify its binary path, start it → your process runs as SYSTEM (classic privesc, own VM); read Token-theft theory (Mimikatz-era).
Verify: your service-trick yields a SYSTEM shell in the VM with logs.
**Lesson check:** what is a token, and why does a weak service = your code running as SYSTEM?

## N7 — the kernel plane: driver dev and debugging
Concept: drivers run in kernel mode; kernel debugging = your window into the plane. Do: build a minimal Kernel-Mode driver skeleton (KMDF via WDK, test-signed, `sc` + `fltmc` to load / `dmesg`-view via DebugView or WinDbg); set up kernel debugging (WinDbg + VM serial/network; if your hypervisor can't wire it, this unit's kernel-debug setup is a documented study note with the driver build as the hard artifact).
Verify: your driver loads on the VM (test-signing on) and prints a hello to the debug channel.
**Lesson check:** user mode vs kernel mode — what does a driver get that userland cannot?

## N8 — kernel exploitation: HEVD, the standard ladder
Concept: a vulnerable driver = kernel bug = SYSTEM. Do: install HackSysExtremeVulnerableDriver (HEVD) on your VM; exploit its stack-overflow with token stealing (classic: steal System token, restore process token) — write your own PoC from the concept; then read one real Windows kernel CVE class (pool overflow) writeup.
Verify: your PoC prints SYSTEM shell (or equivalent) in the VM, logs in `labs/`.
**Lesson check:** what did the driver bug corrupt, and why did that equal SYSTEM instead of a crash?

## N9 — the blue side of your own work
Concept: ETW + event logs + AMSI = what catches you. Do: enable ProcessAudit/CmdLine on the VM, run your N8 exploit, find the events it generated (4688/4690 class, ETW provider traces); write one PowerShell/ETW-based detection rule for the artifact your exploit leaves.
Verify: your detection catches your own N8 activity (before/after).
**Lesson check:** what telemetry survives a successful kernel exploit — and what does that mean for defenders?

## N10 — Active Directory: build the kingdom
Concept: AD = domain + Kerberos (AS-REQ/AS-REP, TGS) + tokens entrusted via delegation; the DC is the root of trust. Do: second VM = Windows Server eval, promote to Domain Controller (forest `corp.local`), join the Win10 VM to the domain, create users/groups/OUs; read the Kerberos flow from klist traces (`klist`, `klist tgt`) and Golden-ticket theory.
Verify: domain joined; klist shows your TGT/TGS tickets; you name the Kerberos steps.
**Lesson check:** what does the DC's access to every ticket mean for anyone who controls the DC?

## N11 — Active Directory: attack your own kingdom
Concept: AD attacks abuse protocol design (and config drift). Do: in the lab: sharp/BloodHound-equivalent enumeration (`bloodhound-python` on the DC VM's API) → find a good path; Kerberoasting (a service account YOU created with SPN, crack its hash offline on your host); pass-the-hash class on your own accounts; then a Golden Ticket minted with the DC's krbtgt hash — all in the lab.
Verify: Kerberoast hash → cracked; golden ticket grants domain admin in lab with evidence.
**Lesson check:** which attack abused which design choice (SPN hashes, TGT reuse, krbtgt single key)?

## N12 — CAPSTONE: hello.exe → SYSTEM → domain, in your words
Prereq: N0–N11 passed. **Close all notes.** Write `labs/windows-capstone.md`: disclosure-grade narrative — PE anatomy (N0/N2) → native layer (N3) → first control (N4/N5) → token privesc (N6) → driver plane (N7) → HEVD SYSTEM (N8) → your detection (N9) → AD kingdom (N10) → your domain attacks (N11) — plus mitigation-bypass order and patches/AD-hardening list you'd ship. Re-run N8 and one N11 attack once, cold.
**Pass = narrative accurate with artifacts referenced; both re-runs succeed without notes.** Then re-open docs → DESKTOP-OFFENSIVE-PATH W5 (HEVD ladder depth) and W7.

---

## Rules
1. Verification + lesson check both true before next unit; quizzes in own words.
2. Copying allowed only in N0/N7 boilerplate (build flags, driver skeleton) — everything else from concept; erase-and-retry once when stuck.
3. 2h–3h/unit; stuck past that = previous unit's verification again.
4. Snapshots before N4/N5/N8/N11 and restore after; AD lab on an isolated virtual network; eval ISOs; public classes only; never attack systems outside the lab.
5. Honest bar: shipping-Windows/AD 0-days are career research. This course's bar = you can build, parse, trace, hook, crash, defeat flags, escape tokens, load kernels drivers, re-create HEVD, detect yourself, stand up a domain, and re-run two attacks cold — the competence floor for Windows/AD security roles.