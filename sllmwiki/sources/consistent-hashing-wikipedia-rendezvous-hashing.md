# Rendezvous hashing - Wikipedia

From Wikipedia, the free encyclopedia

[[image: ](<https://thumb.wikimedia.org/wikipedia/en/thumb/d/d1/Rendezvous_Hash_Schematic.png/250px-Rendezvous_Hash_Schematic.png?utm_source=en.wikipedia.org&utm_campaign=parser&utm_content=thumbnail>)](<https://en.wikipedia.org/wiki/File:Rendezvous_Hash_Schematic.png>)

Rendezvous Hashing with *n=12, k=4.* Clients *C1* and *C4* independently pick the same random subset of four sites *{S2, S5, S6, S10}* from among the twelve options *S1, S2, ..., S12,* for placing replicas or shares of object *O.*

**Rendezvous**or **highest random weight (HRW) hashing**\[1\]\[2\] is an algorithm that allows clients to achieve distributed agreement on a set of $k$ options out of a possible set of $n$ options. A typical application is when clients need to agree on which sites (or proxies) objects are assigned to.

[Consistent hashing](<https://en.wikipedia.org/wiki/Consistent_hashing>) addresses the special case $k=1$ using a different method. Rendezvous hashing is both much simpler and more general than consistent hashing (see below).

## History

\[[edit](<https://en.wikipedia.org/w/index.php?title=Rendezvous_hashing&action=edit&section=1>)\]

Rendezvous hashing was invented by David Thaler and Chinya Ravishankar at the [University of Michigan](<https://en.wikipedia.org/wiki/University_of_Michigan>) in 1996.\[1\] [Consistent hashing](<https://en.wikipedia.org/wiki/Consistent_hashing>) appeared a year later in the literature.

Given its simplicity and generality, rendezvous hashing is now being preferred to consistent hashing in real-world applications.\[3\]\[4\]\[5\] Rendezvous hashing was used very early on in many applications including mobile caching,\[6\] router design,\[7\] secure [key establishment](<https://en.wikipedia.org/wiki/Key_establishment>),\[8\] and [sharding](<https://en.wikipedia.org/wiki/Shard_(database_architecture)>) and [distributed databases](<https://en.wikipedia.org/wiki/Distributed_database>).\[9\] Other examples of real-world systems that use Rendezvous Hashing include the GitHub load balancer,\[10\] the [Apache Ignite](<https://en.wikipedia.org/wiki/Apache_Ignite>) distributed database,\[11\] the Tahoe-LAFS file store,\[12\] the CoBlitz large-file distribution service,\[13\] Apache Druid,\[14\] IBM's Cloud Object Store,\[15\] the Arvados Data Management System,\[16\] Apache Kafka,\[17\] and the Twitter EventBus pub/sub platform.\[18\]

One of the first applications of rendezvous hashing was to enable [multicast](<https://en.wikipedia.org/wiki/Multicast>) clients on the Internet (in contexts such as the [MBONE](<https://en.wikipedia.org/wiki/MBONE>)) to identify multicast rendezvous points in a distributed fashion.\[19\]\[20\] It was used in 1998 by Microsoft's [Cache Array Routing Protocol](<https://en.wikipedia.org/wiki/Cache_Array_Routing_Protocol>) (CARP) for distributed cache coordination and routing.\[21\]\[22\] Some [Protocol Independent Multicast](<https://en.wikipedia.org/wiki/Protocol_Independent_Multicast>) routing protocols use rendezvous hashing to pick a rendezvous point.\[1\]

## Problem definition and approach

\[[edit](<https://en.wikipedia.org/w/index.php?title=Rendezvous_hashing&action=edit&section=2>)\]

### Algorithm

\[[edit](<https://en.wikipedia.org/w/index.php?title=Rendezvous_hashing&action=edit&section=3>)\]

Rendezvous hashing solves a general version of the [distributed hash table](<https://en.wikipedia.org/wiki/Distributed_hash_table>) problem: We are given a set of $n$ sites (servers or proxies, say). How can any set of clients, given an object $O$, agree on a *k-*subset of sites to assign to $O$? The standard version of the problem uses *k = 1.* Each client is to make its selection independently, but all clients must end up picking the same subset of sites. This is non-trivial if we add a *minimal disruption* constraint, and require that when a site fails or is removed, only objects mapping to that site need be reassigned to other sites.

The basic idea is to give each site $S_{j}$ a score (a *weight*) for each object $O_{i}$, and assign the object to the highest scoring site. All clients first agree on a hash function $h(\cdot )$. For object $O_{i}$, the site $S_{j}$ is defined to have weight $w_{i,j}=h(O_{i},S_{j})$. Each client independently computes these weights $w_{i,1},w_{i,2}\dots w_{i,n}$ and picks the *k* sites that yield the *k* largest hash values. The clients have thereby achieved distributed $k$-agreement.

If a site $S$ is added or removed, only the objects mapping to $S$ are remapped to different sites, satisfying the minimal disruption constraint above. The HRW assignment can be computed independently by any client, since it depends only on the identifiers for the set of sites $S_{1},S_{2}\dots S_{n}$ and the object being assigned.

HRW easily accommodates different capacities among sites. If site $S_{k}$ has twice the capacity of the other sites, we simply represent $S_{k}$ twice in the list, say, as $S_{k,1},S_{k,2}$. Clearly, twice as many objects will now map to $S_{k}$ as to the other sites.

### Properties

\[[edit](<https://en.wikipedia.org/w/index.php?title=Rendezvous_hashing&action=edit&section=4>)\]

Consider the simple version of the problem, with *k = 1,* where all clients are to agree on a single site for an object *O.* Approaching the problem naively, it might appear sufficient to treat the *n* sites as buckets in a [hash table](<https://en.wikipedia.org/wiki/Hash_table>) and hash the object name *O* into this table. Unfortunately, if any of the sites fails or is unreachable, the hash table size changes, forcing all objects to be remapped. This massive disruption makes such direct hashing unworkable.

Under rendezvous hashing, however, clients handle site failures by picking the site that yields the next largest weight. Remapping is required only for objects currently mapped to the failed site, and disruption is minimal.\[1\]\[2\]

Rendezvous hashing has the following properties:

1. **Low overhead:** The hash function used is efficient, so overhead at the clients is very low.
2. **[Load balancing](<https://en.wikipedia.org/wiki/Load_balancing_(computing)>):** Since the hash function is randomizing, each of the *n*sites is equally likely to receive the object *O*. Loads are uniform across the sites.
   1. **Site capacity:** Sites with different capacities can be represented in the site list with multiplicity in proportion to capacity. A site with twice the capacity of the other sites will be represented twice in the list, while every other site is represented once.
3. **High [hit rate](<https://en.wikipedia.org/wiki/Hit_rate>):** Since all clients agree on placing an object *O* into the same site *SO*, each fetch or placement of *O* into *SO* yields the maximum utility in terms of hit rate. The object *O* will always be found unless it is evicted by some replacement algorithm at *SO*.
4. **Minimal disruption:** When a site fails, only the objects mapped to that site need to be remapped. Disruption is at the minimal possible level.\[1\]\[2\]
5. **Distributed *k*-agreement:** Clients can reach distributed agreement on *k* sites simply by selecting the top *k* sites in the ordering.\[8\]

## *O*(log *n*) running time via skeleton-based hierarchical rendezvous hashing

\[[edit](<https://en.wikipedia.org/w/index.php?title=Rendezvous_hashing&action=edit&section=5>)\]

The standard version of Rendezvous Hashing described above works quite well for moderate *n,* but when $n$ is extremely large, the hierarchical use of Rendezvous Hashing achieves $O(\log n)$ running time.\[23\]\[24\]\[25\] This approach creates a virtual hierarchical structure (called a "skeleton"), and achieves $O(\log n)$ running time by applying HRW at each level while descending the hierarchy. The idea is to first choose some constant $m$ and organize the $n$ sites into $c=\lceil n/m\rceil$ clusters $C_{1}=\left\{S_{1},S_{2}\dots S_{m}\right\},C_{2}=\left\{S_{m+1},S_{m+2}\dots S_{2m}\right\}\dots$ Next, build a virtual hierarchy by choosing a constant $f$ and imagining these $c$ clusters placed at the leaves of a tree $T$ of virtual nodes, each with fanout $f$.

[[image: ](<https://thumb.wikimedia.org/wikipedia/commons/thumb/b/b1/Rendezvous_Hashing_skeleton%2C_animated.gif/250px-Rendezvous_Hashing_skeleton%2C_animated.gif?utm_source=en.wikipedia.org&utm_campaign=parser&utm_content=thumbnail>)](<https://en.wikipedia.org/wiki/File:Rendezvous_Hashing_skeleton,_animated.gif>)

Using a skeleton to achieve *log(n)* execution time. The real nodes appear as squares at the leaf level. The virtual nodes of the skeleton appear as dotted circles. The leaf-level clusters are of size *m = 4,* and the skeleton has fanout *f = 3.*

In the accompanying diagram, the cluster size is $m=4$, and the skeleton fanout is $f=3$. Assuming 108 sites (real nodes) for convenience, we get a three-tier virtual hierarchy. Since $f=3$, each virtual node has a natural numbering in octal. Thus, the 27 virtual nodes at the lowest tier would be numbered $000,001,002,...,221,222$ in octal (we can, of course, vary the fanout at each level - in that case, each node will be identified with the corresponding mixed-radix number).

The easiest way to understand the virtual hierarchy is by starting at the top, and descending the virtual hierarchy. We successively apply Rendezvous Hashing to the set of virtual nodes at each level of the hierarchy, and descend the branch defined by the winning virtual node. We can in fact start at any level in the virtual hierarchy. Starting lower in the hierarchy requires more hashes, but may improve load distribution in the case of failures.

For example, instead of applying HRW to all 108 real nodes in the diagram, we can first apply HRW to the 27 lowest-tier virtual nodes, selecting one. We then apply HRW to the four real nodes in its cluster, and choose the winning site. We only need $27+4=31$ hashes, rather than 108. If we apply this method starting one level higher in the hierarchy, we would need $9+3+4=16$ hashes to get to the winning site. The figure shows how, if we proceed starting from the root of the skeleton, we may successively choose the virtual nodes $(2)_{3}$, $(20)_{3}$, and $(200)_{3}$, and finally end up with site 74.

The virtual hierarchy need not be stored, but can be created on demand, since the virtual nodes names are simply prefixes of base-$f$ (or mixed-radix) representations. We can easily create appropriately sorted strings from the digits, as required. In the example, we would be working with the strings $0,1,2$ (at tier 1), $20,21,22$ (at tier 2), and $200,201,202$ (at tier 3). Clearly, $T$ has height $h=O(\log c)=O(\log n)$, since $m$ and $f$ are both constants. The work done at each level is $O(1)$, since $f$ is a constant.

The value of $m$ can be chosen based on factors like the anticipated failure rate and the degree of desired load balancing. A higher value of $m$ leads to less load skew in the event of failure at the cost of higher search overhead.

The choice $m=n$ is equivalent to non-hierarchical rendezvous hashing. In practice, the hash function $h(\cdot )$ is very cheap, so $m=n$ can work quite well unless $n$ is very high.

For any given object, it is clear that each leaf-level cluster, and hence each of the $n$ sites, is chosen with equal probability.

### Replication, site failures, and site additions

\[[edit](<https://en.wikipedia.org/w/index.php?title=Rendezvous_hashing&action=edit&section=6>)\]

One can enhance resiliency to failures by replicating each object *O* across the highest ranking *r \< m* sites for *O,* choosing *r* based on the level of resiliency desired. The simplest strategy is to replicate only within the leaf-level cluster.

If the leaf-level site selected for *O* is unavailable, we select the next-ranked site for *O* within the same leaf-level cluster. If *O* has been replicated within the leaf-level cluster, we are sure to find *O* in the next available site in the ranked order of *r* sites. All objects that were held by the failed server appear in some other site in its cluster. (Another option is to go up one or more tiers in the skeleton and select an alternate from among the sibling virtual nodes at that tier. We then descend the hierarchy to the real nodes, as above.)

When a site is added to the system, it may become the winning site for some objects already assigned to other sites. Objects mapped to other clusters will never map to this new site, so we need to only consider objects held by other sites in its cluster. If the sites are caches, attempting to access an object mapped to the new site will result in a cache miss, the corresponding object will be fetched and cached, and operation returns to normal.

If sites are servers, some objects must be remapped to this newly added site. As before, objects mapped to other clusters will never map to this new site, so we need to only consider objects held by sites in its cluster. That is, we need only remap objects currently present in the *m* sites in this local cluster, rather than the entire set of objects in the system. New objects mapping to this site will of course be automatically assigned to it.

## Comparison with consistent hashing

\[[edit](<https://en.wikipedia.org/w/index.php?title=Rendezvous_hashing&action=edit&section=7>)\]

Because of its simplicity, lower overhead, and generality (it works for any *k \< n*), rendezvous hashing is increasingly being preferred over consistent hashing. Recent examples of its use include the GitHub load balancer,\[10\] the Apache Ignite distributed database,\[11\] and by the Twitter EventBus pub/sub platform.\[18\]

Consistent hashing operates by mapping each site uniformly and randomly to multiple points on a unit circle called tokens. Objects are also mapped to the unit circle and placed in the site owning the token that is first encountered traveling clockwise from the object's location. When a site is removed, each of its objects is transferred to the site owning the next token encountered moving clockwise. Provided each site is mapped to a large number of tokens (100–200, say), this will reassign objects in a relatively uniform fashion among the remaining sites.

If a site's tokens are placed on the unit circle by hashing 200 variants of the site ID, say, then the assignment of any object requires storing or recalculating 200 hash values for each site. However, the tokens associated with a given site can be precomputed and stored in a sorted list, requiring only a single application of the hash function to the object, and a binary search to compute the assignment. Even with many tokens per site, however, the basic version of consistent hashing may not balance objects uniformly over sites, since when a site is removed each object assigned to it is distributed only over as many other sites as the site has tokens (say 100–200).

Variants of consistent hashing (such as Amazon's [Dynamo](<https://en.wikipedia.org/wiki/Dynamo_(storage_system)>)) that use more complex logic to distribute tokens on the unit circle offer better [load balancing](<https://en.wikipedia.org/wiki/Load_balancing_(computing)>) than basic consistent hashing, reduce the overhead of adding new sites, and reduce metadata overhead and offer other benefits.\[26\]

### Advantages of Rendezvous hashing over consistent hashing

\[[edit](<https://en.wikipedia.org/w/index.php?title=Rendezvous_hashing&action=edit&section=8>)\]

Rendezvous hashing (HRW) is much simpler conceptually and in practice. It also distributes objects uniformly over all sites, given a uniform hash function. Unlike consistent hashing, HRW requires no precomputing or storage of tokens. Consider *k =1.* An object $O_{i}$ is placed into one of $n$ sites $S_{1},S_{2}\dots S_{n}$ by computing the $n$ hash values $h(O_{i},S_{j})$ and picking the site $S_{k}$ that yields the highest hash value. If a new site $S_{n+1}$ is added, new object placements or requests will compute $n+1$ hash values, and pick the largest of these. If an object already in the system at $S_{k}$ maps to this new site $S_{n+1}$, it will be fetched afresh and cached at $S_{n+1}$. All clients will henceforth obtain it from this site, and the old cached copy at $S_{k}$ will ultimately be replaced by the local cache management algorithm. If $S_{k}$ is taken offline, its objects will be remapped uniformly to the remaining $n-1$ sites.

Variants of the HRW algorithm, such as the use of a skeleton (see below), can reduce the $O(n)$ time for object location to $O(\log n)$, at the cost of less global uniformity of placement. When $n$ is not too large, however, the $O(n)$ placement cost of basic HRW is not likely to be a problem. HRW completely avoids all the overhead and complexity associated with correctly handling multiple tokens for each site and associated metadata.

Rendezvous hashing also has the great advantage that it provides simple solutions to other important problems, such as distributed $k$-agreement.

### Consistent hashing is a special case of Rendezvous hashing

\[[edit](<https://en.wikipedia.org/w/index.php?title=Rendezvous_hashing&action=edit&section=9>)\]

Rendezvous hashing is both simpler and more general than consistent hashing. Consistent hashing can be shown to be a special case of HRW by an appropriate choice of a two-place hash function. From the site identifier $S$ the simplest version of consistent hashing computes a list of token positions, e.g., $t_{s}=h_{c}(S)$ where $h_{c}$ hashes values to locations on the unit circle. Define the two place hash function $h(S,O)$ to be ${\frac {1}{\min _{S}(h_{c}(O)-t_{s})}}$ where $h_{c}(O)-t_{s}$ denotes the distance along the unit circle from $h_{c}(O)$ to $t_{s}$ (since $h_{c}(O)-t_{s}$ has some minimal non-zero value there is no problem translating this value to a unique integer in some bounded range). This will duplicate exactly the assignment produced by consistent hashing.

It is not possible, however, to [reduce](<https://en.wikipedia.org/wiki/Reduction_(complexity)>) HRW to consistent hashing (assuming the number of tokens per site is bounded), since HRW potentially reassigns the objects from a removed site to an unbounded number of other sites.

## Weighted variations

\[[edit](<https://en.wikipedia.org/w/index.php?title=Rendezvous_hashing&action=edit&section=10>)\]

In the standard implementation of rendezvous hashing, every node receives a statically equal proportion of the keys. This behavior, however, is undesirable when the nodes have different capacities for processing or holding their assigned keys. For example, if one of the nodes had twice the storage capacity as the others, it would be beneficial if the algorithm could take this into account such that this more powerful node would receive twice the number of keys as each of the others.

A straightforward mechanism to handle this case is to assign two virtual locations to this node, so that if either of that larger node's virtual locations has the highest hash, that node receives the key. But this strategy does not work when the relative weights are not integer multiples. For example, if one node had 42% more storage capacity, it would require adding many virtual nodes in different proportions, leading to greatly reduced performance. Several modifications to rendezvous hashing have been proposed to overcome this limitation.

### Cache Array Routing Protocol

\[[edit](<https://en.wikipedia.org/w/index.php?title=Rendezvous_hashing&action=edit&section=11>)\]

The [Cache Array Routing Protocol](<https://en.wikipedia.org/wiki/Cache_Array_Routing_Protocol>) (CARP) is a 1998 IETF draft that describes a method for computing *load factors* which can be multiplied by each node's hash score to yield an arbitrary level of precision for weighting nodes differently.\[21\] However, one disadvantage of this approach is that when any node's weight is changed, or when any node is added or removed, all the load factors must be re-computed and relatively scaled. When the load factors change relative to one another, it triggers movement of keys between nodes whose weight was not changed, but whose load factor did change relative to other nodes in the system. This results in excess movement of keys.\[27\]

### Controlled replication

\[[edit](<https://en.wikipedia.org/w/index.php?title=Rendezvous_hashing&action=edit&section=12>)\]

Controlled replication under scalable hashing or CRUSH\[28\] is an extension to RUSH\[29\] that improves upon rendezvous hashing by constructing a tree where a pseudo-random function (hash) is used to navigate down the tree to find which node is ultimately responsible for a given key. It permits perfect stability for adding nodes; however, it is not perfectly stable when removing or re-weighting nodes, with the excess movement of keys being proportional to the height of the tree.

The CRUSH algorithm is used by the [ceph](<https://en.wikipedia.org/wiki/Ceph_(software)>) data storage system to map data objects to the nodes responsible for storing them.\[30\]

### Other variants

\[[edit](<https://en.wikipedia.org/w/index.php?title=Rendezvous_hashing&action=edit&section=13>)\]

In 2005, Christian Schindelhauer and Gunnar Schomaker described a logarithmic method for re-weighting hash scores in a way that does not require relative scaling of load factors when a node's weight changes or when nodes are added or removed.\[31\] This enabled the dual benefits of perfect precision when weighting nodes, along with perfect stability, as only a minimum number of keys needed to be remapped to new nodes.

A similar logarithm-based hashing strategy is used to assign data to storage nodes in [Cleversafe](<https://en.wikipedia.org/wiki/Cleversafe_Inc.>)'s data storage system, now [IBM Cloud Object Storage](<https://en.wikipedia.org/wiki/IBM_Cloud_Object_Storage>).\[27\]

## Systems using Rendezvous hashing

\[[edit](<https://en.wikipedia.org/w/index.php?title=Rendezvous_hashing&action=edit&section=14>)\]

Rendezvous hashing is being used widely in real-world systems. A partial list includes Oracle's Database in-memory,\[9\] the GitHub load balancer,\[10\] the Apache Ignite distributed database,\[11\] the Tahoe-LAFS file store,\[12\] the CoBlitz large-file distribution service,\[13\] Apache Druid,\[14\] IBM's Cloud Object Store,\[15\] the Arvados Data Management System,\[16\] Apache Kafka,\[17\] and by the Twitter EventBus pub/sub platform.\[18\]

## Implementation

\[[edit](<https://en.wikipedia.org/w/index.php?title=Rendezvous_hashing&action=edit&section=15>)\]

Implementation is straightforward once a [hash function](<https://en.wikipedia.org/wiki/Hash_function>) $h(\cdot )$ is chosen (the original work on the HRW method makes a hash function recommendation).\[1\]\[2\] Each client only needs to compute a hash value for each of the $n$ sites, and then pick the largest. This algorithm runs in $O(n)$ time. If the hash function is efficient, the $O(n)$ running time is not a problem unless $n$ is very large. A logarithmic transformation of the uniform score (unit interval) is applied, when multiplied by the node weights the probability of a node winning is proportional to its weight relative to the total weight of all nodes. Note the magnitude of the scores vary greatly but the probabilities remain constant.

### Weighted rendezvous hash

\[[edit](<https://en.wikipedia.org/w/index.php?title=Rendezvous_hashing&action=edit&section=16>)\]

Python code implementing a weighted rendezvous hash:\[27\]

```
import mmh3
import math
from dataclasses import dataclass

def hash_to_unit_interval(s: str) -> float:
    """Hashes a string onto the unit interval (0, 1]"""
    return (mmh3.hash128(s) + 1) / 2**128

@dataclass
class Node:
    """Class representing a node that is assigned keys as part of a weighted rendezvous hash."""
    name: str
    weight: float

    def compute_weighted_score(self, key: str) -> float:
        score: float = hash_to_unit_interval(f"{self.name}: {key}")
        log_score: float = 1.0 / -math.log(score)
        return self.weight * log_score

def determine_responsible_node(nodes: list[Node], key: str) -> Node:
    """Determines which node of a set of nodes of various weights is responsible for the provided key."""
    return max(nodes, key=lambda node: node.compute_weighted_score(key), default=None)
```

Example outputs of WRH:

```
import wrh
from collections import Counter
from wrh import Node

node1: Node = Node("node1", 100)
node2: Node = Node("node2", 200)
node3: Node = Node("node3", 300)
print(str(wrh.determine_responsible_node([node1, node2, node3], "foo")))
# prints: "Node(name='node1', weight=100)"
print(str(wrh.determine_responsible_node([node1, node2, node3], "bar")))
# prints: "Node(name='node2', weight=300)"
print(str(wrh.determine_responsible_node([node1, node2, node3], "hello")))
# prints: "Node(name='node2', weight=300)"
nodes: list[Node] = [node1, node2, node3]
responsible_nodes: list[Node] = [wrh.determine_responsible_node(
    nodes, f"key: {key}").name for key in range(45_000)]
print(Counter(responsible_nodes))
# prints: Counter({'node3': 22487, 'node2': 15020, 'node1': 7493})
```

## References

\[[edit](<https://en.wikipedia.org/w/index.php?title=Rendezvous_hashing&action=edit&section=17>)\]

1. 1 2 3 4 5 6 *Thaler, David; Chinya Ravishankar. ["A Name-Based Mapping Scheme for Rendezvous"](<http://www.eecs.umich.edu/techreports/cse/96/CSE-TR-316-96.pdf>) (PDF). University of Michigan Technical Report CSE-TR-316-96. Retrieved 2013-09-15.*
2. 1 2 3 4 *Thaler, David; Chinya Ravishankar (February 1998). "Using Name-Based Mapping Schemes to Increase Hit Rates". *IEEE/ACM Transactions on Networking*. **6** (1): 1–14. [doi](<https://en.wikipedia.org/wiki/Doi_(identifier)>):[10.1109/90.663936](<https://doi.org/10.1109%2F90.663936>). [S2CID](<https://en.wikipedia.org/wiki/S2CID_(identifier)>) [936134](<https://api.semanticscholar.org/CorpusID:936134>).*
3. ↑ *["Rendezvous Hashing Explained - Randorithms"](<https://randorithms.com/2020/12/26/rendezvous-hashing.html>). *randorithms.com*. Retrieved 2021-03-29.*
4. ↑ *["Rendezvous hashing: my baseline "consistent" distribution method - Paul Khuong: some Lisp"](<https://pvk.ca/Blog/2017/09/24/rendezvous-hashing-my-baseline-consistent-distribution-method/>). *pvk.ca*. Retrieved 2021-03-29.*
5. ↑ *Aniruddha (2020-01-08). ["Rendezvous Hashing"](<https://medium.com/i0exception/rendezvous-hashing-8c00e2fb58b0>). *Medium*. Retrieved 2021-03-29.*
6. ↑ *Mayank, Anup; Ravishankar, Chinya (2006). ["Supporting mobile device communications in the presence of broadcast servers"](<http://www.cs.ucr.edu/~ravi/Papers/Jrnl/Mayank.pdf>) (PDF). *International Journal of Sensor Networks*. **2** (1/2): 9–16. [doi](<https://en.wikipedia.org/wiki/Doi_(identifier)>):[10.1504/IJSNET.2007.012977](<https://doi.org/10.1504%2FIJSNET.2007.012977>).*
7. ↑ *Guo, Danhua; Bhuyan, Laxmi; Liu, Bin (October 2012). "An efficient parallelized L7-filter design for multicore servers". *IEEE/ACM Transactions on Networking*. **20** (5): 1426–1439. [doi](<https://en.wikipedia.org/wiki/Doi_(identifier)>):[10.1109/TNET.2011.2177858](<https://doi.org/10.1109%2FTNET.2011.2177858>). [S2CID](<https://en.wikipedia.org/wiki/S2CID_(identifier)>) [1982193](<https://api.semanticscholar.org/CorpusID:1982193>).*
8. 1 2 *Wang, Peng; Ravishankar, Chinya (2015). ["Key Foisting and Key Stealing Attacks in Sensor Networks'"](<http://www.cs.ucr.edu/~ravi/Papers/Jrnl/foisting.pdf>) (PDF). *International Journal of Sensor Networks*.*
9. 1 2 *Mukherjee, Niloy; et al. (August 2015). "Distributed Architecture of Oracle Database In-memory". *Proceedings of the VLDB Endowment*. **8** (12): 1630–1641. [doi](<https://en.wikipedia.org/wiki/Doi_(identifier)>):[10.14778/2824032.2824061](<https://doi.org/10.14778%2F2824032.2824061>).*
10. 1 2 3 *GitHub Engineering (22 September 2016). ["Introducing the GitHub Load Balancer"](<https://github.blog/2016-09-22-introducing-glb/>). *GitHub Blog*. Retrieved 1 February 2022.*
11. 1 2 3 *["Apache Ignite"](<https://en.wikipedia.org/w/index.php?title=Apache_Ignite&oldid=1105175435>), *Wikipedia*, 2022-08-18, retrieved 2022-12-09*
12. 1 2 *["Tahoe-LAFS"](<https://tahoe-lafs.org/trac/tahoe-lafs>). *tahoe-lafs.org*. Retrieved 2023-01-02.*
13. 1 2 *Park, KyoungSoo; Pai, Vivek S. (2006). "Scale and performance in the CoBlitz large-file distribution service". *Usenix Nsdi*.*
14. 1 2 *["Router Process · Apache Druid"](<https://druid.apache.org/index.html>). *druid.apache.org*. Retrieved 2023-01-02.*
15. 1 2 *["IBM Cloud Object Storage System, Version 3.14.11, Storage Pool Expansion Guide"](<https://www.ibm.com/docs/STXNRM_3.15.3/coss.doc/pdfs/storagePoolExpansion_bookmap.pdf>) (PDF). *IBM Cloud Object Storage SystemTM Version*. Retrieved January 2, 2023.*
16. 1 2 *["Arvados | Keep clients"](<https://doc.arvados.org/v2.5/architecture/keep-clients.html>). *doc.arvados.org*. Retrieved 2023-01-02.*
17. 1 2 *["Horizontally scaling Kafka consumers with rendezvous hashing"](<https://www.tinybird.co/blog-posts/kafka-horizontal-scaling>). *Tinybird.co*. Retrieved 2023-02-15.*
18. 1 2 3 *Aniruddha (2020-01-08). ["Rendezvous Hashing"](<https://medium.com/i0exception/rendezvous-hashing-8c00e2fb58b0>). *i0exception*. Retrieved 2022-12-09.*
19. ↑ *Blazevic, Ljubica (21 June 2000). ["Distributed Core Multicast (DCM): a routing protocol for many small groups with application to mobile IP telephony"](<http://tools.ietf.org/html/draft-blazevic-dcm-mobility-01>). *IETF Draft*. IETF. Retrieved September 17, 2013.*
20. ↑ *Fenner, B. (August 2006). ["Protocol Independent Multicast - Sparse Mode (PIM-SM): Protocol Specification (Revised)"](<http://tools.ietf.org/html/rfc4601>). *IETF RFC*. IETF. Retrieved September 17, 2013.*
21. 1 2 *Valloppillil, Vinod; Kenneth Ross (27 February 1998). ["Cache Array Routing Protocol v1.0"](<http://tools.ietf.org/html/draft-vinod-carp-v1-03>). Internet Draft. Retrieved September 15, 2013.*
22. ↑ *["Cache Array Routing Protocol and Microsoft Proxy Server 2.0"](<https://web.archive.org/web/20140918115447/http://oldsite.mcoecn.org/WhitePapers/Mscarp.pdf>) (PDF). Microsoft. Archived from [the original](<http://oldsite.mcoecn.org/WhitePapers/Mscarp.pdf>) (PDF) on September 18, 2014. Retrieved September 15, 2013.*
23. ↑ *Yao, Zizhen; Ravishankar, Chinya; Tripathi, Satish (May 13, 2001). [*Hash-Based Virtual Hierarchies for Caching in Hybrid Content-Delivery Networks*](<https://web.archive.org/web/20201116215127/http://static.cs.ucr.edu/store/techreports/UCR-CS-2001-05062.pdf>) (PDF). Riverside, CA: CSE Department, University of California, Riverside. Archived from [the original](<http://static.cs.ucr.edu/store/techreports/UCR-CS-2001-05062.pdf>) (PDF) on 16 November 2020. Retrieved 15 November 2015.*
24. ↑ *Wang, Wei; Chinya Ravishankar (January 2009). "Hash-Based Virtual Hierarchies for Scalable Location Service in Mobile Ad-hoc Networks". *Mobile Networks and Applications*. **14** (5): 625–637. [doi](<https://en.wikipedia.org/wiki/Doi_(identifier)>):[10.1007/s11036-008-0144-3](<https://doi.org/10.1007%2Fs11036-008-0144-3>). [S2CID](<https://en.wikipedia.org/wiki/S2CID_(identifier)>) [2802543](<https://api.semanticscholar.org/CorpusID:2802543>).*
25. ↑ *Mayank, Anup; Phatak, Trivikram; Ravishankar, Chinya (2006), [*Decentralized Hash-Based Coordination of Distributed Multimedia Caches*](<http://www.cs.ucr.edu/~ravi/Papers/NWConf/ICN2006.pdf>) (PDF), Proc. 5th IEEE International Conference on Networking (ICN'06), Mauritius: IEEE*
26. ↑ *DeCandia, G.; Hastorun, D.; Jampani, M.; Kakulapati, G.; Lakshman, A.; Pilchin, A.; Sivasubramanian, S.; Vosshall, P.; Vogels, W. (2007). ["Dynamo"](<http://www.allthingsdistributed.com/files/amazon-dynamo-sosp2007.pdf>) (PDF). *ACM SIGOPS Operating Systems Review*. **41** (6): 205–220. [doi](<https://en.wikipedia.org/wiki/Doi_(identifier)>):[10.1145/1323293.1294281](<https://doi.org/10.1145%2F1323293.1294281>). Retrieved 2018-06-07.*
27. 1 2 3 *Jason Resch. ["New Hashing Algorithms for Data Storage"](<http://www.snia.org/sites/default/files/SDC15_presentations/dist_sys/Jason_Resch_New_Consistent_Hashings_Rev.pdf>) (PDF).*
28. ↑ *Sage A. Weil; et al. ["CRUSH: Controlled, Scalable, Decentralized Placement of Replicated Data"](<https://web.archive.org/web/20190220002858/https://www.crss.ucsc.edu/media/papers/weil-sc06.pdf>) (PDF). Archived from [the original](<http://www.crss.ucsc.edu/media/papers/weil-sc06.pdf>) (PDF) on February 20, 2019.*
29. ↑ *R. J. Honicky, Ethan L. Miller. ["Replication Under Scalable Hashing: A Family of Algorithms for Scalable Decentralized Data Distribution"](<https://web.archive.org/web/20170226050158/http://www.ssrc.ucsc.edu/media/papers/honicky-ipdps04.pdf>) (PDF). Archived from [the original](<http://www.ssrc.ucsc.edu/media/papers/honicky-ipdps04.pdf>) (PDF) on 2017-02-26. Retrieved 2017-02-25.*
30. ↑ *Ceph. ["Crush Maps"](<http://docs.ceph.com/docs/master/rados/operations/crush-map/>).*
31. ↑ *Christian Schindelhauer, Gunnar Schomaker (2005). "Weighted Distributed Hash Tables": 218. [CiteSeerX](<https://en.wikipedia.org/wiki/CiteSeerX_(identifier)>) [10.1.1.414.9353](<https://citeseerx.ist.psu.edu/viewdoc/summary?doi=10.1.1.414.9353>).* `{{cite journal}}`: Cite journal requires `|journal=` ([help](<https://en.wikipedia.org/wiki/Help:CS1_errors#missing_periodical>)); Cite uses deprecated parameter `|citeseerx=` ([help](<https://en.wikipedia.org/wiki/Help:CS1_errors#deprecated_params>))

## External links

\[[edit](<https://en.wikipedia.org/w/index.php?title=Rendezvous_hashing&action=edit&section=18>)\]

- [Rendezvous Hashing: an alternative to Consistent Hashing](<https://medium.com/i0exception/rendezvous-hashing-8c00e2fb58b0>)

Retrieved from "[https://en.wikipedia.org/w/index.php?title=Rendezvous\_hashing&oldid=1375060219](<https://en.wikipedia.org/w/index.php?title=Rendezvous_hashing&oldid=1375060219>)"

[Categories](<https://en.wikipedia.org/wiki/Help:Category>):

- [Algorithms](<https://en.wikipedia.org/wiki/Category:Algorithms>)
- [Hashing](<https://en.wikipedia.org/wiki/Category:Hashing>)

Hidden categories:

- [Articles with short description](<https://en.wikipedia.org/wiki/Category:Articles_with_short_description>)
- [Short description matches Wikidata](<https://en.wikipedia.org/wiki/Category:Short_description_matches_Wikidata>)
- [CS1 errors: deprecated parameters](<https://en.wikipedia.org/wiki/Category:CS1_errors:_deprecated_parameters>)
- [CS1 errors: missing periodical](<https://en.wikipedia.org/wiki/Category:CS1_errors:_missing_periodical>)
- [Articles with example Python (programming language) code](<https://en.wikipedia.org/wiki/Category:Articles_with_example_Python_(programming_language)_code>)


<!-- sllmwiki:original-source -->
## Original source

- URL: <https://en.wikipedia.org/wiki/Rendezvous_hashing>
- License: [CC-BY-SA-4.0](<https://creativecommons.org/licenses/by-sa/4.0/>)
