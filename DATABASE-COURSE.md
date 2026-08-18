# Database Course — Absolute-Beginner (hello world → your OWN SQL database in SQLite's shape, gated)

Zero database knowledge assumed. You need: a Mac/Linux box, C (or C++), and SQLite installed for the "meet the real one" reference (`brew install sqlite3`). The repo's storage-engine steps (24-02, 24-26 LSM-tree, 24-51 time-series) are your natural next/final sprints. Each unit: concept → do → runnable verification → **lesson check (own words, `notes/qN-quiz.md`)** — no advance without both. ~3h/unit, 12 units + capstone ≈ 9–11 weeks. This is the database that powers phones, browsers, and millions of apps — you'll rebuild its skeleton: file pages, a B-tree, a SQL parser, a query engine, transactions — and then talk to your own product like an engineer.

Compass (re-read when lost): a database is **storage + index + query engine + transactions**: files hold pages, a B-tree finds rows fast, the SQL parser turns text into a tree, the planner walks it into an executable plan (scan/filter/join), and the log makes "not losing your data" an engineering guarantee instead of a hope. SQLite's genius is that this entire stack sits in one file — you'll understand why by owning each layer.

Safety: pure engineering, own machine; crash-testing means *your own* test-VM files or throwaway directories (never your real data; the repo pattern is lab-seconds only). Rules like every course — verification + own-words quiz gate each unit; copy only Q0/Q10 boilerplate (scaffold, CLI shim); erase-and-retry once when stuck; benchmarks honest.

---

## Q0 — hello world: a database is a file that answers fast
Concept: the baseline: a record store where "find by key" doesn't scan everything. Do: your first file-backed key-value store (append records + a side index), read/scan/write metrics; install SQLite, dump one of its files as bytes, read its header (magic `SQLite format 3`), and run `EXPLAIN QUERY PLAN` on a trivial query — see the words (scan, index, search) you'll be emitting by Q4.
Verify: your KV store answers 100k-key lookups without full scans; SQLite file header identified from the spec.
**Lesson check:** what is the one thing an index buys you — and why is "just read the whole file" the enemy every database layer fights?

## Q1 — pages: storage's building blocks
Concept: files are chopped into fixed pages (SQLite: 4096B) containing records in slotted layouts; free space is tracked per page. Do: your page allocator: fixed-size pages, slot directories (offset+size per record), page free-list, extend/truncate; corruption-detection (magic per page); a db file inspector tool that walks your pages and prints a human table.
Verify: your inspector walks a db you created (records + free list correct); a corrupted page is detected, not crashed on.
**Lesson check:** what does a *slotted* page buy you over compact-pasted records — and why does a free-list matter for delete/reuse?

## Q2 — the B-tree: the whole world turns on fan-out
Concept: B+tree on pages: sorted keys, internal nodes routing, leaves holding records; split on overflow, find in O(log n) page touches. Do: your B-tree over the page allocator: search/insert/split/range scan; `PRAGMA`-style stats (nodes, depth); benchmark 1M random-key inserts then range scans vs your Q0-linear scan — the fan-out chart.
Verify: B-tree beats scan in your own honest table (notes/q2.md); range scan correct; stats printed.
**Lesson check:** why log(n) *page reads* — and what's special about leaves in a B+tree that internal nodes don't do?

## Q3 — SQL parsing: text into a tree
Concept: SQL is a language: tokenizer + recursive descent over a subset — SELECT/INSERT/CREATE/UPDATE/DELETE with WHERE. Do: tokenizer (keywords, identifiers, strings, numbers, operators, positions) + parser (select: column list, from, where, order, limit; insert; create-table with types) → AST; pretty-printer; 6 golden queries hand-verified; 3 malformed → positioned errors.
Verify: AST dumps match hand trees; your qb CLI parses and describes queries (`EXPLAIN` preview coming next unit).
**Lesson check:** why does SQL parse like any PL — and what did you have to decide (ambiguity!) about `WHERE a = b = c` being legal or not?

## Q4 — query execution: plans, not magic
Concept: planning = turning the AST into an executable pipeline: scan → filter → project → sort; index lookup where available. Do: query executor: table scan operator, filter (WHERE eval), projection (column list), ORDER BY (sort op), LIMIT; EXPLAIN output = your plan tree printed as a plan text; run queries against a table you loaded.
Verify: EXPLAIN prints a sensible plan; results match hand-evaluated truth for your query suite; plan uses the index after Q8's first index exists.
**Lesson check:** why separate "plan" from "execute" at all — and what information does a filter operator need that a scan operator doesn't?

## Q5 — transactions: the log is the law
Concept: ACI(D) is boring until the crash; a write-ahead log + commit record + replay makes it real. Do: your WAL: begin/commit protocol, log records (page image or logical op — choose + justify), checkpoint + truncate; page cache with dirty-tracking; crash-recovery: kill -9 (your test script, own test files) mid-transaction, restart, verify committed-visible / uncommitted-gone.
Verify: kill-test matrix passes (commit-vs-not × crash-points); recovery log shown in your tool.
**Lesson check:** why does the log get written BEFORE the page change — and what exactly does "commit" mean when the answer is "never lose it"?

