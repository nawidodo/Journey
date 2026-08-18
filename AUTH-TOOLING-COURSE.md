# Auth-Tooling Course — Absolute-Beginner (hello world → write your own Mimikatz-class & Impacket-class tools, gated)

Zero Windows-protocol knowledge assumed. You need: the Windows VM + Domain Controller lab from `WINDOWS-SECURITY-COURSE` N10/N11 (or build it new: eval Windows Server + eval Windows 10, one private network). Each unit: concept → do → runnable verification → **lesson check (own words, `notes/hN-quiz.md`)** — no advance without both. ~2–3h/unit, 12 units + capstone ≈ 7–9 weeks. This is the *research* side of the N-course's *usage* side: instead of running Mimikatz/Impacket, you write the protocol messages yourself and understand exactly what the tools do.

Compass (re-read when lost): all of this is **protocols + secrets**: Windows authentication is Kerberos + NTLM; secrets live in LSASS/SAM/DPAPI; tools are just people who read the specs and spoke the wire format. You'll speak: ND4 (password hash), NTLMSSP (challenge/response), AS-REQ/AS-REP + TGS (Kerberos tickets), SMB2 (file transport), MSRPC (remote calls). Every unit = one wire message you built with your own hands.

Safety & ethics (THE line, above all others on this course): **everything runs against your own lab domain and your own VM credentials; you use obviously-fake lab passwords (`LabPass!2026`); you never touch a credential that isn't yours, a server that isn't your VM, or a password you didn't invent for the lab.** Real-world tool use requires authorization — this course builds understanding, not authorization. No real passwords or hashes ever get committed to this repo.

---

## H0 — what a credential even is
Concept: passwords → plaintext, NT hash (MD4 of UTF-16LE), LM hash (legacy); machines keep them in LSASS (memory), SAM (offline), DPAPI (app data), and the domain's DCs. Do: inventory on your lab VMs: `whoami /all`, `reg save` nothing yet — instead READ where each storage lives (docs + your VM's Process Explorer listing of lsass.exe; DMP. of `efs`/`dpapi` paths); write a one-page storage matrix.
Verify: storage matrix names each store + custodians (who can read it) per store.
**Lesson check:** why do hashes matter if they aren't the password — and what can an attacker do with a hash alone?

## H1 — the NT hash, computed by you
Concept: NT hash = MD4(UTF-16LE(password)); it's one-way but crackable offline. Do: implement the NT-hash function in C/Python (MD4 — implement or call crypto lib, but write the encoding + hex output yourself) and your own offline brute-forcer over a 100-password lab wordlist; verify against known vectors; crack the hash you made for `LabPass!2026`.
Verify: your function matches known NT-hash test vectors; your cracker recovers your lab password.
**Lesson check:** offline vs online attacking — what's the difference in *what you need* (a hash vs a server) and in traces you leave?

## H2 — NTLM challenge/response, spoken by you
Concept: NTLMSSP = negotiate → challenge → authenticate; client proves knowledge of the NT hash by encrypting the challenge. Do: implement the three messages (Python socket) and authenticate to your OWN Windows VM's file share (host share, your lab creds); capture your own traffic (Wireshark loopback/LAN) and align your messages with the capture.
Verify: your NTLM build opens the lab share; capture shows YOUR three messages aligned.
**Lesson check:** why is NTLM a "challenge/response" — and what replay-style weakness does the design enable (NT hash vs password)?

## H3 — Kerberos: AS-REQ → TGT (your own ticket, hand-built)
Concept: domain logon = KDC issues TGT for your account: AS-REQ (pre-auth encrypted with your key) → AS-REP (the TGT). Do: implement AS-REQ with pre-auth timestamp + RC4-HMAC (or AES) using your lab user, get your own TGT from YOUR DC, parse the ticket fields (realm, kvno, cname); verify against `klist` on the VM.
Verify: your hand-built AS-REQ returns a parsed TGT (ticket shown field-by-field).
**Lesson check:** what does pre-auth encryption prove to the KDC — and what happens to an account that skips it (AS-REP roast, H4)?

## H4 — Kerberoast & AS-REP, your own lab version
Concept: SPN accounts: any user can ASK the DC for a service ticket encrypted with the *service's* key — offline crackable. Do: create a lab SPN user (service account `svc_lab`), request its ticket yourself with your H3 code (TGS-REQ), extract the encrypted part, crack it offline with your H1 cracker; also set an AS-REP-roastable account and pull that.
Verify: your own ticket request + crack recovers the lab password; both techniques in `labs/`.
**Lesson check:** why is "any authenticated user can request a service ticket" a feature that becomes an attack — and which setting (preauth, strong keys) closes it?

## H5 — secrets dump, your own manner (offline side)
Concept: with admin (N11's win), the SAM + SYSTEM hives contain local-account hashes; dumping = copy hives, decrypt offline. Do: on your VM: `reg save` the SAM/SYSTEM hives (lab VM, admin), write your own parser that extracts boot-key-decrypted local account hashes (reimplement the syskey/boot-key decryption with your own code — that's the Mimikatz secretsdump core); verify hashes match `net user` password test.
Verify: your parser prints local-account NT hashes matching your lab setups.
**Lesson check:** why does local-admin access make hashes recoverable offline — and what's the one thing (LAPS/remote, strong local secrets) that breaks the chain?

