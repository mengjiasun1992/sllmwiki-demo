# consistent-hashing

<!-- sllmwiki:generated: rewritten by wiki-check.py --fix; edit tags.yaml and the manifests instead -->

Consistent hashing and its alternatives: the hash ring, virtual nodes, rendezvous, jump and Maglev hashing, bounded loads

**Path:** [distributed-systems](distributed-systems.md) / [data-partitioning](data-partitioning.md) / consistent-hashing

## Narratives

- [What is consistent hashing?](../narratives/what-is-consistent-hashing.md)

## Sources

- [consistent-hashing-wikipedia](../sources/consistent-hashing-wikipedia.md): Wikipedia article on consistent hashing: a hashing technique in which resizing a hash table remaps only n/m keys on average (n keys, m slots), whereas a modulo mapping remaps nearly…
- [consistent-hashing-wikipedia-rendezvous-hashing](../sources/consistent-hashing-wikipedia-rendezvous-hashing.md): Wikipedia article on rendezvous or highest random weight (HRW) hashing, an algorithm by which clients independently agree on a set of k of n sites for an object: each site…
- [consistent-hashing-lamping-veach-jump-hash](../sources/consistent-hashing-lamping-veach-jump-hash.md): John Lamping and Eric Veach of Google present jump consistent hash, a consistent hash function of about five lines of C++ that maps a 64-bit key and a number of…
- [consistent-hashing-mirrokni-bounded-loads](../sources/consistent-hashing-mirrokni-bounded-loads.md): Vahab Mirrokni, Mikkel Thorup and Morteza Zadimoghaddam (Google Research and University of Copenhagen; arXiv 1608.01350, version of July 2017) study dynamic load balancing where clients (balls) and servers (bins) can…
- [consistent-hashing-eisenbud-maglev](../sources/consistent-hashing-eisenbud-maglev.md): This NSDI 2016 paper by Daniel E.
- [consistent-hashing-stoica-chord](../sources/consistent-hashing-stoica-chord.md): Stoica, Morris, Karger, Kaashoek and Balakrishnan's SIGCOMM 2001 paper presents Chord, a distributed lookup protocol for peer-to-peer systems that supports a single operation: given a key, map it onto a…
- [data-partitioning-cassandra-dynamo-architecture](../sources/data-partitioning-cassandra-dynamo-architecture.md): This page of the Apache Cassandra architecture documentation explains which techniques Cassandra takes from Amazon's Dynamo: dataset partitioning with consistent hashing, multi-master replication with versioned data and tunable consistency, gossip-based…
- [amazon-dynamo-decandia-dynamo-sosp2007](../sources/amazon-dynamo-decandia-dynamo-sosp2007.md): DeCandia et al.
- [consistent-hashing-gryski-algorithmic-tradeoffs](<https://dgryski.medium.com/consistent-hashing-algorithmic-tradeoffs-ef6b8e2fcae8>): Damian Gryski's 2018 practitioner survey of consistent hashing algorithms (ring with virtual nodes, rendezvous, jump, multi-probe, Maglev, bounded loads) and their trade-offs in memory, lookup speed and load balance.
- [consistent-hashing-karger-stoc1997](../sources/consistent-hashing-karger-stoc1997.md): The 1997 STOC paper by Karger, Lehman, Leighton, Levine, Lewin and Panigrahy (MIT) that introduced consistent hashing, 'Consistent Hashing and Random Trees: Distributed Caching Protocols for Relieving Hot Spots on…