## Q6 — concurrency: two buyers, one row
Concept: engines serialize (locks) or snapshot (MVCC); both must be *correct* — lost-update is the classic failure. Do: reader-writer lock manager (readers share, writer excludes) or MVCC snapshots (version stamps, readers see old version) — pick one, implement, document the choice; stress: two threads (or two qb processes) update the same counter row 10k times — lost-update test MUST show the count correct.
Verify: stress test lands on exactly 10k with your scheme; deadlock path tested (no hang; timeout).
**Lesson check:** what is the lost-update failure exactly — and why does "lock everything" get abandoned for MVCC in high-read systems?

## Q7 — joins and aggregates: the queries that make SQL famous
Concept: joins combine tables (nested-loop is correct, hash join is fast), aggregates fold rows (COUNT/SUM/GROUP BY). Do: nested-loop join + hash join (choose per size — explain), GROUP BY + aggregates, ORDER BY on the join result; extend EXPLAIN with join type; a 3-table join query suite with known answers.
Verify: join orders produce correct answers; EXPLAIN shows chosen join (nl/hash); 1M-row group-by timing table (notes/q7.md).
**Lesson check:** why hash join beats nested-loop on large inputs — and why does GROUP BY need a *state* (accumulator) per group?

## Q8 — indexes and constraints: the database's promises
Concept: indexes make hot queries fast; constraints (PRIMARY KEY, UNIQUE, FOREIGN KEY) make data be *right* — both are more B-tree + checks. Do: secondary index pages (key → row id), UNIQUE enforcement on insert (dup rejection), PRIMARY KEY, FK lite (INSERT checks referenced id exists, DELETE blocks); planner uses your indexes in WHERE/join; constraint violations → your error, not silent corruption.
Verify: index-driven plan (EXPLAIN shows index search); dup/FK violations rejected with clear errors; benchmark index vs scan on a hot filter.
**Lesson check:** what does an index cost you (write time, space) to buy read speed — and what's a FOREIGN KEY actually *preventing*?

## Q9 — SQLite-parity surface: the queries beginners expect
Concept: NULL, IN, CASE, LIMIT/OFFSET, string functions, date-time lite — the "real SQL feel" surface. Do: implement NULL three-valued logic (WHERE NULL = NULL is not TRUE — document), IN lists, CASE expressions, LIMIT/OFFSET, user functions (UPPER, LENGTH); a query suite ported from SQLite's own tutorial runs identically-ish on qb.
Verify: your NULL-logic tests pass (the classic gotcha documented); tutorial suite runs; explain of a CASE shows in plan.
**Lesson check:** why is NULL a *three-state* logic — and why can't `=` handle it the way you first think it should?

## Q10 — the interface: CLI + C API, SQLite-shaped
Concept: databases ship two doors: a REPL (sqlite3) and an embedding API (prepare/step/finalize). Do: your qb CLI: prompt, schema/DESCRIBE, EXPLAIN, backslash commands (.tables, .schema), history; your C API: `qb_open/qb_prepare/qb_step/qb_close` over a value-oriented row cursor; a HOST program (your INTERPRETER-COURSE sscript!) runs SQL through your API.
Verify: CLI session recorded in notes; API demo runs; sscript-embedded qb query returns rows into script tables.
**Lesson check:** why prepare/step/finalize (compile-once, run-many) — and what makes an embedding API "ownable" rather than "hackable"?

## Q11 — durability and self-defense: backup, integrity, vacuum
Concept: mature engines maintain themselves: backups, integrity checks (corrupt pages found, not guessed), compaction. Do: backup-to-file (consistent snapshot via your WAL checkpoint), integrity checker (walk every page/btree, verify structure + magic + free-list), VACUUM-lite (compact file, reclaim free space); a planted-corruption test your checker catches.
Verify: backup restores correctly; checker flags your planted corruption; VACUUM shrinks the file measurably.
**Lesson check:** why is "integrity" a property you *prove by walking* — and what does VACUUM trade (time) for (space)?

## Q12 — CAPSTONE: qb ships, cold
Prereq: Q0–Q11. **Close all notes.** Re-create the B-tree + query core cold (the gauntlet discipline), then finish qb v1: pages+WAL+B-tree+SQL subset+planner+joins+aggregates+constraints+CLI+C API+integrity; load a 1M-row dataset; run your query suite + EXPLAIN + two crash-recovery tests; write `labs/db-capstone.md` like a DB engineer's post: page-format diagram, WAL design + crash matrix, three proud decisions, regret, roadmap (MVCC? vector search? SQLite-on-disk compatibility?).
**Pass = qb answers the suite from cold, crash tests pass, writeup reads like an engineer's.**

---

## Rules
1. Verification + lesson check both true before next unit; quizzes in your own words.
2. Copying allowed only in Q0/Q10 boilerplate (allocator scaffold, CLI shim) — pages, B-tree, parser, planner, WAL written by you; erase-and-retry once when stuck.
3. 3h/unit; stuck past that = previous unit's verification again.
4. Crash-tests use throwaway test-files/VM only; never your real data; benchmarks honest.
5. Honest bar: SQLite is 20+ years of a small team; this course's bar = your own database answering real queries with indexed speed, transactional correctness (kill-tested), and a C API another program can use, proven cold at the capstone — the floor for storage engineering and the exact mental model for databases, key-value stores, and every "why is the DB slow" mystery you'll ever face.

## Where this lives
`steps/` unchanged (route: 24-02, 24-26, 24-51). Pairs the INTERPRETER course (embed qb into sscript — two tools you wrote, talking to each other) and the NETEXEC/AUTH-Tooling stack later (every AD/credential tool reads a database: yours included).