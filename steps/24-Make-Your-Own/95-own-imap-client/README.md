# 24-95 · Own IMAP client — fetch/flag/move, the mail client your 24-34 earns (stretch)

**Week:** W46+ opt (stretch) · **Track:** P · **Prev:** [`../94-own-pdf-reader`](../94-own-pdf-reader/README.md) · **Next:** [`../96-own-dnssec-lite`](../96-own-dnssec-lite/README.md) · **Pairs:** 24-34, 24-12, 24-61, 20-11

## Objective
You built a mail server (24-34 — SMTP out); now the client half: an IMAP-lite client — connection + auth (LOGIN/STARTTLS overlay on 24-12 TLS), the protocol grammar (tagged commands, untagged responses, literals — the `{n}` continuation dance), mailbox/UID/flag semantics, fetch+parse of messages (24-34 RFC822), and delta-sync (UIDVALIDITY + UID range — pairs 24-61 sync thinking) into a local mailbox. Ends as a real CLI: list inbox, read, mark read, move — against your own 24-34 server (the two-tool integration, oracle = mutt/thunderbird interop).

## Tasks
- [ ] Protocol: tagged/untagged lines, literals, capabilities (the parser — 24-06 discipline), STARTTLS (24-12)
- [ ] Session: LOGIN, SELECT, mailbox list, SEARCH/UID FETCH, STORE flags, MOVE/EXPUNGE semantics
- [ ] Parse: 24-34 RFC822 parser reuse; MIME-lite (multipart, encodings — 24-45 format muscle); search headers
- [ ] Sync: UIDVALIDITY/UID delta, local cache (24-42 KV), the offline-read model (24-61 the same problem)
- [ ] Interop lab: your client ↔ your 24-34 server (full loop send+fetch), then ↔ a public-test IMAP server (oracle) — `labs/`
- [ ] Writeup: mail as attack surface (credentials at rest, STARTTLS stripping — 20-13 thinking), IMAP payload parsing (12-07) — `notes/`

## Resources
- RFC 9051 IMAP4rev2 (the manual); mutt/wire source (peer); your 24-34/24-12/24-61 code

## Exit Criteria
- [ ] CLI reads/syncs/moves mail against own server + interop oracle — `labs/` + `code/`
- [ ] Mail-security writeup — `notes/`

## Links
- [RFC 9051](https://www.rfc-editor.org/rfc/rfc9051)
- [IMAP (Wikipedia)](https://en.wikipedia.org/wiki/Internet_Message_Access_Protocol)