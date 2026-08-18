# Proxy & MITM Course — Absolute-Beginner (hello world → your OWN sniffer + HTTPS inspection proxy like Proxyman, gated)

Zero proxy knowledge assumed — you need Python (3.x stdlib-first), your Mac, a willingness to build the same idea twice: a **sniffer** (observe passing traffic, no interception) and a **MITM proxy** (intercept, decrypt, modify yours). Lab is 100% local and 100% yours: a tiny local server you write, `curl`/your own test client, loopback or your own lab VM; the CA you mint is installed ONLY into your own test tools. Each unit: concept → do → runnable verification → **lesson check (own words, `notes/pmN-quiz.md`)** — no advance without both. ~2-3h/unit, 12 units + capstone ≈ 6-8 weeks.

Compass (re-read when lost): sniffing = **copy + parse**: a socket in promiscuous/BPF mode sees passing packets; you parse and reassemble. MITM = **intercept + relay + decrypt**: traffic is redirected to run *through you* (proxy), TLS is terminated by YOU with a certificate your own tool trusts, so you read, modify, and re-encrypt. Proxyman/Charles/mitmproxy are the commercial/OSS versions of exactly this. The power is disciplined: you can re-write any request your own app makes, throttle, map a URL to a local file — and the boundary line is written by you in PM11.

Safety & ethics hardline: **your devices, your servers, your traffic only; CA installed only into your own tools; no interception of traffic you don't own; no third-party sessions, no "debugging someone else's app"** — the tooling legitimately used by devs, QA, and authorized pentests is the same tooling that breaks privacy when misdirected. The essay in PM11 is the course's heart, not an afterthought.

---

## PM0 — hello world: two tools, one map
Concept: sniffer = pass-by capture; proxy = flow-through intercept; both = parse+decode. Do: write your local lab server (Python http.server subclass) + a test client (curl to it); draw the two topologies (pass-by vs flow-through) with arrows for where YOUR code sits; read about BPF and about CONNECT (two paragraphs each, in your words).
Verify: topology map + your two paragraphs written; lab server/client run.
**Lesson check:** what's the fundamental difference in placement (who must cooperate for MITM to work) — and why does sniffing need no cooperation at all?

## PM1 — the capture loop: packets by hand
Concept: capture = ask the OS for copies (BPF/libpcap), parse headers (ethernet/IP/TCP as your ND/US skills), reassemble streams by sequence numbers. Do: your sniffer using the stdlib where possible + a BPF via `tcpdump`-style external capture OR raw sockets on the lab interface; parse ether/IP/TCP headers with your own code; a TCP reassembler (seq/ack → ordered byte stream) for one flow; verify against tcpdump byte-for-byte on your loopback capture.
Verify: your parsed headers + reassembled flow match tcpdump exactly for the lab session.
**Lesson check:** what does BPF give the kernel permission to do — and why does packet capture see bytes but not "requests" (what's missing)?

## PM2 — flows: from bytes to conversations
Concept: connection table: 5-tuple → state → buffered stream; correlate request/response into transactions; filter engine (`-f host/port/proto`) your own. Do: flow table in your sniffer (new/open/close states, byte counters), HTTP transaction extraction (first-line + headers + body split), your filter language (parse "host X and port 8080" style tokens); terminal table output.
Verify: your sniffer reports the lab session as transactions (method, path, status) matching the server log.
**Lesson check:** why track 5-tuples and not just packets — and what is a "transaction" that a flow is not?

## PM3 — the forward proxy: traffic flows through you
Concept: proxy = accept → parse request → forward → relay back; CONNECT = tunnel mode (client says "make a raw tunnel to X"). Do: your HTTP forward proxy: handles GET/POST, forwards, relays response with byte counts; handles CONNECT (no TLS yet — raw tunnel); point curl at it (`-x`), verify the proxy log sees the request + the tunnel bytes; test both paths.
Verify: curl through your proxy works; CONNECT tunnel bytes relayed and counted.
**Lesson check:** what does CONNECT actually promise the client — and why does HTTPS make plain HTTP-proxying blind (what can't you see)?

## PM4 — TLS MITM: your own CA ends the blindness
Concept: MITM = terminate TLS: your CA signs a cert for each host; the client (your tool, trusting your CA) validates against YOUR chain; you decrypt, read, re-encrypt. Do: generate your CA (openssl CLI, documented — your own key management), issue per-host certs (SNI-based), wrap both legs with Python `ssl` (server-side cert from your CA, client-side normal validation with allowlist); lab test: curl `-x yourproxy --cacert yourCA` to your HTTPS lab server → proxy logs the DECRYPTED body; SSLKEYLOGFILE to Wireshark proven too.
Verify: proxy shows plaintext HTTP inside TLS for your lab flow; cert chain verified (your CA → host cert).
**Lesson check:** what does trusting-the-CA grant the proxy — and where does MITM break if the client refuses to trust you (next unit)?

## PM5 — decode, edit, replay: you can change the world (yours)
Concept: the product value: headers/JSON decode, request rewrite (change header/body), replay. Do: HTTP/1.1 + JSON body decoding in your tool; rewrite rules (e.g., replace `X-Test` header, swap a JSON field); replay (resend an edited request); a tiny UI-lite (terminal + color or a local HTML viewer); demo: your lab server logs the MODIFIED request from your rewrite.
Verify: rewrite visible server-side; replay logged as a second request; JSON decode shown.
**Lesson check:** what does a rewrite rule need to touch (request only? response too?) — and why is replay harder than it sounds (ids, cookies, sequencing)?

## PM6 — WebSocket and streaming: the modern web
Concept: WS upgrade + frame parsing; chunked/SSE streaming — proxies must stream, not buffer, or the web hangs. Do: WS frame parser (opcode, mask, length encoding — RFC 6455) + your proxy speaks WS with a lab WS server (websockets via your own frames); chunked transfer handling (dechunk/rechunk transparently); SSE passthrough with live display.
Verify: WS messages shown frame-by-frame; big chunked body streams without full buffering (memory check).
**Lesson check:** why must a proxy stream chunked bodies — and what does WS's masking bit actually protect against (hint: it's not you)?

## PM7 — pinning: the honest wall
Concept: cert pinning exists precisely to stop you; apps pin their CA/host — MITM fails with a visible error. Do: write a pinned lab client (verifies its own expected cert); run it through your proxy: MITM FAILS (documented error, no panic) — that's the lesson working; read about pinning-bypass techniques (injection, hooking) and write the honest page: bypassing another party's pinning is device-capture/attack tooling for OTHER people's devices — outside this course by design (your own app, pinning OFF or openly managed, is the course's scope).
Verify: pinned-client MITM failure demonstrated + documented; honest page written in your words.
**Lesson check:** who is pinning protecting against — and what does the pinning-vs-proxy arms race reveal about trust on the internet?

