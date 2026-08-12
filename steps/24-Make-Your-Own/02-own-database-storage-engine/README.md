# 24-02 · Own Database / Storage Engine

**Week:** W46+ (parallel, optional, stretch) · **Track:** P · **Prev:** [`../01-own-os-kernel`](../01-own-os-kernel/README.md) · **Next:** [`../03-own-git`](../03-own-git/README.md)

## Objective
Write a tiny storage engine: parse SQL-ish commands, a binary table file, B-tree index, one crash-safe write path. Trains the systems discipline (file formats, serialization, page/paging, invariants) that the security tracks lean on for file-format RE, DFIR artifact parsing, and driver work.

## Tasks
- [ ] REPL + parser + a query path over an in-memory table — `code/`
- [ ] Persist to a binary file; scan + insert — `code/`
- [ ] B-tree index (or skip-list) for keyed lookup — `code/`
- [ ] Crash-safety: append-only log + replay, or WAL — `code/`
- [ ] Debrief: how your format maps to a "document forgery" attack surface (Phase 8/13/21 RE angle) — `notes/`

## Resources
- *Database Internals* (Petrov) ch.1–4; "Write your own mini SQLite" (cstack)
- build-your-own-x: own database catalog

## Exit Criteria
- [ ] Binary-persisted DB with an index + crash-safe write — `code/`
- [ ] RE/PSF debrief note — `notes/`

## Links
- [Let's Build a Simple Database](https://cstack.github.io/db_tutorial/) (SQLite-clone walkthrough)
- [build-your-own-x: own database](https://github.com/codecrafters-io/build-your-own-x?tab=readme-ov-file#build-your-own-database)