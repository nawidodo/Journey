# 24-17 · Own HTTP/1.1 server — parsing is the attack surface (stretch)

**Week:** W46+ opt (stretch) · **Track:** P · **Prev:** [`../16-own-memory-allocator`](../16-own-memory-allocator/README.md)

## Objective
Write an HTTP/1.1 server: TCP accept loop, request parser, routing, static files, keep-alive. Then attack your own parser — the lesson that HTTP parsing bugs (request smuggling, header confusion, CRLF injection) are where web vulns start. Pairs the 08 browser track's server-facing thinking and 21-07 web-log hunting.

## Tasks
- [ ] Server: socket accept, request line + header parse, method/path/version validation, response encoding (Content-Length vs chunked), keep-alive
- [ ] Static files + a small route app (the shape of every web framework)
- [ ] Attack lab: your own parser under — CRLF injection in path/header, header confusion (Host: dup), request smuggling (CL.TE / TE.CL shapes), percent-encoding edge cases; each worked + fixed — `labs/`
- [ ] Defense: strict token validation, canonicalization, request-size limits; re-run attacks, blocked
- [ ] Writeup: the parsing-bug taxonomy you found (pairs 08-xx browser) — `notes/`

## Resources
- RFC 7230–31 (the manual); nginx source (peer); the request-smuggling research (PortSwigger)

## Exit Criteria
- [ ] Server serves files + routes, keep-alive correct — `labs/`
- [ ] Smuggling/CRLF/header-confusion attacks: worked → fixed, writeup — `labs/` + `notes/`

## Links
- [RFC 7230](https://www.rfc-editor.org/rfc/rfc7230)
- [PortSwigger smuggling research](https://portswigger.net/research/http-desync-attacks-request-smuggling-reborn)
