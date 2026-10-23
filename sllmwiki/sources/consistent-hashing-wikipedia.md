# Consistent hashing - Wikipedia

From Wikipedia, the free encyclopedia

| [image: ](<https://thumb.wikimedia.org/wikipedia/en/thumb/f/f2/Edit-clear.svg/40px-Edit-clear.svg.png?utm_source=en.wikipedia.org&utm_campaign=parser&utm_content=thumbnail>) | This article **may be too technical for most readers to understand**. Please [help improve it](<https://en.wikipedia.org/w/index.php?title=Consistent_hashing&action=edit>) to [make it understandable to non-experts](<https://en.wikipedia.org/wiki/Wikipedia:Make_technical_articles_understandable>), without removing the technical details. *(October 2024)**([Learn how and when to remove this message](<https://en.wikipedia.org/wiki/Help:Maintenance_template_removal>))* |
|---|---|

In [computer science](<https://en.wikipedia.org/wiki/Computer_science>), **consistent hashing**\[1\]\[2\] is a special kind of [hashing](<https://en.wikipedia.org/wiki/Hash_function>) technique such that when a [hash table](<https://en.wikipedia.org/wiki/Hash_table>) is resized, only $n/m$ keys need to be remapped on average where $n$ is the number of keys and $m$ is the number of slots. Consistent hashing evenly distributes cache keys across [shards](<https://en.wikipedia.org/wiki/Shard_(database_architecture)>), even if some of the shards crash or become unavailable.\[3\] In contrast, in most traditional hash tables, a change in the number of array slots causes nearly all keys to be remapped because the mapping between the keys and the slots is defined by a [modular operation](<https://en.wikipedia.org/wiki/Modular_arithmetic>).

Consistent hashing is used by [Content Delivery Networks](<https://en.wikipedia.org/wiki/Content_Delivery_Network>) because it is useful for distributing requests for content from a rotating population of web servers. [Tim Berners-Lee](<https://en.wikipedia.org/wiki/Tim_Berners-Lee>) credits consistent hashing algorithms, and [Daniel Lewin](<https://en.wikipedia.org/wiki/Daniel_Lewin>) as their inventor, with solving the [slashdotting](<https://en.wikipedia.org/wiki/Slashdotting>) problem which plagued the [World Wide Web](<https://en.wikipedia.org/wiki/World_Wide_Web>) in the 1990s.\[4\]

## History

\[[edit](<https://en.wikipedia.org/w/index.php?title=Consistent_hashing&action=edit&section=1>)\]

The term "consistent hashing" was introduced by [David Karger](<https://en.wikipedia.org/wiki/David_Karger>) *et al.* at [MIT](<https://en.wikipedia.org/wiki/MIT>) for use in [distributed caching](<https://en.wikipedia.org/wiki/Distributed_cache>), particularly for the [web](<https://en.wikipedia.org/wiki/World_Wide_Web>).\[5\] This academic paper from 1997 in [Symposium on Theory of Computing](<https://en.wikipedia.org/wiki/Symposium_on_Theory_of_Computing>) introduced the term "consistent hashing" as a way of distributing requests among a changing population of web servers.\[6\] Each slot is then represented by a server in a distributed system or cluster. The addition of a server and the removal of a server (during scalability or outage) requires only $num\_keys/num\_slots$ items to be re-shuffled when the number of slots (i.e. servers) change. The authors mention [linear hashing](<https://en.wikipedia.org/wiki/Linear_hashing>) and its ability to handle sequential server addition and removal, while consistent hashing allows servers to be added and removed in an arbitrary order. \[1\] The paper was later re-purposed to address technical challenge of keeping track of a file in [peer-to-peer networks](<https://en.wikipedia.org/wiki/Peer-to-peer>) such as a [distributed hash table](<https://en.wikipedia.org/wiki/Distributed_hash_table>).\[7\]\[8\]

[Teradata](<https://en.wikipedia.org/wiki/Teradata>) used this technique in their [distributed database](<https://en.wikipedia.org/wiki/Distributed_database>)\[*[citation needed](<https://en.wikipedia.org/wiki/Wikipedia:Citation_needed>)*\], released in 1986, although they did not use this term. Teradata still uses the concept of a [hash table](<https://en.wikipedia.org/wiki/Hash_table>) to fulfill exactly this purpose. [Akamai Technologies](<https://en.wikipedia.org/wiki/Akamai_Technologies>) was founded in 1998 by the scientists [Daniel Lewin](<https://en.wikipedia.org/wiki/Daniel_Lewin>) and [F. Thomson Leighton](<https://en.wikipedia.org/wiki/F._Thomson_Leighton>) (co-authors of the article coining "consistent hashing"). In Akamai's content delivery network,\[9\] consistent hashing is used to balance the load within a cluster of servers, while a [stable marriage](<https://en.wikipedia.org/wiki/Stable_marriage_problem>) algorithm is used to balance load across clusters.\[2\]

Consistent hashing has also been used to reduce the impact of partial system failures in large web applications to provide robust caching without incurring the system-wide fallout of a failure.\[10\] Consistent hashing is also the cornerstone of [distributed hash tables](<https://en.wikipedia.org/wiki/Distributed_hash_table>) (DHTs), which employ hash values to partition a keyspace across a distributed set of nodes, then construct an [overlay network](<https://en.wikipedia.org/wiki/Overlay_network>) of connected nodes that provide efficient node retrieval by key.

[Rendezvous hashing](<https://en.wikipedia.org/wiki/Rendezvous_hashing>), designed in 1996, is a simpler and more general technique \[*[citation needed](<https://en.wikipedia.org/wiki/Wikipedia:Citation_needed>)*\]. It achieves the goals of consistent hashing using the very different highest random weight (HRW) algorithm.

## Basic technique

\[[edit](<https://en.wikipedia.org/w/index.php?title=Consistent_hashing&action=edit&section=2>)\]

[[image: ](<https://thumb.wikimedia.org/wikipedia/commons/thumb/7/71/Consistent_Hashing_Sample_Illustration.png/250px-Consistent_Hashing_Sample_Illustration.png?utm_source=en.wikipedia.org&utm_campaign=parser&utm_content=thumbnail>)](<https://en.wikipedia.org/wiki/File:Consistent_Hashing_Sample_Illustration.png>)

In this case, using consistent hashing would result in the "BLOB" getting stored server 139. A BLOB is mapped to the next server that appears on the circle in clockwise order until it reaches a server which is $\zeta \leq {\text{server ID}}$

In the problem of [load balancing](<https://en.wikipedia.org/wiki/Load_balancing_(computing)>), for example, when a [BLOB](<https://en.wikipedia.org/wiki/Binary_large_object>) has to be assigned to one of $n$ servers on a [cluster](<https://en.wikipedia.org/wiki/Computer_cluster>), a standard hash function could be used in such a way that we calculate the hash value for that BLOB, assuming the resultant value of the hash is $\beta$, we perform [modular operation](<https://en.wikipedia.org/wiki/Modular_arithmetic>) with the number of servers ($n$ in this case) to determine the server in which we can place the BLOB: $\zeta =\beta \ \%\ n$; hence the BLOB will be placed in the server whose ${\text{server ID}}$ is successor of $\zeta$ in this case. However, when a server is added or removed during outage or scaling (when $n$ changes), all the BLOBs in every server should be reassigned and moved due to [rehashing](<https://en.wikipedia.org/wiki/Hash_table#Dynamic_resizing>), but this operation is expensive.

Consistent hashing was designed to avoid the problem of having to reassign every BLOB when a server is added or removed throughout the cluster. The central idea is to use a hash function that maps both the BLOB and servers to a unit circle, usually $2\pi$ radians. For example, $\zeta =\Phi \ \%\ 360$ (where $\Phi$ is hash of a BLOB or server's identifier, like [IP address](<https://en.wikipedia.org/wiki/IP_address>) or [UUID](<https://en.wikipedia.org/wiki/Universally_unique_identifier>)). Each BLOB is then assigned to the next server that appears on the circle in clockwise order. Usually, [binary search algorithm](<https://en.wikipedia.org/wiki/Binary_search_algorithm>) or [linear search](<https://en.wikipedia.org/wiki/Linear_search>) is used to find a "spot" or server to place that particular BLOB in $O(\log N)$ or $O(N)$ complexities respectively; and in every iteration, which happens in clockwise manner, an operation $\zeta \ \leq \ \Psi$ (where $\Psi$ is the value of the server within the cluster) is performed to find the server to place the BLOB. This provides an even distribution of BLOBs to servers. But, more importantly, if a server fails and is removed from the circle, only the BLOBs that were mapped to the failed server need to be reassigned to the next server in clockwise order. Likewise, if a new server is added, it is added to the unit circle, and only the BLOBs that would now be assigned to the new server need to be reassigned. These are BLOBs previously assigned to an existing server, but for which the new server is now the next server clockwise.

Importantly, when a server is added or removed, the vast majority of the BLOBs maintain their prior server assignments, and the addition of $n^{th}$ server only causes $1/n$ fraction of the BLOBs to relocate. Although the process of moving BLOBs across cache servers in the cluster depends on the context, commonly, the newly added cache server identifies its "predecessor" and moves all the BLOBs, whose mapping belongs to this server (i.e. whose hash value is less than that of the new server), from it. However, in the case of [web page caches](<https://en.wikipedia.org/wiki/Web_cache>), in most implementations there is no involvement of moving or copying, assuming the cached BLOB is small enough. When a request hits a newly added cache server, a [cache miss](<https://en.wikipedia.org/wiki/Cache_(computing)#CACHE-MISS>) happens and a request to the actual [web server](<https://en.wikipedia.org/wiki/Web_server>) is made and the BLOB is cached locally for future requests. The redundant BLOBs on the previously used cache servers would be removed as per the [cache eviction policies](<https://en.wikipedia.org/wiki/Cache_replacement_policies>).\[11\]

### Implementation

\[[edit](<https://en.wikipedia.org/w/index.php?title=Consistent_hashing&action=edit&section=3>)\]

Let $h_{b}(x)$ and $h_{s}(x)$ be the hash functions used for the BLOB and server's unique identifier respectively. In practice, a [binary search tree](<https://en.wikipedia.org/wiki/Binary_search_tree>) (BST) is used to dynamically maintain the ${\text{server ID}}$ within a cluster or hashring, and to find the successor or minimum within the BST, [tree traversal](<https://en.wikipedia.org/wiki/Tree_traversal>) is used.

**Inserting $x$ into the cluster**

Let $\beta$ be the hash value of a BLOB such that, $h_{b}(x)=\beta \ \%\ 360$ where $x\in \mathrm {BLOB}$ and $h_{b}(x)=\zeta$. To insert $x$, find the successor of $\zeta$ in the BST of ${\text{server ID}}$s. If $\zeta$ is larger than all of the ${\text{server ID}}$s, the BLOB is placed in the server with smallest ${\text{server ID}}$ value.

**Deleting $x$ from the cluster**

Find the successor of $\zeta$ in the BST, remove the BLOB from the returned ${\text{server ID}}$. If $\zeta$ has no successor, remove the BLOB from the smallest of the ${\text{server ID}}$s.\[12\]

**Insert a server into cluster**

Let $\Phi$ be the hash value of a server's identifier such that, $h_{s}(x)=\Phi \ \%\ 360$ where $x\in \{{\text{IP address, UUID}}\}$ and $h_{s}(x)=\theta$. Move all the BLOBs, whose hash value is smaller than $\theta$, from the server whose ${\text{server ID}}$ is successor of $\theta$. If $\theta$ is largest of all the ${\text{server ID}}$s, move the relevant BLOBs from the smallest of the ${\text{server ID}}$s into $\theta$.\[13\]

**Delete a server from cluster**

Find the successor of $\theta$ in the BST, move the BLOBs from $\theta$ into its successor server. If $\theta$ doesn't have a successor, move the BLOBs into the smallest of the ${\text{server ID}}$s.\[14\]

### Variance reduction

\[[edit](<https://en.wikipedia.org/w/index.php?title=Consistent_hashing&action=edit&section=4>)\]

To avoid [skewness](<https://en.wikipedia.org/wiki/Skewness>) of multiple nodes within the radian, which happen due to lack of [uniform distribution](<https://en.wikipedia.org/wiki/Discrete_uniform_distribution>) of the servers within the cluster, multiple labels are used. Those duplicate labels are called "virtual nodes" i.e. multiple labels which point to a single "real" label or server within the cluster. The amount of virtual nodes or duplicate labels used for a particular server within a cluster is called the "weight" of that particular server.\[15\]

## Practical extensions

\[[edit](<https://en.wikipedia.org/w/index.php?title=Consistent_hashing&action=edit&section=5>)\]

A number of extensions to the basic technique are needed for effectively using consistent hashing for load balancing in practice. In the basic scheme above, if a server fails, all its BLOBs are reassigned to the next server in clockwise order, potentially doubling the load of that server. This may not be desirable. To ensure a more even redistribution of BLOBs on server failure, each server can be hashed to multiple locations on the unit circle. When a server fails, the BLOBs assigned to each of its replicas on the unit circle will get reassigned to a different server in clockwise order, thus redistributing the BLOBs more evenly. Another extension concerns a situation where a single BLOB gets "hot" and is accessed a large number of times and will have to be hosted in multiple servers. In this situation, the BLOB may be assigned to multiple contiguous servers by traversing the unit circle in clockwise order. A more complex practical consideration arises when two BLOBs are hashed near each other in the unit circle and both get "hot" at the same time. In this case, both BLOBs will use the same set of contiguous servers in the unit circle. This situation can be ameliorated by each BLOB choosing a different hash function for mapping servers to the unit circle.\[2\]

## Comparison with rendezvous hashing and other alternatives

\[[edit](<https://en.wikipedia.org/w/index.php?title=Consistent_hashing&action=edit&section=6>)\]

Main article: [Rendezvous hashing § Comparison with consistent hashing](<https://en.wikipedia.org/wiki/Rendezvous_hashing#_Comparison_with_consistent_hashing>)

[Rendezvous hashing](<https://en.wikipedia.org/wiki/Rendezvous_hashing>), designed in 1996, is a simpler and more general technique, and permits fully distributed agreement on a set of $k$ options out of a possible set of $n$ options. [It can in fact be shown](<https://en.wikipedia.org/wiki/Rendezvous_hashing#Consistent_hashing_is_a_special_case_of_Rendezvous_hashing>) that consistent hashing is a special case of rendezvous hashing. Because of its simplicity and generality, rendezvous hashing is now being used in place of Consistent Hashing in many applications.

If key values will always increase [monotonically](<https://en.wikipedia.org/wiki/Monotonic>), an alternative approach using a [hash table with monotonic keys](<https://en.wikipedia.org/wiki/Hash_table#Monotonic_keys>) may be more suitable than consistent hashing.\[*[citation needed](<https://en.wikipedia.org/wiki/Wikipedia:Citation_needed>)*\]

## Complexity

\[[edit](<https://en.wikipedia.org/w/index.php?title=Consistent_hashing&action=edit&section=7>)\]

|  | Classic hash table | Consistent hashing |
|---|---|---|
| add a node | $O(K)$ | $O(K/N+\log N)$ |
| remove a node | $O(K)$ | $O(K/N+\log N)$ |
| lookup a key | $O(1)$ | $O(\log N)$ |
| add a key | $O(1)$ | $O(\log N)$ |
| remove a key | $O(1)$ | $O(\log N)$ |

The $O(K/N)$ is an average cost for redistribution of keys and the $O(\log N)$ complexity for consistent hashing comes from the fact that a [binary search](<https://en.wikipedia.org/wiki/Binary_search_algorithm>) among nodes angles is required to find the next node on the ring.\[*[citation needed](<https://en.wikipedia.org/wiki/Wikipedia:Citation_needed>)*\]

## Examples

\[[edit](<https://en.wikipedia.org/w/index.php?title=Consistent_hashing&action=edit&section=8>)\]

Known examples of consistent hashing use include:

- [Couchbase](<https://en.wikipedia.org/wiki/Couchbase>) automated data partitioning \[16\]
- [OpenStack](<https://en.wikipedia.org/wiki/OpenStack>)'s Object Storage Service Swift\[17\]
- Partitioning component of Amazon's storage system [Dynamo](<https://en.wikipedia.org/wiki/Dynamo_(storage_system)>)\[18\]
- Data partitioning in [Apache Cassandra](<https://en.wikipedia.org/wiki/Apache_Cassandra>)\[19\]
- Data partitioning in [ScyllaDB](<https://en.wikipedia.org/wiki/ScyllaDB>)\[20\]
- Data partitioning in [Voldemort](<https://en.wikipedia.org/wiki/Voldemort_(distributed_data_store)>)\[21\]
- [Akka](<https://en.wikipedia.org/wiki/Akka_(toolkit)>)'s consistent hashing router\[22\]
- [Riak](<https://en.wikipedia.org/wiki/Riak>), a distributed key-value database\[23\]
- [Gluster](<https://en.wikipedia.org/wiki/Gluster>), a [network-attached storage](<https://en.wikipedia.org/wiki/Network-attached_storage>) [file system](<https://en.wikipedia.org/wiki/File_system>)\[24\]
- [Akamai](<https://en.wikipedia.org/wiki/Akamai_Technologies>) content delivery network\[25\]
- [Discord](<https://en.wikipedia.org/wiki/Discord_(software)>) chat application\[26\]
- Load balancing [gRPC](<https://en.wikipedia.org/wiki/GRPC>) requests to a distributed cache in SpiceDB\[27\]
- [Chord](<https://en.wikipedia.org/wiki/Chord_(peer-to-peer)>) algorithm\[28\]
- [MinIO](<https://en.wikipedia.org/wiki/MinIO>) object storage system\[29\]

## References

\[[edit](<https://en.wikipedia.org/w/index.php?title=Consistent_hashing&action=edit&section=9>)\]

1. 1 2 *Karger, D.; Lehman, E.; [Leighton, T.](<https://en.wikipedia.org/wiki/F._Thomson_Leighton>); Panigrahy, R.; Levine, M.; [Lewin, D.](<https://en.wikipedia.org/wiki/Daniel_M._Lewin>) (1997). [*Consistent Hashing and Random Trees: Distributed Caching Protocols for Relieving Hot Spots on the World Wide Web*](<http://portal.acm.org/citation.cfm?id=258660>). Proceedings of the Twenty-Ninth Annual ACM [Symposium on Theory of Computing](<https://en.wikipedia.org/wiki/Symposium_on_Theory_of_Computing>). ACM Press New York, NY, USA. pp. 654–663. [doi](<https://en.wikipedia.org/wiki/Doi_(identifier)>):[10.1145/258533.258660](<https://doi.org/10.1145%2F258533.258660>).*
2. 1 2 3 *Bruce Maggs and [Ramesh Sitaraman](<https://en.wikipedia.org/wiki/Ramesh_Sitaraman>) (2015). ["Algorithmic nuggets in content delivery"](<http://www.sigcomm.org/sites/default/files/ccr/papers/2015/July/0000000-0000009.pdf>) (PDF). *ACM SIGCOMM Computer Communication Review*. **45** (3).*
3. ↑ **Designing Distributed Systems Patterns and Paradigms for Scalable, Reliable Services*. O'Reilly Media. 2018. [ISBN](<https://en.wikipedia.org/wiki/ISBN_(identifier)>) [9781491983607](<https://en.wikipedia.org/wiki/Special:BookSources/9781491983607>).*
4. ↑ *Berners-Lee, Tim (2025). *This is for Everyone: the unfinished story of the World Wide Web*. Farrar, Straus and Giroux. p. 156. [ISBN](<https://en.wikipedia.org/wiki/ISBN_(identifier)>) [978-0-374-61246-7](<https://en.wikipedia.org/wiki/Special:BookSources/978-0-374-61246-7>).*
5. ↑ Roughgarden & Valiant 2021, p. 2.
6. ↑ Roughgarden & Valiant 2021, p. 7.
7. ↑ Roughgarden & Valiant 2021, p. 8.
8. ↑ I. Stoica et al., "Chord: a scalable peer-to-peer lookup protocol for Internet applications," in IEEE/ACM Transactions on Networking, vol. 11, no. 1, pp. 17–32, Feb. 2003, doi: 10.1109/TNET.2002.808407.
9. ↑ *Nygren., E.; Sitaraman R. K.; Sun, J. (2010). ["The Akamai Network: A Platform for High-Performance Internet Applications"](<https://www.akamai.com/site/en/documents/research-paper/the-akamai-network-a-platform-for-high-performance-internet-applications-technical-publication.pdf>) (PDF). *ACM SIGOPS Operating Systems Review*. **44** (3): 2–19. [doi](<https://en.wikipedia.org/wiki/Doi_(identifier)>):[10.1145/1842733.1842736](<https://doi.org/10.1145%2F1842733.1842736>). [S2CID](<https://en.wikipedia.org/wiki/S2CID_(identifier)>) [207181702](<https://api.semanticscholar.org/CorpusID:207181702>). [Archived](<https://web.archive.org/web/20221130124826/https://www.akamai.com/site/en/documents/research-paper/the-akamai-network-a-platform-for-high-performance-internet-applications-technical-publication.pdf>) (PDF) from the original on November 30, 2022. Retrieved August 29, 2023.*
10. ↑ *Karger, D.; Sherman, A.; Berkheimer, A.; Bogstad, B.; Dhanidina, R.; Iwamoto, K.; Kim, B.; Matkins, L.; Yerushalmi, Y. (1999). ["Web Caching with Consistent Hashing"](<https://web.archive.org/web/20080721013638/http://www8.org/w8-papers/2a-webserver/caching/paper2.html>). *Computer Networks*. **31** (11): 1203–1213. [doi](<https://en.wikipedia.org/wiki/Doi_(identifier)>):[10.1016/S1389-1286(99)00055-9](<https://doi.org/10.1016%2FS1389-1286%2899%2900055-9>). Archived from [the original](<http://www8.org/w8-papers/2a-webserver/caching/paper2.html>) on 2008-07-21. Retrieved 2008-02-05.*
11. ↑ Roughgarden & Valiant 2021, p. 6.
12. ↑ Moitra 2016, p. 2.
13. ↑ Moitra 2016, p. 2–3.
14. ↑ Moitra 2016, p. 3.
15. ↑ Roughgarden & Valiant 2021, p. 6–7.
16. ↑ *["What Exactly Is Membase?"](<https://blog.couchbase.com/what-exactly-membase/>). 16 December 2014. Retrieved 2020-10-29.*
17. ↑ *Holt, Greg (February 2011). ["Building a Consistent Hashing Ring"](<https://docs.openstack.org/swift/latest/ring_background.html>). *openstack.org*. Retrieved 2019-11-17.*
18. ↑ *DeCandia, G.; Hastorun, D.; Jampani, M.; Kakulapati, G.; Lakshman, A.; Pilchin, A.; Sivasubramanian, S.; Vosshall, P.; [Vogels, Werner](<https://en.wikipedia.org/wiki/Werner_Vogels>) (2007). ["Dynamo"](<http://www.allthingsdistributed.com/files/amazon-dynamo-sosp2007.pdf>) (PDF). *ACM SIGOPS Operating Systems Review*. **41** (6): 205–220. [doi](<https://en.wikipedia.org/wiki/Doi_(identifier)>):[10.1145/1323293.1294281](<https://doi.org/10.1145%2F1323293.1294281>). Retrieved 2018-06-07.*
19. ↑ *Lakshman, Avinash; Malik, Prashant (2010). "Cassandra: a decentralized structured storage system". *ACM SIGOPS Operating Systems Review*. **44** (2): 35–40. [doi](<https://en.wikipedia.org/wiki/Doi_(identifier)>):[10.1145/1773912.1773922](<https://doi.org/10.1145%2F1773912.1773922>). [S2CID](<https://en.wikipedia.org/wiki/S2CID_(identifier)>) [916681](<https://api.semanticscholar.org/CorpusID:916681>).*
20. ↑ *["NoSQL Comparison: MongoDB vs ScyllaDB"](<https://benchant.com/blog/mongodb-vs-scylladb-comparison>). *benchant.com*. Retrieved 21 March 2024.*
21. ↑ *["Design -- Voldemort"](<https://web.archive.org/web/20150209164650/http://www.project-voldemort.com/voldemort/design.html>). *www.project-voldemort.com/*. Archived from [the original](<http://www.project-voldemort.com/voldemort/design.html>) on 9 February 2015. Retrieved 9 February 2015. Consistent hashing is a technique that avoids these problems, and we use it to compute the location of each key on the cluster.*
22. ↑ *["Akka Routing"](<http://doc.akka.io/docs/akka/snapshot/scala/routing.html>). *akka.io*. Retrieved 2019-11-16.*
23. ↑ *["Riak Concepts"](<https://web.archive.org/web/20150919042730/http://docs.basho.com/riak/1.4.8/theory/why-riak/>). Archived from [the original](<http://docs.basho.com/riak/1.4.8/theory/why-riak/>) on 2015-09-19. Retrieved 2016-12-06.*
24. ↑ *["GlusterFS Algorithms: Distribution"](<http://www.gluster.org/2012/03/glusterfs-algorithms-distribution/>). *gluster.org*. 2012-03-01. Retrieved 2019-11-16.*
25. ↑ *Roughgarden, Tim; Valiant, Gregory (2016-03-28). ["Modern Algorithmic Toolbox"](<http://theory.stanford.edu/~tim/s16/l/l1.pdf>) (PDF). *stanford.edu*. Retrieved 2019-11-17.*
26. ↑ *Vishnevskiy, Stanislav (2017-07-06). ["How Discord Scaled Elixir to 5,000,000 Concurrent Users"](<https://discord.com/blog/how-discord-scaled-elixir-to-5-000-000-concurrent-users>). Retrieved 2022-08-16.*
27. ↑ *["Consistent Hash Load Balancing for gRPC"](<https://authzed.com/blog/consistent-hash-load-balancing-grpc>). 24 November 2021. Retrieved 2023-09-04.*
28. ↑  *[Stoica, I.](<https://en.wikipedia.org/wiki/Ion_Stoica>); Morris, R.; Liben-Nowell, D.; Karger, D.; Kaashoek, M. F.; Dabek, F.; [Balakrishnan, H.](<https://en.wikipedia.org/wiki/Hari_Balakrishnan>) (25 Feb 2003). "Chord: a scalable peer-to-peer lookup protocol for Internet applications". *IEEE/ACM Transactions on Networking*. **11** (1): 17–32. [doi](<https://en.wikipedia.org/wiki/Doi_(identifier)>):[10.1109/TNET.2002.808407](<https://doi.org/10.1109%2FTNET.2002.808407>). [S2CID](<https://en.wikipedia.org/wiki/S2CID_(identifier)>) [221276912](<https://api.semanticscholar.org/CorpusID:221276912>).*
29. ↑ *["MinIO Versioning, Metadata and Storage Deep Dive"](<https://blog.min.io/minio-versioning-metadata-deep-dive>). 3 January 2022. Retrieved 2023-10-24.*

### Works cited

\[[edit](<https://en.wikipedia.org/w/index.php?title=Consistent_hashing&action=edit&section=10>)\]

- *Moitra, Ankur (10 February 2016). ["Advanced Algorithms, 6.854"](<https://people.csail.mit.edu/moitra/docs/6854lec3.pdf>) (PDF). [Massachusetts Institute of Technology](<https://en.wikipedia.org/wiki/Massachusetts_Institute_of_Technology>). [Archived](<https://web.archive.org/web/20210413045612/http://people.csail.mit.edu/moitra/docs/6854lec3.pdf>) (PDF) from the original on 13 April 2021. Retrieved 8 October 2021.*
- *Roughgarden, Tim; Valiant, Gregory (28 March 2021). ["The Modern Algorithmic Toolbox, Introduction to Consistent Hashing"](<https://web.stanford.edu/class/cs168/l/l1.pdf>) (PDF). [Stanford University](<https://en.wikipedia.org/wiki/Stanford_University>). [Archived](<https://web.archive.org/web/20210725194111/https://web.stanford.edu/class/cs168/l/l1.pdf>) (PDF) from the original on 25 July 2021. Retrieved 7 October 2021.*

## External links

\[[edit](<https://en.wikipedia.org/w/index.php?title=Consistent_hashing&action=edit&section=11>)\]

- [Understanding Consistent hashing](<https://web.archive.org/web/20110721203235/http://www.tomkleinpeter.com/2008/03/17/programmers-toolbox-part-3-consistent-hashing/>)
- [Consistent hashing by Michael Nielsen on June 3, 2009](<http://michaelnielsen.org/blog/consistent-hashing>)
- [Consistent Hashing, Danny Lewin, and the Creation of Akamai](<https://www.youtube.com/watch?v=apHAqUG3Pi8>)
- [Jump Consistent Hashing: A Fast, Minimal Memory, Consistent Hash Algorithm](<https://arxiv.org/abs/1406.2294>)
- [Rendezvous Hashing: an alternative to Consistent Hashing](<https://medium.com/i0exception/rendezvous-hashing-8c00e2fb58b0>)
- Implementations in various languages:
  - [C](<https://github.com/chrismoos/hash-ring>)
  - [C++](<https://web.archive.org/web/20210414205133/http://www.martinbroadhurst.com/consistent-hash-ring.html>)
  - [C#](<https://code.google.com/p/consistent-hash/>)
  - [Erlang](<https://web.archive.org/web/20110711100426/https://github.com/basho/riak_core/blob/master/src/chash.erl>)
  - [Go](<https://github.com/stathat/consistent>)
  - [Java](<https://web.archive.org/web/20120605030524/http://weblogs.java.net/blog/tomwhite/archive/2007/11/consistent_hash.html>)
  - [PHP](<https://github.com/pda/flexihash>)
  - [Ruby](<https://github.com/afirel/consistent_hashr>)
  - [Python](<http://techspot.zzzeek.org/2012/07/07/the-absolutely-simplest-consistent-hashing-example/>)
  - [Python (again)](<http://michaelnielsen.org/blog/consistent-hashing/>)
  - [Perl](<https://metacpan.org/pod/Set::ConsistentHash>)
  - [Perl6](<https://github.com/bradclawsie/Hash-Consistent>)

\

Retrieved from "[https://en.wikipedia.org/w/index.php?title=Consistent\_hashing&oldid=1375814388](<https://en.wikipedia.org/w/index.php?title=Consistent_hashing&oldid=1375814388>)"

[Category](<https://en.wikipedia.org/wiki/Help:Category>):

- [Hashing](<https://en.wikipedia.org/wiki/Category:Hashing>)

Hidden categories:

- [Articles with short description](<https://en.wikipedia.org/wiki/Category:Articles_with_short_description>)
- [Short description matches Wikidata](<https://en.wikipedia.org/wiki/Category:Short_description_matches_Wikidata>)
- [Wikipedia articles that are too technical from October 2024](<https://en.wikipedia.org/wiki/Category:Wikipedia_articles_that_are_too_technical_from_October_2024>)
- [All articles that are too technical](<https://en.wikipedia.org/wiki/Category:All_articles_that_are_too_technical>)
- [All articles with unsourced statements](<https://en.wikipedia.org/wiki/Category:All_articles_with_unsourced_statements>)
- [Articles with unsourced statements from September 2024](<https://en.wikipedia.org/wiki/Category:Articles_with_unsourced_statements_from_September_2024>)
- [Articles with unsourced statements from April 2021](<https://en.wikipedia.org/wiki/Category:Articles_with_unsourced_statements_from_April_2021>)
- [Articles with unsourced statements from October 2019](<https://en.wikipedia.org/wiki/Category:Articles_with_unsourced_statements_from_October_2019>)


<!-- sllmwiki:original-source -->
## Original source

- URL: <https://en.wikipedia.org/wiki/Consistent_hashing>
- License: [CC-BY-SA-4.0](<https://creativecommons.org/licenses/by-sa/4.0/>)
