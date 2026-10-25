# CAP theorem - Wikipedia

From Wikipedia, the free encyclopedia

In [database theory](<https://en.wikipedia.org/wiki/Database_theory>), the **CAP theorem**, also named **Brewer's theorem** after computer scientist [Eric Brewer](<https://en.wikipedia.org/wiki/Eric_Brewer_(scientist)>), states that any [distributed data store](<https://en.wikipedia.org/wiki/Distributed_data_store>) can provide at most [two of the following three](<https://en.wikipedia.org/wiki/Inconsistent_triad>) guarantees:\[1\]\[2\]\[3\]

**[Consistency](<https://en.wikipedia.org/wiki/Consistency_model>)**

Every read receives the most recent write or an error. Consistency means that all clients see the same data at the same time, no matter which node they connect to. For this to happen, whenever data is written to one node, it must be instantly forwarded or [replicated](<https://en.wikipedia.org/wiki/Replication_(computing)>) to all the other nodes in the system before the write is deemed ‘successful’\[4\]. Consistency as defined in the CAP theorem is quite different from the consistency guaranteed in [ACID](<https://en.wikipedia.org/wiki/ACID>) [database transactions](<https://en.wikipedia.org/wiki/Database_transaction>).\[5\]

**[Availability](<https://en.wikipedia.org/wiki/Availability>)**

Every request received by a non-failing node in the system must result in a response, without the guarantee that it contains the most recent version of the data\[6\]. This is the definition of availability in CAP theorem as defined by Gilbert and Lynch.\[1\] Availability as defined in CAP theorem is different from [high availability](<https://en.wikipedia.org/wiki/High_availability>) in software architecture.\[7\]

**[Partition tolerance](<https://en.wikipedia.org/wiki/Network_partitioning>)**

The system continues to operate despite an arbitrary number of messages being dropped (or delayed) by the network between nodes.\[8\]

When a [network partition](<https://en.wikipedia.org/wiki/Network_partition>) failure happens, it must be decided whether to do one of the following:

- cancel the operation and thus decrease the availability but ensure consistency
- proceed with the operation and thus provide availability but risk inconsistency. This does not necessarily mean that system is [highly available](<https://en.wikipedia.org/wiki/High_availability>) to its users.\[7\]

[[image: ](<https://thumb.wikimedia.org/wikipedia/commons/thumb/9/98/CAP_Theorem_Euler_Diagram.png/250px-CAP_Theorem_Euler_Diagram.png?utm_source=en.wikipedia.org&utm_campaign=parser&utm_content=thumbnail>)](<https://en.wikipedia.org/wiki/File:CAP_Theorem_Euler_Diagram.png>)

CAP theorem Euler diagram

Thus, if there is a network partition, one has to choose between consistency or availability.

During times of normal operations, a data store covers all three.\[9\]

## Explanation

\[[edit](<https://en.wikipedia.org/w/index.php?title=CAP_theorem&action=edit&section=1>)\]

No distributed system is safe from network failures, thus network partitioning generally has to be tolerated.\[10\]\[11\] In the presence of a partition, one is then left with two options: consistency or [availability](<https://en.wikipedia.org/wiki/Availability>). When choosing consistency over availability, the system will return an error or a time out if particular information cannot be guaranteed to be up to date due to network partitioning. When choosing availability over consistency, the system will always process the query and try to return the most recent available version of the information, even if it cannot guarantee it is up to date due to network partitioning.\[12\]

In the absence of a network partition, both availability and consistency can be satisfied.\[13\]

Database systems designed with traditional [ACID](<https://en.wikipedia.org/wiki/ACID>) guarantees in mind such as [RDBMS](<https://en.wikipedia.org/wiki/Relational_database_management_system>) choose [consistency](<https://en.wikipedia.org/wiki/Consistency_(database_systems)>) over availability, whereas systems designed around the [BASE](<https://en.wikipedia.org/wiki/Eventual_consistency>) philosophy, common in the [NoSQL](<https://en.wikipedia.org/wiki/NoSQL>) movement for example, choose availability over consistency,\[14\] but [MongoDB](<https://en.wikipedia.org/wiki/MongoDB>) and [Redis](<https://en.wikipedia.org/wiki/Redis>) resolve network partitions by maintaining consistency while compromising on availability.\[4\]\[9\] [CouchDB](<https://en.wikipedia.org/wiki/Apache_CouchDB>), [Cassandra](<https://en.wikipedia.org/wiki/Apache_Cassandra>), and [ScyllaDB](<https://en.wikipedia.org/wiki/ScyllaDB>) are examples of AP databases.\[9\] There are no NoSQL databases one would classify as CA.\[9\] Most modern distributed databases offer configuration options for both consistency and availability.\[6\]

## History

\[[edit](<https://en.wikipedia.org/w/index.php?title=CAP_theorem&action=edit&section=2>)\]

According to computer scientist [Eric Brewer](<https://en.wikipedia.org/wiki/Eric_Brewer_(scientist)>) of the [University of California, Berkeley](<https://en.wikipedia.org/wiki/University_of_California,_Berkeley>), the theorem first appeared in autumn 1998.\[14\] It was published as the CAP principle in 1999\[15\] and presented as a [conjecture](<https://en.wikipedia.org/wiki/Conjecture>) by Brewer at the 2000 [Symposium on Principles of Distributed Computing](<https://en.wikipedia.org/wiki/Symposium_on_Principles_of_Distributed_Computing>) (PODC).\[16\] In 2002, Seth Gilbert and [Nancy Lynch](<https://en.wikipedia.org/wiki/Nancy_Lynch>) of [MIT](<https://en.wikipedia.org/wiki/MIT>) published a formal proof of Brewer's conjecture, elevating it to a [theorem](<https://en.wikipedia.org/wiki/Theorem>).\[1\]

In 2012, Brewer clarified some of his positions, including why the often-used "two out of three" concept can be somewhat misleading because system designers only need to sacrifice consistency or availability in the presence of partitions; partition management and recovery techniques exist. Brewer also noted the different definition of consistency used in the CAP theorem relative to the definition used in [ACID](<https://en.wikipedia.org/wiki/ACID>).\[14\]\[17\]

A similar theorem stating the trade-off between consistency and availability in distributed systems had been published by Birman and Friedman in 1996.\[18\] Birman and Friedman's result had restricted this lower bound to non-commuting operations.

The [PACELC theorem](<https://en.wikipedia.org/wiki/PACELC_theorem>), introduced in 2010,\[13\] builds on CAP by stating that even in the absence of partitioning, there is another trade-off between latency and consistency. PACELC means, if partition (P) happens, the trade-off is between availability (A) and consistency (C); Else (E), the trade-off is between latency (L) and consistency (C). Some experts like Marc Brooker argue that the CAP theorem is particularly relevant in intermittently connected environments, such as those related to the [Internet of Things](<https://en.wikipedia.org/wiki/Internet_of_things>) (IoT) and [mobile applications](<https://en.wikipedia.org/wiki/Mobile_app>). In these contexts, devices may become partitioned due to challenging physical conditions, such as [power outages](<https://en.wikipedia.org/wiki/Power_outage>) or when entering confined spaces like elevators. For [distributed systems](<https://en.wikipedia.org/wiki/Distributed_computing>), such as [cloud applications](<https://en.wikipedia.org/wiki/Cloud_computing>), it is more appropriate to use the [PACELC theorem](<https://en.wikipedia.org/wiki/PACELC_theorem>), which is more comprehensive and considers trade-offs such as [latency](<https://en.wikipedia.org/wiki/Latency_(engineering)>) and [consistency](<https://en.wikipedia.org/wiki/Consistency_(database_systems)>) even in the absence of network partitions.\[19\]

## See also

\[[edit](<https://en.wikipedia.org/w/index.php?title=CAP_theorem&action=edit&section=3>)\]

- [Fallacies of distributed computing](<https://en.wikipedia.org/wiki/Fallacies_of_distributed_computing>)
- [Lambda architecture](<https://en.wikipedia.org/wiki/Lambda_architecture>) (solution)
- [PACELC theorem](<https://en.wikipedia.org/wiki/PACELC_theorem>)
- [Paxos (computer science)](<https://en.wikipedia.org/wiki/Paxos_(computer_science)>)
- [Raft (computer science)](<https://en.wikipedia.org/wiki/Raft_(computer_science)>)
- [Zooko's triangle](<https://en.wikipedia.org/wiki/Zooko's_triangle>)
- [Inconsistent triad](<https://en.wikipedia.org/wiki/Inconsistent_triad>)
- [Trilemma](<https://en.wikipedia.org/wiki/Trilemma>)

## References

\[[edit](<https://en.wikipedia.org/w/index.php?title=CAP_theorem&action=edit&section=4>)\]

1. 1 2 3 *Gilbert, Seth; Lynch, Nancy (2002). "Brewer's conjecture and the feasibility of consistent, available, partition-tolerant web services". *ACM SIGACT News*. **33** (2). Association for Computing Machinery (ACM): 51–59. [doi](<https://en.wikipedia.org/wiki/Doi_(identifier)>):[10.1145/564585.564601](<https://doi.org/10.1145%2F564585.564601>). [ISSN](<https://en.wikipedia.org/wiki/ISSN_(identifier)>) [0163-5700](<https://search.worldcat.org/issn/0163-5700>). [S2CID](<https://en.wikipedia.org/wiki/S2CID_(identifier)>) [15892169](<https://api.semanticscholar.org/CorpusID:15892169>).*
2. ↑ *["Brewer's CAP Theorem"](<https://www.julianbrowne.com/article/brewers-cap-theorem/>). *julianbrowne.com*. 2009-01-11.*
3. ↑ *Eric A. Brewer (2000). [*Towards Robust Distributed Systems*](<https://sites.cs.ucsb.edu/~rich/class/cs293b-cloud/papers/Brewer_podc_keynote_2000.pdf>) (PDF). Principles on Distributed Computing (PODC).*
4. 1 2 *["What Is the CAP Theorem? | IBM"](<https://www.ibm.com/think/topics/cap-theorem>). *www.ibm.com*. 2022-12-20. Retrieved 2025-12-05.*
5. ↑ *Liochon, Nicolas. ["The confusing CAP and ACID wording"](<http://blog.thislongrun.com/2015/03/the-confusing-cap-and-acid-wording.html>). *This long run*. Retrieved 1 February 2019.*
6. 1 2 *["Hello Interview | System Design in a Hurry"](<https://www.hellointerview.com/learn/system-design/core-concepts/cap-theorem>). *Hello Interview*. Retrieved 2025-12-07.*
7. 1 2 *Fowler, Adam (2015). *NoSQL For Dummies*. For Dummies. [ISBN](<https://en.wikipedia.org/wiki/ISBN_(identifier)>) [978-8126554904](<https://en.wikipedia.org/wiki/Special:BookSources/978-8126554904>).*
8. ↑ *Gilbert, Seth; Lynch, Nancy (2012). ["Perspectives on the CAP Theorem"](<https://ieeexplore.ieee.org/document/6122006/>). *Computer*. **45** (2): 30–36. [doi](<https://en.wikipedia.org/wiki/Doi_(identifier)>):[10.1109/mc.2011.389](<https://doi.org/10.1109%2Fmc.2011.389>). [hdl](<https://en.wikipedia.org/wiki/Hdl_(identifier)>):[1721.1/79112](<https://hdl.handle.net/1721.1%2F79112>). [ISSN](<https://en.wikipedia.org/wiki/ISSN_(identifier)>) [0018-9162](<https://search.worldcat.org/issn/0018-9162>).*
9. 1 2 3 4 *alastairn. ["CAP Theorem"](<https://www.scylladb.com/glossary/cap-theorem/>). *ScyllaDB*. Retrieved 2025-12-06.*
10. ↑ *Kleppmann, Martin (2015-09-18). [A Critique of the CAP Theorem](<https://www.repository.cam.ac.uk/handle/1810/267054>) (Report). Apollo - University of Cambridge Repository. [arXiv](<https://en.wikipedia.org/wiki/ArXiv_(identifier)>):[1509.05393](<https://arxiv.org/abs/1509.05393>). [Bibcode](<https://en.wikipedia.org/wiki/Bibcode_(identifier)>):[2015arXiv150905393K](<https://ui.adsabs.harvard.edu/abs/2015arXiv150905393K>). [doi](<https://en.wikipedia.org/wiki/Doi_(identifier)>):[10.17863/CAM.13083](<https://doi.org/10.17863%2FCAM.13083>). [S2CID](<https://en.wikipedia.org/wiki/S2CID_(identifier)>) [1991487](<https://api.semanticscholar.org/CorpusID:1991487>). Retrieved 24 November 2019.*
11. ↑ *Martin, Kleppmann. ["Please stop calling databases CP or AP"](<https://martin.kleppmann.com/2015/05/11/please-stop-calling-databases-cp-or-ap.html>). *Martin Kleppmann's Blog*. Retrieved 24 November 2019.*
12. ↑ *["CAP theorem - Availability and Beyond: Understanding and Improving the Resilience of Distributed Systems on AWS"](<https://docs.aws.amazon.com/whitepapers/latest/availability-and-beyond-improving-resilience/cap-theorem.html>). *docs.aws.amazon.com*. Retrieved 2025-12-07.*
13. 1 2 *Abadi, Daniel (2010-04-23). ["DBMS Musings: Problems with CAP, and Yahoo's little known NoSQL system"](<http://dbmsmusings.blogspot.com/2010/04/problems-with-cap-and-yahoos-little.html>). *DBMS Musings*. Retrieved 2018-01-23.*
14. 1 2 3 *Brewer, Eric (2012). ["CAP twelve years later: How the "rules" have changed"](<http://www.infoq.com/articles/cap-twelve-years-later-how-the-rules-have-changed>). *Computer*. **45** (2). Institute of Electrical and Electronics Engineers (IEEE): 23–29. [doi](<https://en.wikipedia.org/wiki/Doi_(identifier)>):[10.1109/mc.2012.37](<https://doi.org/10.1109%2Fmc.2012.37>). [ISSN](<https://en.wikipedia.org/wiki/ISSN_(identifier)>) [0018-9162](<https://search.worldcat.org/issn/0018-9162>). [S2CID](<https://en.wikipedia.org/wiki/S2CID_(identifier)>) [890105](<https://api.semanticscholar.org/CorpusID:890105>).*
15. ↑ *Armando Fox; Eric Brewer (1999). *Harvest, Yield and Scalable Tolerant Systems*. Proc. 7th Workshop Hot Topics in Operating Systems (HotOS 99). IEEE CS. pp. 174–178. [doi](<https://en.wikipedia.org/wiki/Doi_(identifier)>):[10.1109/HOTOS.1999.798396](<https://doi.org/10.1109%2FHOTOS.1999.798396>).*
16. ↑ *Eric Brewer. ["Towards Robust Distributed Systems"](<http://www.cs.berkeley.edu/~brewer/cs262b-2004/PODC-keynote.pdf>) (PDF).*
17. ↑ *Carpenter, Jeff; Hewitt, Eben (July 2016). [*Cassandra: The Definitive Guide*](<https://www.oreilly.com/library/view/cassandra-the-definitive/9781491933657/>) (2nd ed.). O'Reilly Media. [ISBN](<https://en.wikipedia.org/wiki/ISBN_(identifier)>) [9781491933657](<https://en.wikipedia.org/wiki/Special:BookSources/9781491933657>). In February 2012, Eric Brewer provided an updated perspective on his CAP theorem ... Brewer now describes the "2 out of 3" axiom as somewhat misleading. He notes that designers only need sacrifice consistency or availability in the presence of partitions, and that advances in partition recovery techniques have made it possible for designers to achieve high levels of both consistency and availability.*
18. ↑ *Ken Birman; Roy Friedman (April 1996). ["Trading Consistency for Availability in Distributed Systems"](<https://ecommons.cornell.edu/handle/1813/7235>). [hdl](<https://en.wikipedia.org/wiki/Hdl_(identifier)>):[1813/7235](<https://hdl.handle.net/1813%2F7235>).*
19. ↑ **Designing Data-Intensive Applications: The Big Ideas Behind Reliable, Scalable, and Maintainable Systems*. O'Reilly Media. [ISBN](<https://en.wikipedia.org/wiki/ISBN_(identifier)>) [978-1449373320](<https://en.wikipedia.org/wiki/Special:BookSources/978-1449373320>).*

Retrieved from "[https://en.wikipedia.org/w/index.php?title=CAP\_theorem&oldid=1374143441](<https://en.wikipedia.org/w/index.php?title=CAP_theorem&oldid=1374143441>)"

[Categories](<https://en.wikipedia.org/wiki/Help:Category>):

- [Distributed computing](<https://en.wikipedia.org/wiki/Category:Distributed_computing>)
- [Database management systems](<https://en.wikipedia.org/wiki/Category:Database_management_systems>)

Hidden categories:

- [Articles with short description](<https://en.wikipedia.org/wiki/Category:Articles_with_short_description>)
- [Short description matches Wikidata](<https://en.wikipedia.org/wiki/Category:Short_description_matches_Wikidata>)


<!-- sllmwiki:original-source -->
## Original source

- URL: <https://en.wikipedia.org/wiki/CAP_theorem>
- License: [CC-BY-SA-4.0](<https://creativecommons.org/licenses/by-sa/4.0/>)