## PM8 — the Proxyman feature shelf: map-local, inject, throttle-prep
Concept: the professional features: redirect rules, map-local (serve file instead of remote), response injection, pause/resume. Do: redirect rule (host/port rewrite), map-local (URL → local file), response-body injection (inject a script into your lab page's HTML), pause/resume on a slow lab endpoint; each with a demo + test.
Verify: each feature demonstrated against your lab server with visible effect.
**Lesson check:** what does map-local make possible in development (and demos) — and why does response injection matter for test-realism more than people expect?

## PM9 — the hotel rule: throttle and scale
Concept: proxies shape traffic (latency/bandwidth) and must survive big bodies/concurrency. Do: latency + bandwidth throttling (token bucket, per-connection), large-body streaming (10MB+ through you with bounded memory), concurrent connections (10 parallel curl) with per-flow accounting; benchmark proxy overhead (% latency added, honest table).
Verify: throttle visibly slows your lab transfer; 10MB streams under memory cap; overhead table produced.
**Lesson check:** what does a token bucket do — and where does proxy overhead actually live (copy? TLS? scheduling)?

## PM10 — the frontier: QUIC, gRPC, DNS (reading + taste)
Concept: HTTP/3 rides UDP (proxies can't just CONNECT-tunnel it), gRPC is HTTP/2 with length-prefixed frames, DNS can be hijacked at a hundred points. Do: reading+notes on HTTP/3 + why it fights MITM (UDP, always-encrypted), HTTP/2 framing (parse a gRPC frame's length+flags by hand from a capture), DNS interception concept (respond before the resolver); mark which of these your v1 will ship (likely gRPC-decode-lite) and which stay reading.
Verify: gRPC length-prefix parse works on a captured frame; HTTP/3 + DNS notes written.
**Lesson check:** why does moving to UDP break the proxy model — and what does HTTP/2 framing add that HTTP/1.1's "one request per connection" didn't?

## PM11 — the essay that keeps it honest
Concept: MITM is a tool: developer's friend, enterprise inspector, attacker's weapon — same bytes. Do: research (reading) corporate TLS inspection + its security-tradeoff controversy; map where YOUR tool crosses into abuse (any traffic not yours); write the boundary essay (300+ words, your position): when MITM is legitimate (own devices, own servers, authorized testing, bug bounty scope) vs when it's surveillance (others' devices, corporate snooping without consent); list the professional guardrails (scope files, consent, disable-live-switches, CA hygiene).
Verify: essay + guardrail list written; you can defend each line in an interview.
**Lesson check:** what single line separates "proxyman for my app" from "proxy against someone" — and why does CA hygiene matter for trust?

## PM12 — CAPSTONE: Proxyman-lite ships, cold
Prereq: PM0–PM11. **Close all notes.** Cold rebuild: capture loop + CONNECT handler + TLS-MITM core (CA + cert issue + ssl wrap) in one sitting, then finish v1: flow tables, HTTP/JSON decode, WS frames, rewrite/map-local/inject rules, throttle, per-flow stats, terminal UI — run ONE full session (lab server + client through you: sniff + intercept + decrypt + modify + throttle) and document it. Write `labs/proxy-capstone.md` like an OSS README + the PM11 essay as the cover letter.
**Pass = the full session works end-to-end cold; README + essay read like a professional's.**

---

## Rules
1. Verification + lesson check both true before next unit; quizzes in your own words.
2. Copying allowed only in PM0/PM3 boilerplate (server scaffold, socket loop) — parsers, proxy, CA flow, rules written by you; erase-and-retry once when stuck.
3. 2-3h/unit; stuck past that = previous unit's verification again.
4. Lab-only: your devices/servers/traffic; CA only into your own tools; snapshots = your own config backups.
5. Honest bar: Proxyman/Charles/mitmproxy are years of polish; this course's bar = your own working sniffer + intercepting TLS proxy with decode/rewrite/map/throttle proven on your own traffic, cold at the capstone — the floor for network-tooling engineering and the exact model behind every dev-proxy, every enterprise inspector, and every C2-shaped proxy you'll ever read.

## Where this lives
Built on your 24-20 sniffer, ND/US header skills, and NETEXEC's protocol fluency; the C2/beacon shape of WINDOWS-MALWARE M8 is this proxy with a different spirit — knowing both sides is the point. AUTH-TOOLING's Kerberos/SMB work shows the same wire-reading discipline one layer down.

The building twin — a VPN is the proxy's mirror (proxy TEARS TLS apart; VPN BUILDS the same bytes into a tunnel): [`VPN-COURSE.md`](VPN-COURSE.md) VP0–VP12.