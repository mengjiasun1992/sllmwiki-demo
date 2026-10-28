# failure-modes

<!-- sllmwiki:generated: rewritten by wiki-check.py --fix; edit tags.yaml and the manifests instead -->

Classic failure modes of large-scale distributed systems (crashes, partitions, overload, cascading and metastable failures, retry storms, bad changes) and how to prevent or mitigate them

**Path:** [distributed-systems](distributed-systems.md) / [distributed-system-design](distributed-system-design.md) / failure-modes

## Narratives

- [Can you walk me through the classic failure modes in large scale distributed system and what are the common solutions to prevent or mitigate each one?](../narratives/what-are-the-classic-failure-modes-of-large-scale-distributed-systems-and-their-mitigations.md) · 9 min

## Sources

- [distributed-system-design-wikipedia-fallacies](../sources/distributed-system-design-wikipedia-fallacies.md): The Wikipedia article on the fallacies of distributed computing: a set of false assumptions, set out by L.
- [distributed-system-design-aws-well-architected-reliability-principles](../sources/distributed-system-design-aws-well-architected-reliability-principles.md): The design principles page of the Reliability Pillar of the AWS Well-Architected Framework, a short list of five principles that help increase the reliability of workloads in the cloud.
- [distributed-system-design-azure-design-principles](../sources/distributed-system-design-azure-design-principles.md): The index page of the Azure Architecture Center's design principles for Azure applications (Microsoft Learn), an official vendor list of principles for building cloud applications that withstand failures, scale with…
- [distributed-system-design-azure-retry-pattern](../sources/distributed-system-design-azure-retry-pattern.md): Microsoft's Retry pattern from the Azure Architecture Center (Microsoft Learn).
- [distributed-system-design-azure-circuit-breaker-pattern](../sources/distributed-system-design-azure-circuit-breaker-pattern.md): Microsoft's Circuit Breaker pattern from the Azure Architecture Center.
- [distributed-system-design-azure-bulkhead-pattern](../sources/distributed-system-design-azure-bulkhead-pattern.md): Microsoft's Bulkhead pattern from the Azure Architecture Center, named after the partitioned sections of a ship's hull.
- [distributed-system-design-azure-throttling-pattern](../sources/distributed-system-design-azure-throttling-pattern.md): Microsoft's Throttling pattern from the Azure Architecture Center.
- [distributed-system-design-azure-transient-fault-handling](../sources/distributed-system-design-azure-transient-fault-handling.md): Microsoft's best practices for transient fault handling, from the Azure Architecture Center.
- [distributed-system-design-wikipedia-cap-theorem](../sources/distributed-system-design-wikipedia-cap-theorem.md): Wikipedia's article on the CAP theorem.
- [distributed-system-design-wikipedia-cascading-failure](../sources/distributed-system-design-wikipedia-cascading-failure.md): Wikipedia's article on cascading failure: a failure in a system of interconnected parts in which the failure of one or a few parts triggers the failure of others, and so…
- [distributed-system-design-aws-timeouts-retries-backoff](<https://aws.amazon.com/builders-library/timeouts-retries-and-backoff-with-jitter/>): An Amazon Builders' Library article by Marc Brooker (December 2019) on how Amazon handles transient failures in calls between services without overwhelming struggling ones.
- [distributed-system-design-google-sre-cascading-failures](<https://sre.google/sre-book/addressing-cascading-failures/>): Chapter 22 of Google's Site Reliability Engineering book (2017), 'Addressing Cascading Failures', written by Mike Ulrich, using the book's sample Shakespeare search service as its running example.
- [failure-modes-azure-retry-storm-antipattern](../sources/failure-modes-azure-retry-storm-antipattern.md): Microsoft's Retry Storm antipattern page in the Azure Architecture Center (CC BY 4.0).
- [failure-modes-azure-queue-based-load-leveling](../sources/failure-modes-azure-queue-based-load-leveling.md): Microsoft's Queue-Based Load Leveling pattern from the Azure Architecture Center (CC BY 4.0).
- [failure-modes-wikipedia-thundering-herd](../sources/failure-modes-wikipedia-thundering-herd.md): The Wikipedia article on the thundering herd problem (CC BY-SA 4.0).
- [failure-modes-wikipedia-split-brain](../sources/failure-modes-wikipedia-split-brain.md): The Wikipedia article on split-brain in computing (CC BY-SA 4.0), flagged by Wikipedia for possible original synthesis and missing citations.
- [failure-modes-bronson-metastable-failures](<https://sigops.org/s/conferences/hotos/2021/papers/hotos21-s11-bronson.pdf>): Nathan Bronson, Abutalib Aghayev, Aleksey Charapko and Timothy Zhu, 'Metastable Failures in Distributed Systems', HotOS 2021, 7 pages (ACM; the authors' PDF on the SIGOPS site carries ACM's standard copyright…
- [failure-modes-huang-gray-failure](<https://www.microsoft.com/en-us/research/wp-content/uploads/2017/06/paper-1.pdf>): Peng Huang, Chuanxiong Guo, Lidong Zhou, Jacob R.
