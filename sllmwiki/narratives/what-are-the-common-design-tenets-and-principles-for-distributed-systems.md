# What are the common design tenets and principles for distributed systems?

## 1. Abstract

The design principles of distributed systems all start from one fact: parts fail independently, and the network between them is neither reliable, instant nor free. The first tenet is therefore to reject the eight "fallacies of distributed computing", such as "the network is reliable" and "latency is zero", and to state the faults a design must tolerate, since a system can't tolerate faults no one considered. From there the principles fall into a few groups. Partition data to scale and replicate it to survive failures. Choose a consistency model on purpose: during a network partition a replicated system must give up either strong consistency or availability (CAP), and even without one, stronger consistency costs latency. Treat every remote call as able to time out with an unknown result: set timeouts, retry with capped exponential backoff and jitter, retry at only one layer, and make operations idempotent so retries are safe. Protect the system from itself under load by shedding work, degrading gracefully, budgeting retries and propagating deadlines, because retries and failover can turn one failure into a cascade. Contain failures with fault isolation, such as cells and circuit breakers. Finally, keep guarantees simple enough to reason about and test the failure paths. This brief introduces each principle at definition level, with where to go deeper.

## 2. Terminology

| Term | Definition |
|---|---|
| **Fallacies of distributed computing** | False assumptions about networks that newcomers to distributed applications commonly make. |
| **Partitioning** | Splitting a data set into independent subsets stored on different nodes. |
| **Replication** | Keeping copies of the same data on several nodes. |
| **Consistency model** | A contract stating what results reads and writes on a replicated store can return. |
| **Network partition** | A network failure that cuts nodes off from each other while the nodes keep running. |
| **CAP theorem** | During a network partition, a replicated store must choose between consistency and availability. |
| **Timeout** | The longest a client waits for a reply before giving up. |
| **Backoff** | Waiting longer between successive retries; exponential backoff doubles (or multiplies) the wait each time. |
| **Jitter** | Random delay added to retries or scheduled work so clients don't act at the same instant. |
| **Idempotent operation** | One that can be repeated with no additional side effects. |
| **Cascading failure** | A failure that grows through positive feedback, as one part's failure makes others more likely to fail. |
| **Load shedding** | Deliberately dropping some requests as a server nears overload. |
| **[sllmwiki_reasoning]** | The narrative's own reasoning: a conclusion, comparison or reconstruction no cited source states. |
| **[sllmwiki_gap]** | Something the question needs that the sources don't answer. |

## 3. Definition

[sllmwiki_gap] None of the sources gives a canonical list of distributed-systems design tenets; this narrative groups the principles that recur across them.

