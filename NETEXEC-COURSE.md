# Post-Exploitation Tooling Course — Absolute-Beginner (hello world → write your OWN netexec-class tool, gated)

Zero knowledge assumed, but this course deliberately **reuses the Windows protocol floor instead of re-teaching it**: complete `AUTH-TOOLING-COURSE` H0–H10 first (NT hash, NTLMSSP, Kerberos, SMB2, MSRPC, psexec) — you will be *coding the tool that uses those messages*. Lab: the Windows Server DC + Win10 VM pair from `WINDOWS-SECURITY-COURSE` N10/N11. Each unit: concept → do → runnable verification → **lesson check (own words, `notes/neN-quiz.md`)** — no advance without both. ~2h/unit, 12 units + capstone ≈ 6–7 weeks.

Compass (re-read when lost): netexec-class tools are **protocol switchers over one mission: "I have credentials — what can they do across this fleet?"** Log in via every transport (SMB, WinRM, LDAP), enumerate what's reachable (shares, users, sessions, SPNs, MSSQL), then execute one thing everywhere — and print it in a table. The product is discipline: tri-state auth results, protocol modules, safe spraying defaults, clean output. Your course builds that product.

Safety & ethics hardline (identical to sibling courses): your lab VMs only, isolated network, invented lab credentials (`LabPass!2026` style), never a real account/host/domain; tools ship with scope guards and `--lab-only`; lockout-aware defaults (a spray that locks accounts is a failed tool, not a feature); no real data in the repo.

---

## NE0 — hello world: what a post-exploitation tool actually is
Concept: scanner ≠ exploiter; netexec = authenticated post-exploitation: credential-in, value-out. Do: read the netexec/CrackMapExec mission + UX (protocol flag, target, cred format — one command shape); write YOUR tool's UX spec: flags, argument parsing, help text, exit codes; stub the CLI with a `--version` and a fake run printing ""lab-only"".
Verify: your CLI parses `-H`/`-u`/`-p`/`--targets` and prints a lab-only banner + exits.
**Lesson check:** what's the minimum a "tool" must decide (inputs, outputs, failure modes) before one line of protocol code?

## NE1 — the credential parsing layer (the boring, critical part)
Concept: tools live or die on credential formats: plaintext, NT hash, NTLMv2 hash, LM:NT pairs. Do: reuse AUTH-TOOLING H1 output; write your own cred parser + loader lib (file/CLI): detect format (length+hex rules), split `user:pass|hash` forms, validate your lab's dummy credentials round-trip; unit-test 10 format cases.
Verify: your parser classifies each test format correctly; invalid creds rejected with clear errors.
**Lesson check:** why does a tool accept 6 credential formats — and what two bugs hide in naive parsing (format misdetect, injection in output)?

## NE2 — the SMB auth engine: tri-state, your own rules
Concept: the netexec bread and butter: attempt Session Setup against each target; the answer is a tri-state — SUCCESS / LOGON_FAILURE / OTHER (disabled, locked). Do: from your H8 SMB2 client: implement the auth check function: try negotiate+session setup with given creds, classify the status code; loop over a target list file; output the classic table (host / user / state / result).
Verify: your engine reports correct tri-state on lab hosts (known-good, wrong-pass, disabled) — 3 hosts.
**Lesson check:** what does STATUS_LOGON_FAILURE vs SUCCESS tell an operator — and why is "OTHER" the case that must be explicit?

## NE3 — enumeration after auth: shares, users, sessions
Concept: valid creds open doors: enumerate what's reachable. Do: from H9's MSRPC: extend to SamrLookupDomain/QueryDisplayInformation (user list via SAMR on the DC) + NetShareEnum (already own) + NetSessionInfo (sessions) on the lab; emit per-protocol structured results into your NE2 table.
Verify: your enum module lists shares/users/sessions on the lab DC with auth, denied without.
**Lesson check:** why enumerate three ways (SAMR/MSRPC/LDAP) for the same "users" — what does protocol choice change?

## NE4 — the WinRM engine: HTTP is a transport too
Concept: WinRM = HTTP + SOAP envelopes + NTLM/Kerberos auth; exec via WSMan shells. Do: implement a mini-winrm client: authenticate over HTTP (NTLM from H2 mechanics), open a shell (`http://host/wsman` create shell), run `whoami`, pull output, close; verify against the lab Win10 host with WinRM enabled.
Verify: your WinRM exec returns output from the lab host (HTTP trace captured).
**Lesson check:** what does WinRM add over SMB exec (firewalls, non-file protocols) — and what telemetry does HTTP leave for your O10/NE10 defense?

## NE5 — exec dispatcher: one command, four transports
Concept: netexec's value per host: same command run via smbexec, wmiexec, psexec, winrm — pick per reachability. Do: wrap your H10 psexec + NE4 winrm into a dispatcher: `run(host, command, transport)` with per-transport result normalization (exit code + stdout); table it across your two lab hosts; add transport-fallback order.
Verify: dispatcher runs the command on both hosts over at least two transports each (table with codes).
**Lesson check:** why four ways to get a shell — and what makes a dispatcher's failure handling the real engineering?

