# 24-74 · Own CI runner — YAML pipelines into your containers (stretch)

**Week:** W46+ opt (stretch) · **Track:** P · **Prev:** [`../73-own-svg-renderer`](../73-own-svg-renderer/README.md) · **Next:** [`../75-own-package-registry`](../75-own-package-registry/README.md) · **Pairs:** 24-03, 24-13, 24-48, 24-39

## Objective
You wrote the git (24-03), the container runtime (24-13), and the orchestrator (24-48): assemble them into a CI. A runner-lite: YAML pipeline (jobs/steps — your 24-09 parser), job isolation in your own 24-13 containers, artifact passing (24-42), checkout via your 24-03 git, and the security layer: untrusted-workload sandboxing, secret handling (env redaction — pairs 20-12 zeroization), and supply-chain risk (pinning, pairs 24-39). The demo: push to your git → CI builds your 24-63 raycaster and publishes an artifact.

## Tasks
- [ ] Pipeline: YAML-lite parse, job graph + dependency ordering (DAG — pairs 24-72), step execution
- [ ] Isolation: each job in your 24-13 container (no network, read-only fs — the sandbox defaults), resource limits (24-16)
- [ ] Artifacts + secrets: job output → store (24-42), pass to next; secrets injected + redacted from logs (the leak lab)
- [ ] Security lab: malicious "repository" workflow → tries host access (seccomp pair 24-13), env theft → redaction catches it — `labs/`
- [ ] Self-check: 24-03 push triggers full pipeline; artifact = runnable raycaster

## Resources
- GitHub Actions YAML docs (the manual — the model); your 24-03/24-13/24-48/24-39 code

## Exit Criteria
- [ ] Push-triggered pipeline runs jobs in isolated containers, artifacts flow — `labs/`
- [ ] Sandbox + secret-redaction lab — `labs/` + `notes/`

## Links
- [GitHub Actions workflow syntax](https://docs.github.com/en/actions/reference/workflow-syntax-for-github-actions)
- [act (local runner)](https://github.com/nektos/act)