## H6 — LSASS, the live-memory crown (mimikatz's home)
Concept: LSASS holds domain user tokens/Kerberos material/running-wdigest-era plaintext; reading it = OpenProcess + ReadProcessMemory + pattern-scan. Do: ON YOUR VM (Defender ON, admin): attempt the classic read of lsass.exe; if LSA Protection/Cred Guard blocks it — that IS the lesson: capture the block, document the memory structures you'd scan for (msv1_0, kerberos, wdigest) from public structure docs, and write the read attempt + analysis; if your build allows partial read, extract and redact immediately.
Verify: `labs/h6.md` — attempt documented, structures named, block-or-partial outcome recorded (honest either way).
**Lesson check:** why do modern Windows fight LSASS reading (Credential Guard, LSA protection) — and where did those fights move the attack (to the DC, the eaps, the backups)?

## H7 — DPAPI: decrypt your OWN lab secrets
Concept: DPAPI protects app secrets with user-key + master key; same-user access decrypts. Do: in your VM profile: store a test password in a small C#/PowerShell DPAPI-protected blob, then decrypt it with the same user's DPAPI call; then find a real target in YOUR lab (a browser saved-password file from your own lab profile or your own test app) and decrypt with your own master-key + session-key program.
Verify: your own DPAPI blob round-trips; your lab target's secret recovered + redacted.
**Lesson check:** what does DPAPI assume about "same user" — and what does that mean when an attacker already has user code execution?

## H8 — SMB2, wire-built (the Impacket moves)
Concept: SMB2 = negotiate → session-setup → tree-connect → read: the file-transport protocol of Windows. Do: implement a raw SMB2 client (Python, no library): negotiate, NTLM session setup (reuse H2), tree connect to your lab share, list + read one file — every packet your own.
Verify: your SMB2 client lists and reads from the lab share (sid/setup fields visible in your logs).
**Lesson check:** why is SMB "the" Windows network protocol — and what makes it the prime remote-attack surface (port 445)?

## H9 — MSRPC: one interface by hand (share enumerator)
Concept: Windows admin services expose MSRPC interfaces (SRVSVC, SVCCTL); calling them = bind + request. Do: implement an MSRPC bind + one SRVSVC call (`netr_ShareEnum`) over SMB named pipe on your VM pair; parse the share list. This is the smallest honest slice of Impacket's `rpcclient`.
Verify: your own RPC call returns the lab's share list.
**Lesson check:** what is an "interface" in MSRPC terms — and why does exposing RPC over srvsvc make "list all shares" possible remotely?

## H10 — remote exec, your own psexec-style
Concept: psexec = push binary/service over admin share + SVCCTL start + named-pipe output. Do: in the lab (with the admin creds you legitimately hold in the lab): implement the psexec pattern — copy your own trivially-signed exe to ADMIN$ share via your H8 SMB client, create/start the service via your H9 SVCCTL path, read output; clean up the service after. All your own code.
Verify: your own psexec-style run executes a command on the VM and returns output + cleanup done.
**Lesson check:** service-creation-as-exec — why does SVCCTL become code execution, and what did cleanup prove about operational discipline?

## H11 — the defensive lens: catch your own work
Concept: the tools' shadows are the detections: Sysmon event IDs, LSASS access (Event 10), Kerberos golden/roast patterns, SMB admin$ traffic, service creation (4697/7045). Do: from EDR-COURSE T7/T8 skills: write/implant detection rules (Sigma) for H4, H5, H10 artifacts; run them against your lab; tune; table the result.
Verify: your rules catch your own H4/H10 runs; false-positive check on clean baseline.
**Lesson check:** which of your H-units left the most telemetry — and what does that say about the trade secret of operational security (opsec ≠ magic)?

## H12 — CAPSTONE: one research tool, in your words
Prereq: H0–H11. **Close all notes.** Write ONE coherent tool (own code) that chains the lab-owned pieces: hash a target (H1) → ask your DC for the SPN ticket (H4) → crack it (H1 cracker) → read a lab share (H8) → execute remotely (H10) — with a `--lab-only` banner, target validation, and its own detection-notes section (H11). Write `labs/auth-capstone.md` like an open-source tool README: architecture, usage on the lab, exact ethics line, and the three features you'd add next (e.g., AES Kerberos, DPAPI parsing, LAPS awareness). Re-run the full lab chain once, cold.
**Pass = the chain runs in the lab without notes and the README reads like a research tool's, ethics included.**

---

## Rules
1. Verification + lesson check both true before next unit; quizzes in your own words — protocol work punishes hand-waving with silent failures.
2. Copying allowed only in H0/H8 boilerplate (socket scaffolding, struct layout from specs) — every message built and parsed by you; erase-and-retry once when stuck.
3. 2–3h/unit; stuck past that = previous unit's verification again.
4. **Lab-only hardline: your VMs, your invented credentials (`LabPass!2026` style), your own DC; no real account, no real network, no real data — ever. No real hashes/passwords committed to the repo (redact or use dummy).** Defender stays ON (H6's block is a lesson). Snapshots before H5/H6.
5. Honest bar: Mimikatz/Impacket are product-years written by researchers; this course's bar = you hand-built the core wire messages (NTLM, Kerberos, SMB, MSRPC), re-created the classic techniques on your own lab, and wrote your own detections — the floor for protocol research, red-team tooling, and detection engineering, proven cold at the capstone.

## Where this lives
Pairs with `WINDOWS-SECURITY-COURSE` N10/N11 (usage side) and `EDR-COURSE` T7/T8 (defense side) — research, attack, and detect built by the same hands on the same lab.