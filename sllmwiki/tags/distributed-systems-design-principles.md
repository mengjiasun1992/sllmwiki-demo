# distributed-systems-design-principles

<!-- sllmwiki:generated: rewritten by wiki-check.py --fix; edit tags.yaml and the manifests instead -->

Common design tenets: fallacies, CAP and consistency trade-offs, fault tolerance, idempotency, scalability

**Path:** [distributed-systems](distributed-systems.md) / distributed-systems-design-principles

## Sources

- [ds-mixu-distsys-intro](../sources/ds-mixu-distsys-intro.md): Chapter 1 of Mikito Takada's free online book 'Distributed systems for fun and profit', a high-level introduction.
- [ds-mixu-distsys-abstractions](../sources/ds-mixu-distsys-abstractions.md): Chapter 2 of Mikito Takada's 'Distributed systems for fun and profit', on the models used to reason about distributed systems.
- [ds-wikipedia-distributed-computing](../sources/ds-wikipedia-distributed-computing.md): Wikipedia's article on distributed computing, the field that studies distributed systems: computer systems whose inter-communicating components sit on different networked computers and coordinate by passing messages toward a common goal.
- [ds-wikipedia-fallacies](../sources/ds-wikipedia-fallacies.md): Wikipedia's article on the fallacies of distributed computing, the false assumptions that programmers new to distributed applications invariably make, as listed by L.
- [ds-wikipedia-cap-theorem](../sources/ds-wikipedia-cap-theorem.md): Wikipedia's article on the CAP theorem (Brewer's theorem): a distributed data store can provide at most two of consistency (every read receives the most recent write or an error), availability…
- [ds-aws-challenges-distributed-systems](../sources/ds-aws-challenges-distributed-systems.md): Jacob Gabrielson's 2019 Amazon Builders' Library article on why distributed systems are hard.
- [ds-aws-timeouts-retries-backoff](../sources/ds-aws-timeouts-retries-backoff.md): Marc Brooker's 2019 Amazon Builders' Library article on the three essential tools Amazon uses to build resilient systems that tolerate failure without magnifying small failures into outages: timeouts, retries and…
- [ds-aws-idempotent-apis](../sources/ds-aws-idempotent-apis.md): Malcolm Featonby's Amazon Builders' Library article on making retries safe with idempotent APIs.
- [ds-google-sre-cascading-failures](../sources/ds-google-sre-cascading-failures.md): Chapter 22 of Google's Site Reliability Engineering book (2017), 'Addressing Cascading Failures' by Mike Ulrich, licensed CC BY-NC-ND 4.0.
