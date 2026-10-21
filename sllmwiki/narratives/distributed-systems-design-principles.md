# What are the common design tenets and principles for distributed systems?

## 1. Abstract

No single source defines one canonical list, but the sources read here agree on a core. They start from an assumption: failures are inevitable when one machine calls another, so a system must be designed to tolerate them rather than avoid them. From that follow the principles: detect failures and recover automatically; remove single points of failure through redundancy; scale out horizontally with many small, interchangeable parts; partition data and work around the limits of any one resource; minimize coordination between components, accepting eventual consistency where the application allows; bound every remote call with a timeout and retry carefully, with backoff, jitter and idempotent operations; and treat the trade-off between consistency and availability during network partitions as an explicit design decision. Operational tenets complete the list: automate change, test recovery, instrument the system for operators, and design for evolution through loose coupling and versioned interfaces. The fallacies of distributed computing, such as "the network is reliable" and "latency is zero", serve as a checklist of the assumptions these principles guard against.

## 2. Terminology

| Term | Definition |
|---|---|
| **CAP** | Consistency, availability, partition tolerance: a shared-data system can't have all three during a network partition |
| **Partition** | A break in communication that splits a system's nodes into groups that can't reach each other |
| **Eventual consistency** | Replicas may disagree for a while but converge once updates propagate |
| **Idempotent** | An operation that has the same effect however many times it is repeated |
| **Backoff** | Waiting longer between successive retries |
| **Jitter** | Randomness added to delays so many clients don't act at the same moment |
| **Single point of failure** | A component whose failure alone takes the system down |
| **[sllmwiki_reasoning]** | The narrative's own reasoning: a conclusion, comparison or reconstruction no cited source states. |
| **[sllmwiki_gap]** | Something the question needs that the sources don't answer. |

## 3. Definition

[sllmwiki_reasoning] There is no accepted definition of "the principles of distributed systems"; this narrative follows the common ground of the official cloud design principles from Microsoft and AWS, Amazon's practitioner guidance and Brewer's account of CAP.

Microsoft's list states the starting point: "In distributed systems, failures are inevitable." [\[1\]](../sources/azure-cloud-design-principles.md#core-principles) Amazon puts it the same way: it is impossible to build systems that never fail, so systems are designed to tolerate failure and to avoid magnifying a small percentage of failures into a complete outage. [\[2\]](../sources/builders-library-timeouts-retries-backoff.md#failures-happen) In plain words: the principles are the habits that keep a system working when its parts, and the network between them, misbehave.

## 4. Why it matters

The fallacies of distributed computing are the assumptions newcomers make that these principles correct: the network is reliable, latency is zero, bandwidth is infinite, the network is secure, topology doesn't change, there is one administrator, transport cost is zero, the network is homogeneous. [\[3\]](../sources/fallacies-of-distributed-computing.md#the-fallacies) For example, an application that assumes the network is reliable may wait forever for a reply during an outage and fail to retry once the network returns. [\[4\]](../sources/fallacies-of-distributed-computing.md#the-effects-of-the-fallacies)

## 5. Key ideas