The closest thing to a founding list is the fallacies of distributed computing, collected at Sun Microsystems by L. Peter Deutsch and others: "The network is reliable; Latency is zero; Bandwidth is infinite; The network is secure; Topology doesn't change; There is one administrator; Transport cost is zero; The network is homogeneous". [\[1\]](../sources/ds-wikipedia-fallacies.md#the-fallacies) Each, when believed, causes real faults; for example, code written assuming a reliable network has little error handling, so it may stall or wait forever during an outage and fail to retry afterwards. [\[2\]](../sources/ds-wikipedia-fallacies.md#the-effects-of-the-fallacies)

[sllmwiki_reasoning] In plain words, the principles are the habits that follow from refusing those assumptions: expect failure and delay everywhere, and design so the system still behaves predictably when they happen.

## 4. Why it matters

A request across a network can end in an unknown result: the server may or may not have acted. In a banking service that means a withdrawal may or may not have happened. [\[3\]](../sources/ds-aws-challenges-distributed-systems.md#handling-unknown-unknowns-1) One response at Amazon is to distrust everything: any line of code that could cause network communication might not do what it's supposed to. [\[3\]](../sources/ds-aws-challenges-distributed-systems.md#handling-unknown-unknowns-1) Mishandled, such failures spread: one catalog server with a full disk returned fast, empty replies, the load balancer sent it more traffic because it looked fast, and the whole amazon.com website went down. [\[4\]](../sources/ds-aws-challenges-distributed-systems.md#distributed-bugs-spread-epidemically)

## 5. Key ideas

**Define the faults you tolerate.** Fault tolerance is behaving in a well-defined way when faults occur; you define the faults you expect and design for them, because "You can't tolerate faults you haven't considered." [\[5\]](../sources/ds-mixu-distsys-intro.md#availability-and-fault-tolerance) A system model states these assumptions about nodes, links and timing; the weaker its assumptions, the more robust the design, and real networks are best treated as unreliable and at best partially synchronous. [\[6\]](../sources/ds-mixu-distsys-abstractions.md#a-system-model)

**Partition and replicate.** Partitioning splits data so partitions can be processed in parallel and fail independently; replication copies data to improve performance and survive node failures. [\[7\]](../sources/ds-mixu-distsys-intro.md#design-techniques-partition-and-replicate) Replication is also the source of many problems, because copies must be kept in sync according to some consistency model. [\[8\]](../sources/ds-mixu-distsys-intro.md#replication)

**Choose consistency deliberately.** Since no distributed system is safe from network failures, partitions must be tolerated, and during one a system must choose between consistency (return an error or time out) and availability (answer with possibly stale data). [\[9\]](../sources/ds-wikipedia-cap-theorem.md#explanation) Brewer later called the "two out of three" reading misleading, because the sacrifice is only needed during partitions, and PACELC adds that even without a partition there is a trade-off between latency and consistency. [\[10\]](../sources/ds-wikipedia-cap-theorem.md#history) Strong consistency lets you program as if data weren't replicated; weaker models can give lower latency and higher availability. [\[8\]](../sources/ds-mixu-distsys-intro.md#replication)

**Bound every wait.** Set a timeout on every remote call, chosen from the downstream service's latency at an acceptable false-timeout rate, such as its 99.9th percentile. [\[11\]](../sources/ds-aws-timeouts-retries-backoff.md#timeouts) Propagate one absolute deadline through a request's tree of calls, so downstream servers stop work nobody is waiting for. [\[12\]](../sources/ds-google-sre-cascading-failures.md#deadline-propagation)

**Retry carefully.** Retries mask transient failures but are "selfish" and can worsen overload; use capped exponential backoff, limit retries, and retry at a single point in the stack, since retries at each layer multiply. [\[13\]](../sources/ds-aws-timeouts-retries-backoff.md#retries-and-backoff) Add jitter so retries and periodic work don't arrive at the same moment. [\[14\]](../sources/ds-aws-timeouts-retries-backoff.md#jitter) A server-wide retry budget can keep a capacity shortfall from becoming a global cascading failure. [\[15\]](../sources/ds-google-sre-cascading-failures.md#retries)

**Make retries safe with idempotency.** An idempotent operation can be retried with no additional side effects; Amazon's preferred approach is a unique client-supplied request identifier, recorded atomically with the work it triggers. [\[16\]](../sources/ds-aws-idempotent-apis.md#reducing-client-complexity-with-idempotent-api-design)

**Degrade instead of collapsing.** Load shedding drops some traffic near overload, and graceful degradation reduces the work per request, for example by searching only cached data. [\[17\]](../sources/ds-google-sre-cascading-failures.md#load-shedding-and-graceful-degradation) Past its breaking point, a service should let some errors or lower-quality results through rather than try to serve every request. [\[18\]](../sources/ds-google-sre-cascading-failures.md#closing-remarks)

**Isolate failures.** Cell-based architectures group services into self-contained cells so a failure stays local, and circuit breakers inside and between cells stop failures spreading. [\[19\]](../sources/ds-wikipedia-distributed-computing.md#cell-based-architecture) Circuit breakers have a cost: they add modal behavior that is hard to test, which is why Amazon often prefers local token-bucket retry limits. [\[13\]](../sources/ds-aws-timeouts-retries-backoff.md#retries-and-backoff)

**Keep guarantees understandable.** People reason better about systems that behave like a single system; weaker guarantees allow more performance but are harder to reason about. [\[20\]](../sources/ds-mixu-distsys-intro.md#abstractions-and-models)

## 6. Boundaries

**Principles, not guarantees.** The FLP result shows that no deterministic algorithm can guarantee consensus in an asynchronous system where even one process may crash, so consensus algorithms must give up safety or liveness when message delays are unbounded. [\[21\]](../sources/ds-mixu-distsys-abstractions.md#the-flp-impossibility-result) [sllmwiki_reasoning] The principles above manage these limits; none removes them.

**Improving the normal case can raise the risk of outage.** Retries, shifting load away from unhealthy servers and adding caches all help in steady state, yet can make a large failure more likely. [\[18\]](../sources/ds-google-sre-cascading-failures.md#closing-remarks)

## 7. Where to go next

- What is a distributed system? See [that narrative](what-is-a-distributed-system.md).
- How do consensus protocols such as Paxos and Raft work? (not yet written)
- Which consistency models exist, and when is each the right choice? (not yet written)
- How do you test the failure paths of a distributed system? (not yet written)

## 8. References

### 8.1 Sources read

- [ds-wikipedia-distributed-computing](../sources/ds-wikipedia-distributed-computing.md): cell-based architecture and circuit breakers
- [ds-aws-challenges-distributed-systems](../sources/ds-aws-challenges-distributed-systems.md): unknown results, distrust of everything, how failures spread
- [ds-aws-timeouts-retries-backoff](../sources/ds-aws-timeouts-retries-backoff.md): timeouts, retries, backoff and jitter
- [ds-aws-idempotent-apis](../sources/ds-aws-idempotent-apis.md): idempotency with client request identifiers
- [ds-google-sre-cascading-failures](../sources/ds-google-sre-cascading-failures.md): cascading failures, load shedding, retry budgets, deadlines
- [ds-mixu-distsys-intro](../sources/ds-mixu-distsys-intro.md): fault tolerance, partitioning, replication, abstractions
- [ds-mixu-distsys-abstractions](../sources/ds-mixu-distsys-abstractions.md): system models and the FLP result
- [ds-wikipedia-fallacies](../sources/ds-wikipedia-fallacies.md): the eight fallacies and their effects
- [ds-wikipedia-cap-theorem](../sources/ds-wikipedia-cap-theorem.md): CAP, its caveats and PACELC

### 8.2 Citations

1. [ds-wikipedia-fallacies: The fallacies](../sources/ds-wikipedia-fallacies.md#the-fallacies)
2. [ds-wikipedia-fallacies: The effects of the fallacies](../sources/ds-wikipedia-fallacies.md#the-effects-of-the-fallacies)
3. [ds-aws-challenges-distributed-systems: Handling unknown unknowns](../sources/ds-aws-challenges-distributed-systems.md#handling-unknown-unknowns-1)
4. [ds-aws-challenges-distributed-systems: Distributed bugs spread epidemically](../sources/ds-aws-challenges-distributed-systems.md#distributed-bugs-spread-epidemically)
5. [ds-mixu-distsys-intro: Availability (and fault tolerance)](../sources/ds-mixu-distsys-intro.md#availability-and-fault-tolerance)
6. [ds-mixu-distsys-abstractions: A system model](../sources/ds-mixu-distsys-abstractions.md#a-system-model)
7. [ds-mixu-distsys-intro: Design techniques: partition and replicate](../sources/ds-mixu-distsys-intro.md#design-techniques-partition-and-replicate)
8. [ds-mixu-distsys-intro: Replication](../sources/ds-mixu-distsys-intro.md#replication)
9. [ds-wikipedia-cap-theorem: Explanation](../sources/ds-wikipedia-cap-theorem.md#explanation)
10. [ds-wikipedia-cap-theorem: History](../sources/ds-wikipedia-cap-theorem.md#history)
11. [ds-aws-timeouts-retries-backoff: Timeouts](../sources/ds-aws-timeouts-retries-backoff.md#timeouts)
12. [ds-google-sre-cascading-failures: Deadline propagation](../sources/ds-google-sre-cascading-failures.md#deadline-propagation)
13. [ds-aws-timeouts-retries-backoff: Retries and backoff](../sources/ds-aws-timeouts-retries-backoff.md#retries-and-backoff)
14. [ds-aws-timeouts-retries-backoff: Jitter](../sources/ds-aws-timeouts-retries-backoff.md#jitter)
15. [ds-google-sre-cascading-failures: Retries](../sources/ds-google-sre-cascading-failures.md#retries)
16. [ds-aws-idempotent-apis: Reducing client complexity with idempotent API design](../sources/ds-aws-idempotent-apis.md#reducing-client-complexity-with-idempotent-api-design)
17. [ds-google-sre-cascading-failures: Load Shedding and Graceful Degradation](../sources/ds-google-sre-cascading-failures.md#load-shedding-and-graceful-degradation)
18. [ds-google-sre-cascading-failures: Closing Remarks](../sources/ds-google-sre-cascading-failures.md#closing-remarks)
19. [ds-wikipedia-distributed-computing: Cell-Based Architecture](../sources/ds-wikipedia-distributed-computing.md#cell-based-architecture)
20. [ds-mixu-distsys-intro: Abstractions and models](../sources/ds-mixu-distsys-intro.md#abstractions-and-models)
21. [ds-mixu-distsys-abstractions: The FLP impossibility result](../sources/ds-mixu-distsys-abstractions.md#the-flp-impossibility-result)

## 9. Metadata

- **Version:** 1.0
- **Template:** concept_brief_overview
- **Created:** 2026-10-20
- **Last modified:** 2026-10-20
- **History:**
  - 1.0 (2026-10-20): First version.
