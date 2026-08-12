# 23-03 · Coordinated Disclosure

**Week:** post-M23 (ongoing) · **Track:** O · **Prev:** [`../02-resume-interview`](../02-resume-interview/README.md) · **Next:** [`../04-capstone-career-launch`](../04-capstone-career-launch/README.md)

## Objective
Disclosure process is a syssec job function and the one thing the offensive tracks never trained. Find one real bug (in your own lab software first; an open-source project second), write a proper advisory, and walk the full disclosure timeline. Your own labs from this journey are legitimate targets — and the safest place to learn the process.

## Tasks
- [ ] Choose a target: a bug you wrote during the journey (driver, emulator, engine, implant) or a bug in an OSS project you already use (reproducible, own environment)
- [ ] Write the advisory: summary, CVSS, affected versions, root cause, trigger, impact, PoC, fix — `labs/`
- [ ] Report through the right channel (vendor security contact, GitHub security advisory, HackerOne program) with a 90-day-style timeline
- [ ] Handle the exchange: ask for CVE, respond to triage, verify the fix
- [ ] Write up the process afterwards — what you'd do differently — `notes/`

## Resources
- Your own labs (the ethical, legal target) — every step's `labs/`
- DISCLOSE/CERT-CC guidance; GitHub security advisories; HackerOne disclosure 101

## Exit Criteria
- [ ] One advisory written and submitted to a real channel — `labs/` (redact personal data; keep PoC minimal)
- [ ] Timeline documented from report → response → fix — `notes/`

## Links
- [CERT/CC vulnerability disclosure guidance](https://vuls.cert.org/confluence/display/CVD)
- [GitHub security advisories](https://docs.github.com/en/code-security/security-advisories)
- [HackerOne disclosure 101](https://www.hackerone.com/disclosure-guidelines)