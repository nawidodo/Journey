# 24-88 · Own LDAP-lite — bind/search directory, the auth backplane (stretch)

**Week:** W46+ opt (stretch) · **Track:** P · **Prev:** [`../87-own-linker`](../87-own-linker/README.md) · **Next:** [`../89-own-tts`](../89-own-tts/README.md) · **Pairs:** 25-09, 24-17, 20-07

## Objective
Every corporate auth decision lands in a directory: build an LDAP-lite — BER/ASN.1 encoding (pairs 24-70 DER reuse), bind (the auth path — password + SASL-lite), search with scope/filter (the query engine — pairs 24-41), schema + DIT (the tree), and referral/ACL-lite. Then the security lab that makes it real: bind brute-force (rate limiting — pairs 24-17), anonymous access misconfig (the enumeration leak — pairs 25-09), LDAP-injection in your own search filter parser (the classic auth-bypass bug).

## Tasks
- [ ] Wire: BER encode/decode (24-70 discipline), LDAP protocol ops (bind/search/result), operation flow
- [ ] Directory: DIT tree + schema (objectClass/attributes), storage over your 24-42 KV, search filter parser (pairs 24-06 regex)
- [ ] Auth: bind checks, password hash store (20-12), SASL-lite (pairs 20-07), ACL on entries/attrs
- [ ] Security lab: filter injection (`(&(uid=admin)(pwd=*))`-style) against your own parser → bypass demo + fix (parameterized filter — pairs 24-17 injection lessons); anonymous-listing misconfig — `labs/`
- [ ] Interop: ldapsearch (OpenLDAP client) binds + searches your server (the oracle)

## Resources
- RFC 4511 (the manual); OpenLDAP source (peer); your 24-70/24-42/24-17 code

## Exit Criteria
- [ ] LDAP bind/search works with real client interop — `labs/`
- [ ] Injection + misconfig lab and fixes — `labs/` + `notes/`

## Links
- [RFC 4511 LDAP](https://www.rfc-editor.org/rfc/rfc4511)
- [OpenLDAP](https://www.openldap.org/)