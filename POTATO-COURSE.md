# Potato Course — Absolute-Beginner (hello world → write your OWN Potato-family privilege-escalation tool, gated)

Zero knowledge assumed, but it lives in the Windows world: you need the Windows Server VM + Windows 10 VM lab from `WINDOWS-SECURITY-COURSE` (an **era-correct Windows Server 2016/2019 eval** — the classic vectors need a live spooler and pre-2022 hardening; the course also teaches the modern landscape). Each unit: concept → do → runnable verification → **lesson check (own words, `notes/oN-quiz.md`)** — no advance without both. ~2h/unit, 12 units + capstone ≈ 6–7 weeks. This is the *token-privesc specialization*: every Windows offense between "service account" and "SYSTEM".

Compass (re-read when lost): the Potato family is ONE idea, five spellings — **"trick a SYSTEM component into touching a resource you control, then impersonate it."** If you hold the SeImpersonatePrivilege (standard for service accounts), the OS lets your thread borrow the caller's token. The game is engineering the SYSTEM caller. Each unit adds one vector: COM activation (Rotten/Juicy), named pipes (PrintSpoofer/Efs), token plumbing (God), and the detection that ends them.

Safety & ethics (same hardline as the sibling courses): lab VMs only — Server eval + Win10 eval on an isolated network; you set up the vulnerable service accounts; era-correct unpatched VMs are FOR learning, never connected to anything real; no real accounts/data; tools are lab tools with a `--lab-only` requirement; no real hashes/passwords in the repo.

---

## O0 — hello world: the token, read deeply
Concept: every process runs with a token: SIDs, privileges, integrity level, token type (primary vs impersonation). Do: in the lab: `whoami /all` on the Win10 VM; enumerate privileges — find SeImpersonatePrivilege and SeAssignPrimaryTokenPrivilege (which accounts have them and why service accounts do); use Process Explorer/tokens on your own VM to compare an admin token vs a service token.
Verify: you explain which of YOUR lab tokens carries SeImpersonate and why.
**Lesson check:** token vs identity — what's in a token beyond "who am I", and why do service accounts ship with the two privileges that potato tools need?

## O1 — the account you'll own: set up the LocalService lab
Concept: potato tools run as a service account with SeImpersonate and try to reach SYSTEM. Do: create a throwaway service on the Server VM (`sc create labsvc` + `sc config ... obj= LocalService`) with a test binary you wrote (echoes whoami + whoami /priv); start it; capture its token view — you now have a lab shell as LocalService with SeImpersonatePrivilege.
Verify: your service runs as LocalService and shows SeImpersonate in its own `whoami /priv` output.
**Lesson check:** why does a service account get SeImpersonate — what is it FOR — and how does "for its job" become "for SYSTEM"?

## O2 — impersonation by hand: named-pipe client-server
Concept: a server thread can impersonate the client of a named pipe: ImpersonateNamedPipeClient → your thread NOW runs with the client's token. Do: write pipe server + pipe client (C/Python): server creates \\.\pipe\labpot, client connects (from another session on the VM), server impersonates, calls `whoami` via the impersonated token — no SYSTEM yet, but the MECHANISM proven; then type-convert the token (DuplicateTokenEx) and reuse it on another thread.
Verify: your server's impersonated `whoami` shows the CLIENT's identity (logs both).
**Lesson check:** what does ImpersonateNamedPipeClient actually swap — and why must an attacker hold SeImpersonatePrivilege for this to be dangerous?

## O3 — the COM activation seed (RottenPotato's core)
Concept: DCOM/RPCSS activates components on demand, and by default in the *caller's* impersonation context — if the caller is SYSTEM, the activation work happens as SYSTEM. Do: read the RottenPotato-era writeups; in the lab from your O1 service account: call `CoCreateInstance` for a lab/allowed CLSID and observe which token the activation path uses (trace with Process Monitor/procmon on the VM); document the ORPC activation handshake (OXID resolver, identity) with your own diagram.
Verify: `labs/o3.md` — diagram + procmon trace showing activation under your service token.
**Lesson check:** why did COM "activate as caller" become a privilege-escalation find — what is the component doing with your token when it runs?

## O4 — the SYSTEM customer: make SYSTEM talk to you
Concept: every potato needs a SYSTEM connection to something you control — the classic: the Print Spooler connects to UNC/named pipes of attackers (Print Spooler bug family). Do: on the era-correct Server VM with spooler running: confirm `spoolsv.exe` is present; from your O1 service account, use the spooler's RPC to make it connect to a pipe you created (the PrinterBug/SpoolSample pattern: call AddPrinterDriver/OpenPrinter with a UNC-ish printer name pointing at your pipe); SYSTEM connects.
Verify: your pipe server logs the spooler's connection arriving as SYSTEM (token inspected).
**Lesson check:** why does the spooler connect out to UNC paths — and what made Microsoft finally disable that vector (2022)?

## O5 — PrintSpoofer-class: pipe + impersonation = SYSTEM
Concept: the completed chain: SYSTEM connects (O4) → you ImpersonateNamedPipeClient (O2) → SYSTEM's token in your thread → spawn cmd as SYSTEM. Do: assemble your own PrintSpoofer-lite: pipe server + spooler-trigger + impersonate + CreateProcessWithTokenW; run from the O1 LocalService shell on the Server VM; note: this vector needs the unpatched era (else you'll write the honest blocker report — that IS a unit outcome too).
Verify: your tool's spawned process shows SYSTEM (`whoami` twice: before/after) with full logs.
**Lesson check:** walk the four-stage chain (trigger → connect → impersonate → spawn) and name the single fix Microsoft shipped that killed this spelling.