**Design for failure and self-healing.** Detect failures, respond gracefully and recover automatically, using retries, health monitoring, circuit breakers and bulkheads. [\[1\]](../sources/azure-cloud-design-principles.md#core-principles) AWS adds: trigger automated recovery from metrics of business value, and test recovery procedures by simulating failures before they happen. [\[5\]](../sources/aws-reliability-pillar-design-principles.md#design-principles)

**Redundancy and horizontal scale.** Avoid single points of failure with multiple instances, replicas and multi-zone or multi-region deployments. [\[1\]](../sources/azure-cloud-design-principles.md#core-principles) Replace one large resource with many small ones so that one failure has less impact, and add or remove resources with demand instead of guessing capacity. [\[5\]](../sources/aws-reliability-pillar-design-principles.md#design-principles)

**Partition and minimize coordination.** Partition data and work around the limits of databases, networks and compute, with keys that avoid hotspots; decouple components, communicate asynchronously, and accept eventual consistency where appropriate. [\[1\]](../sources/azure-cloud-design-principles.md#core-principles)

**Choose consistency and availability deliberately.** During a partition, a system must choose between consistency and availability, but partitions are rare, the choice can differ per operation or data item, and the properties are matters of degree. [\[6\]](../sources/cap-twelve-years-later-brewer.md#why-2-of-3-is-missleading) Brewer's recommendation is to manage partitions explicitly: detect them, enter a partition mode that may limit some operations, and recover afterwards, restoring consistency and compensating for mistakes. [\[7\]](../sources/cap-twelve-years-later-brewer.md#managing-partitions) Choosing availability has a hidden cost: designers must know every invariant they will later restore. [\[8\]](../sources/cap-twelve-years-later-brewer.md#cap-confusion)

**Bound and retry remote calls carefully.** Set a timeout on every remote call, chosen from the downstream service's latency percentiles. [\[9\]](../sources/builders-library-timeouts-retries-backoff.md#timeouts) Retry transient failures with capped exponential backoff and a limit on attempts; retry at one layer only, since retries at each layer multiply load; and retry operations with side effects only when they are idempotent. [\[10\]](../sources/builders-library-timeouts-retries-backoff.md#retries-and-backoff) Add jitter to retries, timers and periodic jobs to spread out spikes. [\[11\]](../sources/builders-library-timeouts-retries-backoff.md#jitter)

**Operate, automate and evolve.** Give operators logging, distributed tracing and standard metrics; prefer managed services; design for evolution with loose coupling and versioned APIs; and ground decisions in business requirements such as recovery objectives and SLOs. [\[12\]](../sources/azure-cloud-design-principles.md#operational-principles) [\[13\]](../sources/azure-cloud-design-principles.md#strategic-principles) Make infrastructure changes through automation that is itself tracked and reviewed. [\[5\]](../sources/aws-reliability-pillar-design-principles.md#design-principles)

## 6. Boundaries

[sllmwiki_reasoning] These principles are design guidance, not theorems: CAP is the one proven limit among them, and the cloud vendors' lists mix general distributed-systems tenets with platform advice, such as using managed services or a vendor's identity service, that matters less outside the cloud.

[sllmwiki_gap] The end-to-end argument (Saltzer, Reed and Clark, 1984), a classic principle for placing functions such as reliability at the endpoints, is recorded only as a link here, so this narrative doesn't cover it from a saved source.

## 7. Where to go next

- How does the CAP theorem constrain replicated data stores, and what is PACELC?
- How do consensus protocols such as Paxos and Raft keep replicas consistent?
- What is the end-to-end argument, and how does it apply today?
- How do circuit breakers, bulkheads and load shedding prevent cascading failures?

## 8. References

### 8.1 Sources read

- [azure-cloud-design-principles](../sources/azure-cloud-design-principles.md): the core, operational and strategic principles
- [aws-reliability-pillar-design-principles](../sources/aws-reliability-pillar-design-principles.md): automated recovery, recovery testing, horizontal scale, capacity, automated change
- [builders-library-timeouts-retries-backoff](../sources/builders-library-timeouts-retries-backoff.md): failure as a given, timeouts, retries, backoff, jitter, idempotency
- [cap-twelve-years-later-brewer](../sources/cap-twelve-years-later-brewer.md): the consistency and availability trade-off and partition management
- [fallacies-of-distributed-computing](../sources/fallacies-of-distributed-computing.md): the assumptions the principles guard against

### 8.2 Citations

1. [azure-cloud-design-principles: Core principles](../sources/azure-cloud-design-principles.md#core-principles)
2. [builders-library-timeouts-retries-backoff: Failures Happen](../sources/builders-library-timeouts-retries-backoff.md#failures-happen)
3. [fallacies-of-distributed-computing: The fallacies](../sources/fallacies-of-distributed-computing.md#the-fallacies)
4. [fallacies-of-distributed-computing: The effects of the fallacies](../sources/fallacies-of-distributed-computing.md#the-effects-of-the-fallacies)
5. [aws-reliability-pillar-design-principles: Design principles](../sources/aws-reliability-pillar-design-principles.md#design-principles)
6. [cap-twelve-years-later-brewer: Why "2 of 3" is missleading](../sources/cap-twelve-years-later-brewer.md#why-2-of-3-is-missleading)
7. [cap-twelve-years-later-brewer: Managing partitions](../sources/cap-twelve-years-later-brewer.md#managing-partitions)
8. [cap-twelve-years-later-brewer: Cap confusion](../sources/cap-twelve-years-later-brewer.md#cap-confusion)
9. [builders-library-timeouts-retries-backoff: Timeouts](../sources/builders-library-timeouts-retries-backoff.md#timeouts)
10. [builders-library-timeouts-retries-backoff: Retries and backoff](../sources/builders-library-timeouts-retries-backoff.md#retries-and-backoff)
11. [builders-library-timeouts-retries-backoff: Jitter](../sources/builders-library-timeouts-retries-backoff.md#jitter)
12. [azure-cloud-design-principles: Operational principles](../sources/azure-cloud-design-principles.md#operational-principles)
13. [azure-cloud-design-principles: Strategic principles](../sources/azure-cloud-design-principles.md#strategic-principles)

## 9. Metadata

- **Version:** 1.0
- **Template:** concept_brief_overview
- **Created:** 2026-10-20
- **Last modified:** 2026-10-20
- **History:**
  - 1.0 (2026-10-20): First version.
