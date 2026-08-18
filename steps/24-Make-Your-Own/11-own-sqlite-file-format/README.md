# 24-11 · Own SQLite file format — real .sqlite files, forensics payoff (stretch)

**Week:** W46+ opt (stretch) · **Track:** P · **Prev:** [`../10-own-riscv-microkernel`](../10-own-riscv-microkernel/README.md)

## Objective
24-02 built a storage engine from scratch. This builds the *actual* SQLite file format — page-1 header, B-tree leaf/interior pages, freelist, varint/serial-type record encoding — so your engine reads and writes real `.sqlite` files that `sqlite3` opens, and vice versa. The correctness oracle is the real tool; the payoff is forensics: browser history, chat DBs, mobile app databases are all SQLite.

## Tasks
- [ ] Format: page-1 header (page size, freelist, schema cookie), B-tree pages (leaf/interior, cell layout), varint + serial-type record encoding; parse a file `sqlite3` created
- [ ] Reader: walk a table's B-tree, decode rows into your 24-02 engine's row model; then write — insert creates/updates pages a real sqlite3 accepts
- [ ] Interop oracle: round-trip a table your engine wrote → open with `sqlite3` CLI → query → diff vs your own output (byte-level where possible)
- [ ] Forensics payoff: parse real artifacts — Chrome/Chromium History DB (`urls` table), an Android app DB or messenger store — with your own reader; extract visit history + timestamps
- [ ] Writeup: page-format RE notes — how the format reads as a file-format RE exercise (pairs 12-01 PE, 13 USB descriptors, 21-04 disk artifacts) — `notes/`

## Resources
- SQLite file format spec (the manual); cstack db_tutorial (24-02 carry-over); `sqlite3` CLI as peer; your 24-02 engine

## Exit Criteria
- [ ] Engine reads + writes .sqlite files interoperable with real sqlite3 — `labs/`
- [ ] Own reader extracts history from a real Chrome DB — `labs/`

## Links
- [SQLite file format](https://www.sqlite.org/fileformat2.html)
- [cstack db_tutorial](https://cstack.github.io/db_tutorial/)
