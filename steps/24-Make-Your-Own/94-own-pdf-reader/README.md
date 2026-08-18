# 24-94 · Own PDF reader-lite — xref, streams, the #1 maldoc format (stretch)

**Week:** W46+ opt (stretch) · **Track:** P · **Prev:** [`../93-own-sokoban-solver`](../93-own-sokoban-solver/README.md) · **Next:** [`../95-own-imap-client`](../95-own-imap-client/README.md) · **Pairs:** 24-27, 24-35, 24-25, 12-07/17-03

## Objective
PDF is the malware-delivery king (12-07 malspam, 17-03 mobile) because parsing it is brutal: build a reader-lite — object model (`obj R` indirect refs, the xref table + trailer), content streams (FlateDecode via your 24-25 inflate), page tree + text operators (`Tj/TJ`, font/encoding maps — pairs 24-49), and render text to your 24-73/SVG or 24-14 canvas. Then the security lab that explains the whole threat class: a malformed-object bomb (infinite `R` loop, huge-length stream, bad xref) against your parser — you handle it without crashing (pairs 24-27 robustness), and the classic «quit-then-open» JS/annotation hook points cataloged.

## Tasks
- [ ] Parser: tokenizer + object grammar, indirect refs, xref/trailer, incremental-update sections (the format RE — 24-27 discipline)
- [ ] Streams: FlateDecode (24-25), ASCIIHex, the filter chain; content = page operators
- [ ] Text: Tj/TJ string ops, font + encoding (24-49 cmap discipline), extract lines; render as SVG (24-73)
- [ ] Robustness lab: fuzz-lite (mutate your own PDFs — 05-12 thinking): infinite-ref loop, length bombs, malformed xref → clean failure, no crash, no hang — `labs/`
- [ ] Writeup: why PDFs are the top phish payload (inconsistency between viewers — the parser-divergence gap, pairs 08/24-47), the hook-point catalog — `notes/`

## Resources
- PDF 32000 spec (the manual — ch. 7 the parser, ch. 9 text); qpdf source (peer, a correctness oracle); your 24-27/24-25/24-73 code

## Exit Criteria
- [ ] Reader extracts text from your generated + a real PDF; renders via 24-73 — `labs/` + `code/`
- [ ] Robustness runs clean + maldoc writeup — `labs/` + `notes/`

## Links
- [PDF 32000-1:2008](https://opensource.adobe.com/dc-acrobat-sdk-docs/pdfstandards/PDF32000_2008.pdf)
- [qpdf](https://qpdf.sourceforge.io/)