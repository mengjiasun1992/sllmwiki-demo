# distributed-system-design

<!-- sllmwiki:generated: rewritten by wiki-check.py --fix; edit tags.yaml and the manifests instead -->

Design tenets and principles for building distributed systems: failure, consistency, scalability, idempotency, backpressure

**Path:** [distributed-systems](distributed-systems.md) / distributed-system-design

## Subtopics

- [calm-theorem](calm-theorem.md): The CALM theorem: consistency as logical monotonicity, which distributed programs need no coordination
- [failure-modes](failure-modes.md): Classic failure modes of large-scale distributed systems (crashes, partitions, overload, cascading and metastable failures, retry storms, bad changes) and how to prevent or mitigate them

## Narratives

- [What are the common design tenets and principles for distributed systems?](../narratives/what-are-the-common-design-tenets-and-principles-for-distributed-systems.md) · 10 min
- [What is a distributed system?](../narratives/what-is-a-distributed-system.md) · 5 min (supporting)
- [How does Raft elect a leader and keep every server's log consistent through crashes and network partitions?](../narratives/how-does-raft-elect-a-leader-and-keep-every-servers-log-consistent.md) · 9 min (supporting)

## Sources

- [distributed-systems-van-steen-tanenbaum-brief-intro](../sources/distributed-systems-van-steen-tanenbaum-brief-intro.md): Open-access survey article by Maarten van Steen and Andrew S.
- [distributed-system-design-wikipedia-fallacies](../sources/distributed-system-design-wikipedia-fallacies.md): The Wikipedia article on the fallacies of distributed computing: a set of false assumptions, set out by L.
- [distributed-system-design-aws-well-architected-reliability-principles](../sources/distributed-system-design-aws-well-architected-reliability-principles.md): The design principles page of the Reliability Pillar of the AWS Well-Architected Framework, a short list of five principles that help increase the reliability of workloads in the cloud.
- [ewma-linkerd-beyond-round-robin](<https://linkerd.io/2016/03/16/beyond-round-robin-load-balancing-for-latency/>): A March 2016 Linkerd (Buoyant) blog post by Steve Jenson and Ruben Oanta argues that load balancing should protect against latency as well as provide scalability and resilience, and compares…
- [distributed-system-design-waldo-note-on-distributed-computing](<https://scholar.harvard.edu/files/waldo/files/waldo-94.pdf>): Jim Waldo, Geoff Wyant, Ann Wollrath and Sam Kendall's 1994 Sun Microsystems Laboratories technical report (SMLI TR-94-29) arguing that objects that interact in a distributed system must be dealt with…
- [distributed-system-design-azure-design-principles](../sources/distributed-system-design-azure-design-principles.md): The index page of the Azure Architecture Center's design principles for Azure applications (Microsoft Learn), an official vendor list of principles for building cloud applications that withstand failures, scale with…
- [distributed-system-design-azure-retry-pattern](../sources/distributed-system-design-azure-retry-pattern.md): Microsoft's Retry pattern from the Azure Architecture Center (Microsoft Learn).
- [distributed-system-design-azure-circuit-breaker-pattern](../sources/distributed-system-design-azure-circuit-breaker-pattern.md): Microsoft's Circuit Breaker pattern from the Azure Architecture Center.
- [distributed-system-design-azure-bulkhead-pattern](../sources/distributed-system-design-azure-bulkhead-pattern.md): Microsoft's Bulkhead pattern from the Azure Architecture Center, named after the partitioned sections of a ship's hull.
- [distributed-system-design-azure-throttling-pattern](../sources/distributed-system-design-azure-throttling-pattern.md): Microsoft's Throttling pattern from the Azure Architecture Center.
- [distributed-system-design-azure-transient-fault-handling](../sources/distributed-system-design-azure-transient-fault-handling.md): Microsoft's best practices for transient fault handling, from the Azure Architecture Center.
- [distributed-system-design-wikipedia-cap-theorem](../sources/distributed-system-design-wikipedia-cap-theorem.md): Wikipedia's article on the CAP theorem.
- [distributed-system-design-wikipedia-end-to-end-principle](../sources/distributed-system-design-wikipedia-end-to-end-principle.md): Wikipedia's article on the end-to-end principle, a design principle in computer networking that places application-specific features such as reliability and security in the communicating end nodes rather than in intermediary…
- [distributed-system-design-wikipedia-cascading-failure](../sources/distributed-system-design-wikipedia-cascading-failure.md): Wikipedia's article on cascading failure: a failure in a system of interconnected parts in which the failure of one or a few parts triggers the failure of others, and so…
- [distributed-system-design-hamilton-internet-scale-services](<https://www.usenix.org/legacy/event/lisa07/tech/full_papers/hamilton/hamilton.pdf>): James Hamilton's LISA 2007 paper (Windows Live Services Platform, Microsoft) summarises best practices, drawn from MSN, Windows Live, Exchange Hosted Services and other very large services, for designing and deploying…
- [distributed-system-design-aws-timeouts-retries-backoff](<https://aws.amazon.com/builders-library/timeouts-retries-and-backoff-with-jitter/>): An Amazon Builders' Library article by Marc Brooker (December 2019) on how Amazon handles transient failures in calls between services without overwhelming struggling ones.
- [distributed-system-design-aws-challenges-distributed-systems](<https://aws.amazon.com/builders-library/challenges-with-distributed-systems/>): An Amazon Builders' Library article by Jacob Gabrielson (2019) explaining why distributed systems are harder and less intuitive than other computing, the background for AWS's design practices.
- [distributed-system-design-google-sre-cascading-failures](<https://sre.google/sre-book/addressing-cascading-failures/>): Chapter 22 of Google's Site Reliability Engineering book (2017), 'Addressing Cascading Failures', written by Mike Ulrich, using the book's sample Shakespeare search service as its running example.
- [distributed-system-design-brewer-cap-twelve-years-later](<https://www.infoq.com/articles/cap-twelve-years-later-how-the-rules-have-changed/>): Eric Brewer's 2012 retrospective on the CAP theorem (first published in IEEE Computer, republished by InfoQ), by the theorem's originator.
- [distributed-system-design-ongaro-raft-consensus](<https://raft.github.io/raft.pdf>): Diego Ongaro and John Ousterhout's 'In Search of an Understandable Consensus Algorithm (Extended Version)' (Stanford, 2014; extended from the USENIX ATC 2014 paper), the paper that introduced Raft.
- [distributed-system-design-lamport-time-clocks](<https://lamport.azurewebsites.net/pubs/time-clocks.pdf>): Leslie Lamport's 'Time, Clocks, and the Ordering of Events in a Distributed System' (Communications of the ACM 21(7), 1978), saved from the author's own site.
- [distributed-system-design-saltzer-end-to-end-arguments](<https://deepplum.com/Papers/EndtoEnd.html>): Saltzer, Reed and Clark's 'End-to-End Arguments in System Design' (ACM Transactions on Computer Systems, 1984; revised from a 1981 conference paper), the classic paper stating a design principle for placing…
