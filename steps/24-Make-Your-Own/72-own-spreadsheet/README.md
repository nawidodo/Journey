# 24-72 · Own spreadsheet — formula engine, dependency graph, recalc (stretch)

**Week:** W46+ opt (stretch) · **Track:** P · **Prev:** [`../71-own-encrypted-overlay-fs`](../71-own-encrypted-overlay-fs/README.md) · **Next:** [`../73-own-svg-renderer`](../73-own-svg-renderer/README.md) · **Pairs:** 24-09, 24-42, 24-38

## Objective
The most-used programming language has no compiler: spreadsheets. Build one: grid model, cell formula parser (your 24-09 parser discipline — references, ranges, functions), the **dependency graph** with topological recalc (the DAG core — pairs 24-41 index thinking, 24-48 orchestration), cycle detection (the error users fear), and CSV/TSV import-export (interop with your 24-60 corpus side). The payoff: you understand Excel's recalc engine, circular-reference handling, and why "spreadsheet risk" is a research field (pairs 21-02 fraud detection).

## Tasks
- [ ] Grid + cells: values/types, addressing (A1/B2), range syntax; CSV parse/serialize (pairs 24-25 parsing)
- [ ] Parser: formula tokenizer + AST (24-09 skills), functions (SUM/IF/VLOOKUP-lite), precedence
- [ ] Recalc: dependency graph, topological order, dirty-tracking (only recompute changed cells), cycle detection + error cell
- [ ] Engine lab: 1000-cell model — recalc after single edit touches only dirty cells (the measured win, pairs 24-30); a circular ref → clean error — `labs/`
- [ ] Self-check: your formulas match a real spreadsheet's results on the same model (the oracle)

## Resources
- Excel recalc internals posts (the manual); your 24-09/24-42 code

## Exit Criteria
- [ ] Formula model recalculates correctly + efficiently (dirty-only) — `labs/` + `code/`
- [ ] Cycle/dirty-tracking notes — `notes/`

## Links
- [Excel recalculation (Microsoft)](https://learn.microsoft.com/en-us/office/troubleshoot/excel/recalculate-formulas)
- [CSV spec notes (RFC 4180)](https://www.rfc-editor.org/rfc/rfc4180)