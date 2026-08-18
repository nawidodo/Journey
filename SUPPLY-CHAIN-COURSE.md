# Supply-Chain Course — Absolute-Beginner (hello world → YOUR OWN package manager + registry + supply-chain security, apt/brew/choco-class, gated)

Zero packaging knowledge assumed — you bring hashing/signing from 20-crypto steps, CVE-vision from CVE-STUDY, from-source build skills from the compiler courses, and lab discipline. The course builds `spkg` (your name): your own package format + builder, a repository server, a dependency resolver, signed verification, transactional install/update/remove — the whole apt/brew/Chocolatey shape — and then the supply-chain-security layer that modern forges live and die by: tamper rejection, SBOM, vulnerability audit. Steps 24-39 (own-package-manager), 24-75 (own-package-registry), 24-91 (supply-chain-signer) are your charted coastline — this course sails the whole bay at once. Each unit: concept → do → runnable verification → **lesson check (own words, `notes/scN-quiz.md`)** — no advance without both. ~3-4h/unit, 12 units + capstone ≈ 8-10 weeks.

Compass (re-read when lost): a package manager = four machines in a trench coat: a FORMAT (what a package IS), a REPOSITORY (where packages live + how they're found), a RESOLVER (which versions satisfy whom), and a VERIFIER (proving what you install is what was published). Layer the supply-chain truth on top: every install is a trust decision about strangers' code, and modern incidents (typosquatting, dependency-confusion, malicious updates) show the trust failing in boring places. Your course: build the four machines, then arm the trust: hashes+signatures, pinning, SBOM, auditing — so your own repo is the one you'd actually trust.

Supply-chain ethics (unit SC0): publishing ≠ attacking. This course PUBLISHES YOUR OWN packages to YOUR OWN registry only; the attack-classes (typosquatting etc.) are studied as READING and DEFENSE (what to verify, what to pin) — never practiced against third parties; mirroring/embedding tests run against your own repo; nothing ships to a public index.

---

## SC0 — hello world: the four machines
Concept: format, repository, resolver, verifier; the ecosystem map (deb/dpkg+apt, brew bottles, choco, npm-class). Do: the four-machine diagram + the ecosystem comparison table (each real manager: its format/repo/resolver/verify — reading their docs); hello: hand-build one "package" (a folder of YOUR files + a hand-written manifest) and "install" it to a fake root by copying — the seed of everything.
Verify: diagram + table written; hand-installed package works from the fake root.
**Lesson check:** which of the four machines is the ONE that makes a package manager (not just a downloader) — and what does each real manager's VERIFIER do differently (what does that tell you about trust models)?

## SC1 — the format: packages with a spine
Concept: your package = archive + manifest: files (relative paths, modes), metadata (name, version, deps, scripts), integrity (per-file hashes). Do: your format spec (layout, magic, fields, endianness); builder tool (`spkg build <dir>` → .spkg with the manifest + hashes + optional signature slot); validator (rejects malformed); the round-trip: build → inspect → install to fake root (from built archive, not hand-copy).
Verify: build/inspect/install round-trip clean; validator rejects a corrupted package.
**Lesson check:** what MUST a manifest contain to make installs transactional (files list? scripts? exact hashes? origins?) — and why does the FORMAT constrain everything after it (what's hard to retrofit)?

## SC2 — the repository: a server with an index
Concept: repository = index + files: versioned package metadata, publish flow, client discovery. Do: your repo server (static HTTP/file layout: `index.json` + per-package metadata + archives), `spkg publish` (upload + index update), `spkg search/list` (client reads index); the index format versioning (schema version; old clients vs new — compat rule); publish/remove flow with history (package versions stay, removal = deprecation flag — the real-world lesson).
Verify: publish → search → install-from-repo works; deprecation flag behavior correct.
**Lesson check:** why does a repo KEEP old versions (what breaks when packages vanish — the left-pad lesson) — and what does INDEX VERSIONING protect (who breaks without it)?

## SC3 — the resolver: mathematics with a lockfile
Concept: deps are a graph: version constraints (semver-lite/ranges), solver (pick versions satisfying everyone), conflicts, lockfile (pinned exact set). Do: your constraint syntax (exact/range/pessimistic) + parser; the solver (topological order + backtracking-lite on your graph; cycle detection), lockfile generation (pinned immutably), test graphs: valid diamond dep, conflict (unsolvable → report), cycle (rejected); benchmarks: your solver's scaling on a 100-node graph (honest).
Verify: solver resolves the three test graphs correctly; lockfile reproduces byte-identically (re-run same graph).
**Lesson check:** why is the resolver NP-ish in general (what makes it hard — what do real solvers trade) — and what does a LOCKFILE buy that a fresh solve can't (who keeps resolving nightly and what breaks)?

## SC4 — verification: trust, mechanically
Concept: hashes + signatures: per-package signature (your key), signed repo metadata (index signed → tampered index rejected), replay protection (fresh timestamps). Do: signing pipeline (your keys from crypto course), `spkg verify` (signature + per-file hashes + index signature), the tamper battery: flip package byte, swap index, replay old index — all rejected with evidence; the key-distribution problem (how does a client TRUST your key — the bootstrapping essay: fingerprinted keys, pinning, web-of-trust-lite reading).
Verify: battery green (all tamper classes rejected); bootstrapping essay written.
**Lesson check:** what does signing the INDEX add over signing packages (who else could you defend against) — and why is KEY BOOTSTRAPPING the eternal catch-22 (whom do you trust to give you the key)?

## SC5 — the engine: transactional install
Concept: install must be COMMIT-able: stage → apply → commit with rollback (a failed install leaves the old state). Do: your transaction engine: stage (files to temp), apply (manifest-driven), rollback path (undo on any failure — simulate a failure: corrupt mid-install → rolled back clean), upgrade/downgrade (version transitions with scripts), the "scripts are dangerous" truth (install scripts run with power — pin, document, sandbox-lite reading). 
Verify: install/upgrade/downgrade paths pass; injected failure rolls back with the tree untouched.
**Lesson check:** what does ROLLBACK require of the FORMAT (what must be recorded during stage) — and why are install scripts the supply-chain hot spot (what do they run as)?

## SC6 — from source: building packages honestly
Concept: packaging source: build your own project (from the compiler courses) into spkg: compile → staging dir → manifest capture → archive; reproducibility attempt (build-id / SDIST: same source → same bytes? measure — honest). Do: `spkg build-src` pipeline for a small C project of yours, build-verification (the package's hashes match the built tree), the reproducibility report (what varied, what didn't — honest); the "trusting build machines" note (supply chain's real junction — who compiles, and is the build environment itself trusted?).
Verify: source-built package installs + runs; reproducibility report honest.
**Lesson check:** why is reproducible building the GOLD standard (what attack does it kill) and why is it HARD — and what does the build machine question say about where supply-chain security actually lives?

## SC7 — cache, offline, mirror: the resilient edge
Concept: clients cache: offline installs, partial-cache corruption recovery, mirrors (alternate repos). Do: your cache layer (store + hash-check + reuse), offline install demo (from cache only), cache-corruption handling (detected → refetch), mirror config (two repos, failover); the cache poisoning note (cache must verify signatures TOO — replay the tamper battery through the cache).
Verify: offline install works from cache; corrupt-cache entry self-heals; mirror failover works.
**Lesson check:** what does a CACHE change about verification (what must the cache NOT cache: a poisoned index?) — and why do mirrors matter (what breaks in a repo outage — the update-model lesson)?

## SC8 — supply-chain attacks: the catalog of trust failures
Concept: the classes: typosquatting, dependency confusion, malicious update, shadowing — how each works, who it hits, the defenses. Do: the attack-catalog table (class × mechanism × real incident (reading) × YOUR defense); the defense stack you build: pinning, hash-pinning, provenance metadata (who published, when, from where — your repo records it), review-before-update workflow; the 300-word essay: "my package manager trusts by default; here's the minimum I would change in apt/brew" (argued).
Verify: catalog table complete; defense stack demoed (a planted "malicious update" in YOUR repo rejected by policy); essay written.
**Lesson check:** which attack class is cheapest to execute (and thus most common) — and why does PINNING beat speed (what does "latest" really commit you to)?

## SC9 — SBOM and audit: knowing your own supply chain
Concept: your packages have deps which have deps: SBOM (software bill of materials — the full ingredient list), vulnerability scan (match versions against CVE data — CVE-STUDY skill), remediation (upgrade path + advisory). Do: SBOM generator (walk install manifest + dep graph → your SBOM format: components, versions, licenses-lite), vulnerability checker (feed your known-CVE table (from CVE-STUDY KV) → report affected components), remediation flow (upgrade → resolve → re-verify); the demo: a package with a planted "vulnerable" dep version → audit flags it → upgrade path shown.
Verify: SBOM generated for your package graph; audit flags the planted vuln; remediation completes.
**Lesson check:** what can an SBOM show that a scanner can't (coverage: what you DIDN'T know you had) — and why is licensing-lite still supply-chain risk (what does a compliance failure cost)?

## SC10 — the real ecosystems, read: apt, brew, choco, OCI
Concept: the industry's answers: deb/dpkg internals, Homebrew bottles, Chocolatey, and the container world (OCI images as packages; Sigstore/cosign — modern signing). Do: reading unit: pick TWO real formats and dissect (structure, scripts, verification model) mapped to YOUR four machines; the container-as-package reading (OCI layer model vs your archive, registry auth); the modern-signing reading (Sigstore: keyless, ephemeral, transparency logs — why it exists, what it fixes vs your static keys).
Verify: dissection notes + comparison table (real vs yours); Sigstore essay (what you'd adopt, what you'd question).
**Lesson check:** what did the industry converge on that you built differently (and why does it keep converging) — and what does SIGSTORE solve that static keys can't (the perpetual key problem)?

## SC11 — the product: spkg
Concept: usable package manager + registry: build/publish/search/install/update/remove/verify/audit from ONE CLI; repo ops tooling. Do: `spkg` full CLI (commands above + help + exit codes + JSON output), `spkg-repo` admin (publish, deprecate, index-sign, keys), end-to-end demo: a dependency pair (two packages, one depends on the other) built, published, installed on a FRESH sandbox, updated, verified, audited; the README-as-contract (your usage doc).
Verify: end-to-end demo runs clean from fresh sandbox; CLI UX pass (your own usage notes).
**Lesson check:** what did the FULL loop teach you that the units hid (integration pain: what broke only when joined) — and why is a package manager measured by ITS CLI (what makes it trustworthy-feeling)?

## SC12 — CAPSTONE: the supply chain, cold
Prereq: SC0–SC11. **Close all notes.** Cold: rebuild format+builder, resolver, verify, transactional install in one sitting (no notes), then: publish YOUR project (from the compiler courses) to your repo, install on a fresh sandbox, resolve a real 3-dep graph you invent, tamper-attack your own repo (flip a byte at the mirror — rejected), audit the graph. Write `labs/supply-chain-capstone.md`: architecture (four machines + trust stack diagram), format spec summary, resolver notes, tamper/audit reports, the SC8 essay, three proud decisions, regret, roadmap (keyless signing, SBOM automation, mirror federation).
**Pass = the cold rebuild runs the full publish→resolve→verify→install→audit loop with tamper rejected and the essay intact.**

---

## Rules
1. Verification + lesson check both true before next unit; quizzes in your own words.
2. Copying allowed only in SC0/SC1 boilerplate (archive scaffold, manifest skeleton) — builder, resolver, verifier, engine, audit, CLI written by you; erase-and-retry once when stuck.
3. 3-4h/unit; stuck past that = previous unit's verification again.
4. **Supply-chain ethics: publishes to YOUR OWN registry; attack classes = reading + defense only, never against third parties; nothing ships to a public index.**
5. Honest bar: apt/Homebrew/Sigstore are decade-scale ecosystems with security teams; this course's bar = a working four-machine package ecosystem with real verification, transactional safety, and audit, proven cold at the capstone — the floor for supply-chain engineering and the complete answer to "should I install this".

## Where this lives
Charts the 24-39/75/91 coastline (package manager, registry, signer steps); reuses 20-crypto keys/hashes, CVE-STUDY's tables for audit, compiler-course builds for source packaging, and INTEGRITY's chain discipline — one ecosystem, your keys, your rules, your trust.