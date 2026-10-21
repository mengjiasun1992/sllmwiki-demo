# raft-leader-election

<!-- sllmwiki:generated: rewritten by wiki-check.py --fix; edit tags.yaml and the manifests instead -->

How Raft servers elect a leader: terms, timeouts, votes and the election restriction

**Path:** [distributed-systems](distributed-systems.md) / [consensus](consensus.md) / [raft](raft.md) / raft-leader-election

## Sources

- [raft-dissertation-basic-raft](../sources/raft-dissertation-basic-raft.md): The basic Raft chapter of Diego Ongaro's 2014 Stanford PhD dissertation, Consensus: Bridging Theory and Practice (LaTeX source, CC BY 4.0), which expands the Raft paper.
- [raft-dissertation-leader-election-eval](../sources/raft-dissertation-leader-election-eval.md): The opening of the leader election evaluation chapter of Diego Ongaro's dissertation (CC BY 4.0).
- [raft-dissertation-prevote](../sources/raft-dissertation-prevote.md): The Pre-Vote section of the leader election chapter of Diego Ongaro's dissertation (CC BY 4.0).
- [raft-etcd-readme](../sources/raft-etcd-readme.md): The README of etcd-io/raft (Apache-2.0), the Go Raft library used by etcd, Kubernetes, CockroachDB, TiDB and others, which it describes as the most widely used Raft library in production.
- [raft-paper-extended](<https://raft.github.io/raft.pdf>): In Search of an Understandable Consensus Algorithm (Extended Version), by Diego Ongaro and John Ousterhout, Stanford University; the extended version of the USENIX ATC 2014 paper.
