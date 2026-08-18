# 24-109 · Own plist parser — bplist00, the config format under every Apple app (stretch)

**Week:** W46+ opt (stretch) · **Track:** P · **Pairs:** 24-104, 24-97, 24-45, 24-105

## Objective
Entitlements, Info.plist, provisioning profiles, launchd jobs, logmetadata — Apple's plist is the config format you keep parsing: build both parsers — XML plist (the `<dict>/<key>` tree, 24-09-lite) and binary plist (`bplist00` — the header, object table + offset table, the 7-bit encoded integers and the object graph, a format RE pure and simple), plus a serializer for your own writes. Then use it: read your own 24-104 test app's Info.plist/entitlements, decode a real provisioning profile (mobileprovision = CMS blob — extract + parse the embedded plist with 24-70 crypto), and the malformed-object lab (bad offsets, cycle bombs) degrades cleanly — the format that trusts bytes, hardened.

## Tasks
- [ ] XML: plist DTD-lite tree walk; type mapping (dict/array/string/date/data)
- [ ] bplist00: header + trailer (offset table size/position), object table walk (marker bytes → types), the integer/real encodings, `uid` (the CoreFoundation thing), cycles
- [ ] Serialize: write bplist from your own dict model (round-trip byte-level vs Apple's `plutil` — the oracle)
- [ ] Real data: parse your own 24-104 test app's Info.plist + entitlements (what gates injection — pairs 104); `mobileprovision` CMS unwrap → embedded plist (24-70) — the profile's expiry/capability fields — `labs/`
- [ ] Robustness lab: corrupt offset table, cycle bomb, huge lengths → clean failure; fuzz-style sweep (05-12) — `labs/`
- [ ] Writeup: why plists are a parsing sea (every tool mis-parses something — the 08 parser-divergence lesson on Apple), plist in malware (XProtect bypasses — 12) — `notes/`

## Resources
- Apple plist docs + bplist RE notes (the manual — CVE-style writeups are the best spec); your 24-104/24-97/24-45 code

## Exit Criteria
- [ ] Round-trip bplist/xml matches `plutil` byte-for-byte — `labs/` + `code/`
- [ ] Profile/entitlement decode + robustness writeup — `labs/` + `notes/`

## Links
- [Property List Programming Guide](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/PropertyLists/)
- [bplist format notes](https://medium.com/@karaiskc/understanding-apple-s-binary-property-list-format-2d5d4a01b9dc)