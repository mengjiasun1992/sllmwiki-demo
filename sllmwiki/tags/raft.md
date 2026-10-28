# raft

<!-- sllmwiki:generated: rewritten by wiki-check.py --fix; edit tags.yaml and the manifests instead -->

The Raft consensus algorithm: leader election, log replication and safety for a replicated log

**Path:** [distributed-systems](distributed-systems.md) / [consensus](consensus.md) / raft

## Narratives

- [How does Raft elect a leader and keep every server's log consistent through crashes and network partitions?](../narratives/how-does-raft-elect-a-leader-and-keep-every-servers-log-consistent.md) · 9 min
- [Can you walk me through the classic failure modes in large scale distributed system and what are the common solutions to prevent or mitigate each one?](../narratives/what-are-the-classic-failure-modes-of-large-scale-distributed-systems-and-their-mitigations.md) · 9 min (supporting)

## Sources

- [distributed-systems-kleppmann-cambridge-notes](../sources/distributed-systems-kleppmann-cambridge-notes.md): Martin Kleppmann, lecture notes for the University of Cambridge Part IB course 'Concurrent and Distributed Systems', distributed half, 2021/22 (CC BY-SA).
- [distributed-system-design-ongaro-raft-consensus](<https://raft.github.io/raft.pdf>): Diego Ongaro and John Ousterhout's 'In Search of an Understandable Consensus Algorithm (Extended Version)' (Stanford, 2014; extended from the USENIX ATC 2014 paper), the paper that introduced Raft.
- [raft-ongaro-dissertation-ch3-basic-raft](../sources/raft-ongaro-dissertation-ch3-basic-raft.md): Chapter 3, 'Basic Raft algorithm', of Diego Ongaro's Stanford PhD dissertation 'Consensus: Bridging Theory and Practice' (2014, CC BY 4.0), saved from its LaTeX source; it is the extended form…
- [raft-ongaro-dissertation-ch9-leader-election](../sources/raft-ongaro-dissertation-ch9-leader-election.md): Chapter 9, 'Leader election evaluation', of Diego Ongaro's Stanford PhD dissertation 'Consensus: Bridging Theory and Practice' (2014, CC BY 4.0), saved from its LaTeX source.
- [raft-etcd-raft-readme](../sources/raft-etcd-raft-readme.md): The README of etcd-io/raft (Apache-2.0), the Go Raft library used by etcd, Kubernetes, CockroachDB, TiKV, Docker Swarm and others, and described as the most widely used Raft library in production.