## O6 — JuicyPotato idea: COM CLSID rotation
Concept: JuicyPotato automated RottenPotato: rotate COM CLSIDs until one activates as SYSTEM + breeds the impersonation; no spooler needed. Do: on the Server VM from O1: pick the era's known-safe CLSID set, implement activation+pipe+impersonation (your O2+O3 code); run until you get SYSTEM via a CLSID path; document the CLSID you used and why it qualified.
Verify: SYSTEM spawn via your COM-based potato on the lab (or the honest blocker writeup if the era/platform closes it).
**Lesson check:** what did CLSID rotation add over the single-CLSID approach — and what's the 2023-era death of this spelling (COM hardening)?

## O7 — the modern head: GodPotato-class ideas (Kerberos token plumbing)
Concept: modern potato-variants abuse the token-duplication flaws in RPCSS/Kerberos handling (GodPotato: KERB/DPCP + flawed-impersonation token duplication on Windows 7→11-era workstations). Do: study the GodPotato/SweetPotato lineage; on the Win10 VM (era-correct for the flaw or patched — either is a valid lab): attempt the duplication path; capture the EDR/Defender reaction; write the mechanism diagram (token type flags, duplication, privilege check) even where the run fails.
Verify: `labs/o7.md` — diagram + attempt log + the "why this died/won" analysis.
**Lesson check:** what is the flaw class GodPotato hunts — and why do the fixes keep being about *who may duplicate a token*?

## O8 — the plumbing library: tokens as tools
Concept: all potatoes reduce to: get a SYSTEM-adjacent token → duplicate → set thread token → spawn. Do: refactor your O2/O5 code into a small token-helper lib (DuplicateTokenEx, SetThreadToken, CreateProcessWithTokenW, token-info dump); unit-test each call on the lab VM with your own fake client tokens.
Verify: your lib's each primitive has a passing lab test; the whole runs from O1's LocalService shell.
**Lesson check:** name the three Win32 token calls a potato can't do without — and what each permission requirement is.

## O9 — why potatoes die: the hardening history
Concept: every vector died to a patch: spooler UNC removal (2022), COM hardening (2023), RPC restrictions, service isolation, LSA protection walls. Do: read the Microsoft fixing-commits/WDAC-era guidance; build the vector-evolution matrix (vector → trigger → patch → successor) for Rotten/Juicy/Print/Efs/God lineages.
Verify: matrix in `notes/o9.md` covering 6+ vectors with patch names.
**Lesson check:** pick one vector — name its patch and the exact mechanism the patch removed.

## O10 — the blue side: catch your own potato
Concept: potatoes leave telemetry: pipe name creation, spooler RPC to odd names, new process from SYSTEM via service context, CLSID activation anomalies. Do: from EDR-COURSE T7/T8: Sysmon on the Server VM (service running as LocalService), write Sigma rules over your O5/O6 runs; tune until they fire on your potato and stay silent on clean boot.
Verify: your rules catch your own potato runs (before/after table).
**Lesson check:** which potato stage left the loudest telemetry — and what does that say about real-world potato survival times?

## O11 — your toolkit: one CLI, every spelling
Concept: tools are products: one interface, flags, sane defaults, lab guardrails. Do: package O-detections: `pot()` CLI with `--mode pipe|com|clsid`, `--target-vm` guard (refuses non-lab hostnames), `--detected` switch that runs your O10 rules first; test all modes on the Server VM from O1; fail loudly on patched-era (with the blocker message).
Verify: all modes run or fail-with-exact-reason on the lab; README written.
**Lesson check:** what does a lab guardrail (`--target-vm`) say about how a serious tool must be shaped?

## O12 — CAPSTONE: cold, closed, documented
Prereq: O0–O11. **Close all notes.** On the era-correct Server VM (fresh snapshot): deploy your O11 tool as LocalService → SYSTEM via one working mode; then on the Win10 VM reproduce the O2 mechanism standalone (impersonation, no escalation) to prove base understanding; write `labs/potato-capstone.md` like open-source tooling: architecture, lab usage, the O9 evolution matrix, ethics paragraph, and three next features.
**Pass = SYSTEM achieved on the Server lab without notes; writeup reads like a tool maintainer's.**

---

## Rules
1. Verification + lesson check both true before next unit; quizzes in your own words.
2. Copying allowed only in O0/O2 boilerplate (socket scaffolding, token-call signatures) — every trigger, pipe, and token call built by you; erase-and-retry once when stuck.
3. 2h/unit; stuck past that = previous unit's verification again.
4. **Lab-only hardline: era-correct isolated VMs; you create the vulnerable accounts; tools refuse non-lab targets; no real systems, credentials, or data; Defender stays ON (its blocks teach).** Snapshots before O5/O6/O7.
5. Honest bar: real potato tools are researcher years; this course's bar = you hand-built pipe-impersonation, COM-activation escalation, the production of a guarded CLI, the hardening history, and the detections — the floor for Windows token research and privesc engineering, proven cold at the capstone. Modern patched environments are the point of O9/O10: knowing the vector died is as valuable as running it once.

## Where this lives
Pairs with `WINDOWS-SECURITY-COURSE` N6/N9/N11; the same lab serves `AUTH-TOOLING-COURSE` (credentials) and the fleet-automation twin [`NETEXEC-COURSE.md`](NETEXEC-COURSE.md) — token research, credential research, fleet automation, and detection built by one pair of hands.