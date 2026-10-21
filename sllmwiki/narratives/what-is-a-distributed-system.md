# What is a distributed system?

## 1. Abstract

A distributed system is a set of computers, each with its own memory, that work together by sending messages over a network so that they can act toward a common goal, often looking like a single system to its users. There is no single agreed definition, but the common core is autonomous nodes plus message passing. What sets distributed systems apart from a program on one machine is what follows from that core: parts fail independently of each other, there is no global clock, each node sees only part of the system, and a request sent over the network may end with no way of knowing whether it took effect. Leslie Lamport's famous 1987 definition captures this from the user's side: a computer you didn't know existed can fail and stop yours from working. People build distributed systems anyway because a problem outgrows one machine, or because spreading work and copies of data across machines gives more capacity and more tolerance of faults than any single computer can. This brief gives the definition, a concrete example, the key ideas a newcomer needs, and how distributed computing differs from parallel computing; the design principles that follow from these properties are a question of their own.

## 2. Terminology

| Term | Definition |
|---|---|
| **Node** | One computer (or process) in a distributed system, with its own local memory. |
| **Message passing** | Nodes exchanging information only by sending messages to each other over a network. |
| **Partial failure** | Some parts of the system fail while the rest keep running. |
| **Fault domain** | A set of components that fail together, such as one machine. |
| **Scalability** | The ability to handle a growing amount of work, or to be enlarged to accommodate it. |
| **Availability** | The proportion of time a system is working and reachable by its users. |
| **Fault tolerance** | The ability to behave in a well-defined way when faults occur. |
| **Parallel computing** | Computing where processors share one memory to exchange information. |

## 3. Definition

