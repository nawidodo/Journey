# 24-48 · Own orchestrator — kube-lite: scheduler + health checks on your runtime (stretch)

**Week:** W46+ opt (stretch) · **Track:** P · **Prev:** [`../47-own-js-interpreter`](../47-own-js-interpreter/README.md)

## Objective
You built the container runtime (24-13); now orchestrate it. A kube-lite: declarative API (desired state), scheduler (resource-aware placement), health checks + restart loop (the reconciliation pattern), a simple etcd-like store (reuse 24-42). The security angle: why orchestrators are the crown jewels — API authn/authz, pod escape surface (pairs 24-13), and the supply-chain blast radius.

## Tasks
- [ ] API: declarative spec (your own YAML-lite), desired-state store (reuse 24-42 as the backing store); controller loop watches → reconciles
- [ ] Scheduler: resource-fit (CPU/mem from cgroups — pairs 24-13), spread/affinity rules, placement events
- [ ] Health: liveness/readiness probes, restart policy, backoff; kill a workload — the controller restores it (the demo)
- [ ] Security lab: API with no auth → unauthorized reconcile; add token authn + RBAC-lite; pod breakout attempt (reuse 24-13 escape matrix) — `labs/`
- [ ] Self-check: N containers on your 24-13 runtime, one dies, orchestrator restores; scale up/down works

## Resources
- Kubernetes design docs (the manual — the reconciliation idea); kubelet/kube-scheduler source (peer); your 24-13/24-42 notes

## Exit Criteria
- [ ] Orchestrator runs workloads on 24-13 runtime, self-heals — `labs/`
- [ ] Authn/RBAC + breakout lab — `labs/` + `notes/`

## Links
- [Kubernetes concepts](https://kubernetes.io/docs/concepts/)
- [kube-scheduler](https://github.com/kubernetes/kubernetes/tree/master/cmd/kube-scheduler)
