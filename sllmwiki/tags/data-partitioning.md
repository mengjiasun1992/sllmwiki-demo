# data-partitioning

<!-- sllmwiki:generated: rewritten by wiki-check.py --fix; edit tags.yaml and the manifests instead -->

Splitting a dataset across nodes by key: hash, range and directory partitioning, and rebalancing

**Path:** [distributed-systems](distributed-systems.md) / data-partitioning

## Subtopics

- [consistent-hashing](consistent-hashing.md): Consistent hashing and its alternatives: the hash ring, virtual nodes, rendezvous, jump and Maglev hashing, bounded loads

## Narratives

- [How do the ways of hashing keys to servers compare: modulo, ring, rendezvous, jump, Maglev and bounded loads?](../narratives/how-do-the-ways-of-hashing-keys-to-servers-compare.md)
- [What is consistent hashing?](../narratives/what-is-consistent-hashing.md) (supporting)

## Sources

- [consistent-hashing-stoica-chord](../sources/consistent-hashing-stoica-chord.md): Stoica, Morris, Karger, Kaashoek and Balakrishnan's SIGCOMM 2001 paper presents Chord, a distributed lookup protocol for peer-to-peer systems that supports a single operation: given a key, map it onto a…
- [data-partitioning-cassandra-dynamo-architecture](../sources/data-partitioning-cassandra-dynamo-architecture.md): This page of the Apache Cassandra architecture documentation explains which techniques Cassandra takes from Amazon's Dynamo: dataset partitioning with consistent hashing, multi-master replication with versioned data and tunable consistency, gossip-based…
- [amazon-dynamo-elhemali-dynamodb-atc2022](../sources/amazon-dynamo-elhemali-dynamodb-atc2022.md): Elhemali et al.
- [amazon-dynamo-aws-dynamodb-partitions](../sources/amazon-dynamo-aws-dynamodb-partitions.md): This page of the Amazon DynamoDB Developer Guide (AWS documentation) explains how DynamoDB stores table data in partitions: allocations of SSD-backed storage that are automatically replicated across multiple Availability Zones…