## NE6 — LDAP diet: the DC's directory as intel
Concept: authenticated LDAP against the DC answers "who exists, what SPN, what group" — the netexec ldap module's core. Do: implement bind + search (Python ldap3-free: raw BER is a stretch — instead use a minimal LDAP over your own framing with the `ldap` concept: bind, search base DC, filter users + SPNs); list your lab users + SPN-havers, match against AUTH-TOOLING H4's targets.
Verify: your LDAP search returns your lab users/SPNs (filter explained per query).
**Lesson check:** why does one authenticated LDAP query out-value one SMB login — and which field (SPN) makes it Kerberoast-fodder?

## NE7 — spraying with discipline
Concept: credential spray = one password, many users, throttled; the sin is lockouts. Do: read the lab DC's lockout policy (`net accounts`); implement spray module: single password × user list × hosts with `--throttle` (seconds between attempts) and a hard guard that aborts on lockout threshold; run against your own lab users (one wrong-password account gets locked — that's the demo; re-enable it after).
Verify: spray run logs attempts + respects throttle + aborts correctly on lockout signal.
**Lesson check:** why is "safe spray" an engineering problem — and what does a lockout do to an entire engagement (not just one account)?

## NE8 — the module system: plugins over protocols
Concept: netexec's power is modularity — per-protocol plugins with shared result printing. Do: design your plugin interface (name, args, protocol, run(host, creds) → rows); implement 3 modules of your own (info-gather, share-lister, user-enum) runnable under smb AND ldap protocols; plugin loader reads a modules/ dir; CLI `-M module --module-args`.
Verify: `-M user-enum --protocol smb` and `--protocol ldap` both run via the same module contract.
**Lesson check:** why did netexec win by modularizing — and what does a plugin contract force you to standardize (data shape, failure, output)?

## NE9 — the product finish: output and targets
Concept: tooling is measured in failure clarity and output shape. Do: target parser (single host, CIDR, file, `-exclude`), JSON + table output, exit-code discipline (0 ok / 1 no-auth / 2 infra), `-v` verbosity ladder; write `README.md` for your tool with usage examples.
Verify: `--json` output parses as valid JSON for your NE2/NE5 runs; CIDR target set works; exit codes correct.
**Lesson check:** pick the three output decisions that make or break an operator's night — and why JSON over "pretty" tables for automation.

## NE10 — blue side: catch your own tool
Concept: netexec-class traffic has a shape: auth attempts, SAMR/LDAP bursts, exec transports, spray rhythms. Do: from EDR-COURSE: enable Sysmon on the lab, write Sigma rules for: NXC-style SMB auth burst, SAMR user-lookup cluster, WinRM exec (event 91/7600 class), spray cadence; run your tool, tune until fired; silence check on clean boot.
Verify: your rules fire on your NE2/NE5/NE7 runs (before/after table).
**Lesson check:** which of your modules is loudest — and which signal survives if an attacker slows the spray to real-human pace?

## NE11 — the ops ethics appendix: scope, authorization, and the refusal to be careless
Concept: tools are used inside authorization; the serious ones encode it. Do: write the ops-playbook section: scope file format, target validation against scope, `--no-lockout` default, logging that survives review, the "stop condition" rules; bake the scope check INTO your CLI (refuse unlisted hosts).
Verify: unlisted host refused; scope file parsed; playbook section written in your capstone.
**Lesson check:** what three guardrails does every real engagement tool need — and why is "refuse by default" the professional default?

## NE12 — CAPSTONE: your netexec-class tool, cold
Prereq: NE0–NE11. **Close all notes.** Assemble/re-create "lab-nxc": cred parser → SMB tri-state → enum (shares/users/sessions/LDAP-SPN) → exec dispatcher (psexec+winrm) → spray with guards → 3 modules → JSON output → scope refusal — then run the full sweep against your lab DC + two hosts from a clean VM, cold. Write `labs/netexec-capstone.md` like OSS: architecture, usage, module docs, detection-notes, ethics. Re-run the sweep once, cold, with notes closed.
**Pass = sweep succeeds without notes; README + detection-notes read like a maintainer's.**

---

## Rules
1. Verification + lesson check both true before next unit; quizzes in your own words.
2. Copying allowed only in NE0 boilerplate (CLI scaffolding) and NE8's plugin-loader skeleton — protocols and engines written by you; erase-and-retry once when stuck.
3. 2h/unit; stuck past that = previous unit's verification again.
4. Lab-only; invented credentials; scope-guarded; lockout-aware; Defender ON; snapshots before NE7; never a real host/account.
5. Honest bar: netexec is years of team work; this course's bar = your own tool that authenticates, enumerates, executes, sprays safely, modules cleanly, and detects itself — the floor for post-exploitation engineering and AD-automation work, proven cold at the capstone.

## Where this lives
The fourth pillar on the Windows lab: `WINDOWS-SECURITY-COURSE` (use), `AUTH-TOOLING-COURSE` (wires), `POTATO-COURSE` (privesc), this course (fleet-automation) — plus `EDR-COURSE` (the blue half) — one lab, four tools, both sides of the table.

Network-tooling sibling from the interception side: sniffer + TLS-MITM proxy like mitmproxy/Proxyman — [`PROXY-MITM-COURSE.md`](PROXY-MITM-COURSE.md) PM0–PM12 (your traffic only).