Wikipedia defines distributed systems as "computer systems whose inter-communicating components are located on different networked computers", whose components "communicate and coordinate their actions by passing messages to one another in order to achieve a common goal". [\[1\]](../sources/ds-wikipedia-distributed-computing.md#distributed-computing---wikipedia) It adds that there is no single definition, but two properties are generally cited: several autonomous computational entities, each with its own local memory, and communication between them by message passing. [\[2\]](../sources/ds-wikipedia-distributed-computing.md#introduction)

In plain words: several computers, none of which can read the others' memory, cooperate by sending each other messages. Sometimes the users see the collection as one unit working on a shared problem; sometimes each computer has its own user, and the system coordinates shared resources or communication between them. [\[2\]](../sources/ds-wikipedia-distributed-computing.md#introduction)

A practitioner's definition looks at the same thing by its purpose: distributed programming is solving a problem you could solve on one computer by using several, usually because the problem no longer fits on one. [\[3\]](../sources/ds-mixu-distsys-intro.md#1-distributed-systems-at-a-high-level)

## 4. Why it matters

Lamport's definition, from a 1987 email to his colleagues, puts the newcomer's experience first: "A distributed system is one in which the failure of a computer you didn't even know existed can render your own computer unusable." [\[4\]](../sources/ds-lamport-definition-email.md) He wrote it after a machine-room electrical problem, noting that each new version of their system software made workstations more dependent on programs running elsewhere. [\[4\]](../sources/ds-lamport-definition-email.md)

A concrete example: take a Pac-Man game whose board lives on a separate server. On one machine, `board.find("pacman")` is a plain function call. Over a network it becomes a request and a reply that pass through eight steps (sending, delivering and validating the request, updating the server, then sending, delivering and validating the reply, and updating the client), and the client, the server and the network can each fail at any of them independently. [\[5\]](../sources/ds-aws-challenges-distributed-systems.md#requestreply-messaging-across-a-network) If no reply comes, the client times out and the result is unknown: the move may or may not have happened. [\[6\]](../sources/ds-aws-challenges-distributed-systems.md#handling-failure-modes-in-hard-real-time-distributed-systems)

People accept this because, given finite money, a problem eventually outgrows any single machine you can buy, and at that point several commodity machines with fault-tolerant software give the best value. [\[3\]](../sources/ds-mixu-distsys-intro.md#1-distributed-systems-at-a-high-level) A single machine either works or doesn't, whereas a distributed system can take unreliable components and build a reliable system on top of them. [\[7\]](../sources/ds-mixu-distsys-intro.md#availability-and-fault-tolerance)

## 5. Key ideas

**Independent failure.** On one machine, components share fate: if the CPU fails, everything fails, so programs rarely handle it. In a distributed system the servers and the network do not share fate, so each part's failure must be handled by the others. [\[6\]](../sources/ds-aws-challenges-distributed-systems.md#handling-failure-modes-in-hard-real-time-distributed-systems) One component failing does not make the whole system fail. [\[1\]](../sources/ds-wikipedia-distributed-computing.md#distributed-computing---wikipedia)

**No global clock, no global view.** Three challenges define the field: concurrency of components, the lack of a global clock, and independent failure of components. [\[1\]](../sources/ds-wikipedia-distributed-computing.md#distributed-computing---wikipedia) Each computer has a limited, incomplete view of the system, which may change while a program runs. [\[2\]](../sources/ds-wikipedia-distributed-computing.md#introduction)

**Uncertainty.** Independent failures and nondeterminism cause the most impactful problems, and it is not always possible to know whether something failed. [\[8\]](../sources/ds-aws-challenges-distributed-systems.md#introduction)

**What we want from it.** The goals are scalability, performance and availability. [\[9\]](../sources/ds-mixu-distsys-intro.md#what-we-want-to-achieve-scalability-and-other-good-things) Two physical facts work against them: the number of nodes, which raises the chance that some node fails, and the distance between them, which sets a floor on latency because information travels at most at the speed of light. [\[10\]](../sources/ds-mixu-distsys-intro.md#what-prevents-us-from-achieving-good-things)

**Two basic techniques.** Data can be split across nodes (partitioning) or copied to several nodes (replication); together they are the basic tools of distributed design. [\[11\]](../sources/ds-mixu-distsys-intro.md#design-techniques-partition-and-replicate)

## 6. Boundaries

**Distributed versus parallel.** The terms overlap and no clear line exists between them, but as a rule of thumb, in parallel computing processors share a memory, while in distributed computing each processor has its own private memory and information moves only by messages. [\[12\]](../sources/ds-wikipedia-distributed-computing.md#parallel-and-distributed-computing)

**Not only "many machines".** The word first meant computers spread over a geographical area, but it is now also used for autonomous processes on one physical computer that interact by message passing. [\[2\]](../sources/ds-wikipedia-distributed-computing.md#introduction)

**Not all equally hard.** Offline systems such as batch processing get most of the benefits with few of the downsides, while request/reply services, where a user waits for each answer, are the hardest to get right. [\[13\]](../sources/ds-aws-challenges-distributed-systems.md#types-of-distributed-systems)

## 7. Where to go next

- What are the common design tenets and principles for distributed systems? See [that narrative](what-are-the-common-design-tenets-and-principles-for-distributed-systems.md).
- What is consensus, and how do protocols such as Paxos and Raft reach it? (not yet written)
- How do partitioning and replication work in practice? (not yet written)

## 8. References

### 8.1 Sources read

- [ds-wikipedia-distributed-computing](../sources/ds-wikipedia-distributed-computing.md): the standard definition, common properties, and the line against parallel computing
- [ds-lamport-definition-email](../sources/ds-lamport-definition-email.md): Lamport's 1987 definition by failure
- [ds-mixu-distsys-intro](../sources/ds-mixu-distsys-intro.md): why build distributed systems, their goals and constraints, partitioning and replication
- [ds-aws-challenges-distributed-systems](../sources/ds-aws-challenges-distributed-systems.md): the request/reply example, independent failure, kinds of distributed systems

### 8.2 Citations

1. [ds-wikipedia-distributed-computing: Distributed computing - Wikipedia](../sources/ds-wikipedia-distributed-computing.md#distributed-computing---wikipedia)
2. [ds-wikipedia-distributed-computing: Introduction](../sources/ds-wikipedia-distributed-computing.md#introduction)
3. [ds-mixu-distsys-intro: 1. Distributed systems at a high level](../sources/ds-mixu-distsys-intro.md#1-distributed-systems-at-a-high-level)
4. [ds-lamport-definition-email](../sources/ds-lamport-definition-email.md)
5. [ds-aws-challenges-distributed-systems: Request/reply messaging across a network](../sources/ds-aws-challenges-distributed-systems.md#requestreply-messaging-across-a-network)
6. [ds-aws-challenges-distributed-systems: Handling failure modes in hard real-time distributed systems](../sources/ds-aws-challenges-distributed-systems.md#handling-failure-modes-in-hard-real-time-distributed-systems)
7. [ds-mixu-distsys-intro: Availability (and fault tolerance)](../sources/ds-mixu-distsys-intro.md#availability-and-fault-tolerance)
8. [ds-aws-challenges-distributed-systems: Introduction](../sources/ds-aws-challenges-distributed-systems.md#introduction)
9. [ds-mixu-distsys-intro: What we want to achieve: Scalability and other good things](../sources/ds-mixu-distsys-intro.md#what-we-want-to-achieve-scalability-and-other-good-things)
10. [ds-mixu-distsys-intro: What prevents us from achieving good things?](../sources/ds-mixu-distsys-intro.md#what-prevents-us-from-achieving-good-things)
11. [ds-mixu-distsys-intro: Design techniques: partition and replicate](../sources/ds-mixu-distsys-intro.md#design-techniques-partition-and-replicate)
12. [ds-wikipedia-distributed-computing: Parallel and distributed computing](../sources/ds-wikipedia-distributed-computing.md#parallel-and-distributed-computing)
13. [ds-aws-challenges-distributed-systems: Types of distributed systems](../sources/ds-aws-challenges-distributed-systems.md#types-of-distributed-systems)

## 9. Metadata

- **Version:** 1.0
- **Template:** concept_brief_overview
- **Created:** 2026-10-20
- **Last modified:** 2026-10-20
- **History:**
  - 1.0 (2026-10-20): First version.
