# 24-34 · Own mail server — SMTP + DKIM/SPF/DMARC, spoofing lab (stretch)

**Week:** W46+ opt (stretch) · **Track:** P · **Prev:** [`../33-own-apfs-parser`](../33-own-apfs-parser/README.md)

## Objective
Email is the phishing delivery system — and the authentication stack (SPF/DKIM/DMARC) is the defense. Build a mail server: SMTP (RFC 5321) envelope + DATA, a mailbox store, then DKIM signing and SPF/DMARC verification. The security lab: spoofing — your own server receives a forged-from message, your SPF/DKIM/DMARC checks reject it (own domain in lab config only, local delivery, no public sending).

## Tasks
- [ ] SMTP: HELO/EHLO, MAIL/RCPT, DATA, dot-stuffing, response codes; a real MUA (Thunderbird/mail CLI) talks to it locally
- [ ] Store: mailbox format (mbox/Maildir), delivery + retrieval (IMAP-lite optional)
- [ ] Auth stack: SPF (TXT record check — pairs 24-19 DNS), DKIM (sign outgoing with your RSA key from 20-02; verify incoming), DMARC policy evaluation
- [ ] Spoofing lab: craft a forged-from message (your own 24-20 sniffer/crafter helps) → passes SMTP but fails DKIM/SPF/DMARC → quarantined; the "why email spoofing still works" writeup (pairs 21-02 hunting, phishing lens) — `labs/`
- [ ] Self-check: DKIM signature you sign verifies; forged mail rejected; DMARC report generated

## Resources
- RFC 5321/6376/7208/7489 (the manuals); OpenSPF docs; your 24-19 + 20-02 notes

## Exit Criteria
- [ ] Local SMTP + DKIM-signed mail, SPF/DMARC verified — `labs/`
- [ ] Spoofing lab: forged → rejected, writeup — `labs/` + `notes/`

## Links
- [RFC 5321 (SMTP)](https://www.rfc-editor.org/rfc/rfc5321)
- [RFC 6376 (DKIM)](https://www.rfc-editor.org/rfc/rfc6376)
