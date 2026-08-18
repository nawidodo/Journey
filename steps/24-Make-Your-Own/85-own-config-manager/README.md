# 24-85 · Own config manager — ansible-lite: idempotent apply over your SSH (stretch)

**Week:** W46+ opt (stretch) · **Track:** P · **Prev:** [`../84-own-init-system`](../84-own-init-system/README.md) · **Pairs:** 24-12, 24-48, 24-30

## Objective
Every fleet is configured by a declarative agent: build one — a manifest (YAML-lite — 24-09), an SSH transport (your own 24-12 server/client), and **idempotent modules** (file/package/user-lite) that only act when state differs (check-then-apply — the 24-48 reconciliation pattern). The discipline is the deliverable: run the manifest 100× — nothing changes after the first; drift detection (check mode) finds an out-of-band edit. Security tie: config management is the pivot in every enterprise compromise (pairs 25-09 AD, 21-02 hunting) — SSH key hygiene and the "who can push config" trust boundary.

## Tasks
- [ ] Manifest: declarative state list, module dispatch (the parser + plugin table)
- [ ] Transport: run modules over your 24-12 SSH (or local exec in the same VM), collect results
- [ ] Modules: file (content/perm/mode), package-lite (own 24-39), user-lite; idempotence checks first (the apply-skip path)
- [ ] Drift lab: check mode flags an out-of-band edit; apply restores; no-op run changes nothing (the 100× test) — `labs/`
- [ ] Writeup: trust-boundary analysis (who can push what), SSH-key hygiene (24-12) — `notes/`

## Resources
- Ansible docs (the manual — the module contract); your 24-12/24-48/24-30 code

## Exit Criteria
- [ ] Manifest applies idempotently over SSH; drift detected + restored — `labs/` + `code/`
- [ ] Trust-boundary writeup — `notes/`

## Links
- [Ansible module docs](https://docs.ansible.com/ansible/latest/module_plugin_guide/)
- [Ansible](https://www.ansible.com/)