# Distributed computing - Wikipedia

From Wikipedia, the free encyclopedia

Not to be confused with [Decentralized computing](<https://en.wikipedia.org/wiki/Decentralized_computing>).

**Distributed computing** is a field of [computer science](<https://en.wikipedia.org/wiki/Computer_science>) that studies **distributed systems**, defined as [computer systems](<https://en.wikipedia.org/wiki/Computer_system>) whose inter-communicating components are located on different [networked computers](<https://en.wikipedia.org/wiki/Computer_network>).\[1\]\[2\]

The components of a distributed system communicate and coordinate their actions by [passing messages](<https://en.wikipedia.org/wiki/Message_passing>) to one another in order to achieve a common goal. Three challenges of distributed systems are: maintaining [concurrency](<https://en.wikipedia.org/wiki/Concurrency_(computer_science)>) of components, overcoming the [lack of a global clock](<https://en.wikipedia.org/wiki/Clock_synchronization>), and managing the independent failure of components.\[1\] When a component of one system fails, the entire system does not fail.\[3\] Examples of distributed systems vary from [SOA-based systems](<https://en.wikipedia.org/wiki/Service-oriented_architecture>) to [microservices](<https://en.wikipedia.org/wiki/Microservices>) to [massively multiplayer online games](<https://en.wikipedia.org/wiki/Massively_multiplayer_online_game>) to [peer-to-peer applications](<https://en.wikipedia.org/wiki/Peer-to-peer>). Distributed systems cost more than monolithic architectures, primarily due to increased needs for additional hardware, servers, gateways, firewalls, new subnets, proxies, and so on.\[4\] Distributed systems can also suffer from [fallacies of distributed computing](<https://en.wikipedia.org/wiki/Fallacies_of_distributed_computing>). Conversely, a well-designed distributed system is more scalable, more durable, more changeable, and more fine-tuned than a [monolithic application](<https://en.wikipedia.org/wiki/Monolithic_application>) deployed on a single machine.\[5\] According to Marc Brooker: "a system is scalable in the range where [marginal cost](<https://en.wikipedia.org/wiki/Marginal_cost>) of additional workload is nearly constant." [Serverless](<https://en.wikipedia.org/wiki/Serverless_computing>) technologies fit this definition but the total cost of ownership, and not just the infrastructure cost must be considered.\[6\]

A [computer program](<https://en.wikipedia.org/wiki/Computer_program>) that runs within a distributed system is called a **distributed program**,\[7\] and *distributed programming* is the process of writing such programs.\[8\] There are many types of implementations for the message-passing mechanism, including pure HTTP, [RPC-like](<https://en.wikipedia.org/wiki/Remote_procedure_call>) connectors, and [message queues](<https://en.wikipedia.org/wiki/Message-oriented_middleware>).\[9\]

*Distributed computing* also refers to the use of distributed systems to solve computational problems. In *distributed computing*, a problem is divided into many tasks, each of which is solved by one or more computers,\[10\] which communicate with each other via message passing.\[11\]

## Introduction

\[[edit](<https://en.wikipedia.org/w/index.php?title=Distributed_computing&action=edit&section=1>)\]

The word *distributed* in terms such as "distributed system", "distributed programming", and "[distributed algorithm](<https://en.wikipedia.org/wiki/Distributed_algorithm>)" originally referred to computer networks where individual computers were physically distributed within some geographical area.\[12\] The terms are nowadays used in a much wider sense, even referring to autonomous [processes](<https://en.wikipedia.org/wiki/Process_(computing)>) that run on the same physical computer and interact with each other by [message passing](<https://en.wikipedia.org/wiki/Message_passing>).\[11\]

There is no single definition of a distributed system,\[13\] but two common properties are generally cited:

- There are several autonomous computational entities (*computers* or *[nodes](<https://en.wikipedia.org/wiki/Node_(networking)>)*), each of which has its own local [memory](<https://en.wikipedia.org/wiki/Memory_(computers)>).\[14\]
- The entities communicate with each other by message passing.\[15\]

A distributed system may have a common goal, such as solving a large computational problem;\[16\] the user then perceives the collection of autonomous processors as a unit. Alternatively, each computer may have its own user with individual needs, and the purpose of the distributed system is to coordinate the use of shared resources or provide communication services to the users.\[17\]

Other typical properties of distributed systems are:

- The system must [tolerate failures](<https://en.wikipedia.org/wiki/Fault_tolerance>) in individual computers.\[18\]
- The structure of the system (network topology, network latency, number of computers) is not known in advance.
- The system may consist of different kinds of computers and network links.
- The system may change during the execution of a distributed program.\[19\]
- Each computer has a limited, incomplete view of the system.
- Each computer may know only one part of the input.\[20\]

## Patterns

\[[edit](<https://en.wikipedia.org/w/index.php?title=Distributed_computing&action=edit&section=2>)\]

Here are common [architectural patterns](<https://en.wikipedia.org/wiki/Architectural_patterns>) used for distributed computing:\[21\]

- [Saga interaction pattern](<https://en.wikipedia.org/wiki/Saga_interaction_pattern>)
- [Microservices](<https://en.wikipedia.org/wiki/Microservices>)
- [Event driven architecture](<https://en.wikipedia.org/wiki/Event_driven_architecture>)
- [Client–server architecture](<https://en.wikipedia.org/wiki/Client–server_model>)
- [Service-oriented architecture (SOA)](<https://en.wikipedia.org/wiki/Service-oriented_architecture>)
- [Publish–subscribe pattern](<https://en.wikipedia.org/wiki/Publish–subscribe_pattern>)
- [Peer-to-peer (P2P)](<https://en.wikipedia.org/wiki/Peer-to-peer>)

## Events vs. Messages

\[[edit](<https://en.wikipedia.org/w/index.php?title=Distributed_computing&action=edit&section=3>)\]

In distributed systems, [events](<https://en.wikipedia.org/wiki/Event_(computing)>) represent a fact or state change (e.g., *OrderPlaced*) and are typically broadcast asynchronously to multiple consumers, promoting loose coupling and scalability. While events generally don't expect an immediate response, acknowledgment mechanisms are often implemented at the infrastructure level (e.g., Kafka commit offsets, SNS delivery statuses) rather than being an inherent part of the event pattern itself.\[22\]\[23\]

In contrast, [messages](<https://en.wikipedia.org/wiki/Message>) serve a broader role, encompassing commands (e.g., *ProcessPayment*), events (e.g., *PaymentProcessed*), and documents (e.g., *DataPayload*). Both events and messages can support various delivery guarantees, including at-least-once, at-most-once, and exactly-once, depending on the technology stack and implementation. However, exactly-once delivery is often achieved through idempotency mechanisms rather than true, infrastructure-level exactly-once semantics.\[22\]\[23\]

Delivery patterns for both events and messages include publish/subscribe (one-to-many) and point-to-point (one-to-one). While request/reply is technically possible, it is more commonly associated with messaging patterns rather than pure event-driven systems. Events excel at state propagation and decoupled notifications, while messages are better suited for command execution, workflow orchestration, and explicit coordination.\[22\]\[23\]

Modern architectures commonly combine both approaches, leveraging events for distributed state change notifications and messages for targeted command execution and structured workflows based on specific timing, ordering, and delivery requirements.\[22\]\[23\]

## Parallel and distributed computing

\[[edit](<https://en.wikipedia.org/w/index.php?title=Distributed_computing&action=edit&section=4>)\]

[[image: ](<https://thumb.wikimedia.org/wikipedia/commons/thumb/c/c6/Distributed-parallel.svg/330px-Distributed-parallel.svg.png?utm_source=en.wikipedia.org&utm_campaign=parser&utm_content=thumbnail>)](<https://en.wikipedia.org/wiki/File:Distributed-parallel.svg>)

(a), (b): a distributed system.\
(c): a parallel system.

Distributed systems are groups of networked computers which share a common goal for their work. The terms "[concurrent computing](<https://en.wikipedia.org/wiki/Concurrent_computing>)", "[parallel computing](<https://en.wikipedia.org/wiki/Parallel_computing>)", and "distributed computing" have much overlap, and no clear distinction exists between them.\[24\] The same system may be characterized both as "parallel" and "distributed"; the processors in a typical distributed system run concurrently in parallel.\[25\] Parallel computing may be seen as a particularly tightly coupled form of distributed computing,\[26\] and distributed computing may be seen as a loosely coupled form of parallel computing.\[13\] Nevertheless, it is possible to roughly classify concurrent systems as "parallel" or "distributed" using the following criteria:

- In parallel computing, all processors may have access to a [shared memory](<https://en.wikipedia.org/wiki/Shared_memory>) to exchange information between processors.\[27\]
- In distributed computing, each processor has its own private memory ([distributed memory](<https://en.wikipedia.org/wiki/Distributed_memory>)). Information is exchanged by passing messages between the processors.\[28\]

The figure on the right illustrates the difference between distributed and parallel systems. Figure (a) is a schematic view of a typical distributed system; the system is represented as a network topology in which each node is a computer and each line connecting the nodes is a communication link. Figure (b) shows the same distributed system in more detail: each computer has its own local memory, and information can be exchanged only by passing messages from one node to another by using the available communication links. Figure (c) shows a parallel system in which each processor has a direct access to a shared memory.

The situation is further complicated by the traditional uses of the terms parallel and distributed *algorithm* that do not quite match the above definitions of parallel and distributed *systems* (see below for more detailed discussion). Nevertheless, as a rule of thumb, high-performance parallel computation in a shared-memory multiprocessor uses parallel algorithms while the coordination of a large-scale distributed system uses distributed algorithms.\[29\]

## History

\[[edit](<https://en.wikipedia.org/w/index.php?title=Distributed_computing&action=edit&section=5>)\]

The use of concurrent processes which communicate through message-passing has its roots in [operating system](<https://en.wikipedia.org/wiki/Operating_system>) architectures studied in the 1960s.\[30\] The first widespread distributed systems were [local-area networks](<https://en.wikipedia.org/wiki/Local-area_networks>) such as [Ethernet](<https://en.wikipedia.org/wiki/Ethernet>), which was invented in the 1970s.\[31\]

[ARPANET](<https://en.wikipedia.org/wiki/ARPANET>), one of the predecessors of the [Internet](<https://en.wikipedia.org/wiki/Internet>), was introduced in the late 1960s, and ARPANET [e-mail](<https://en.wikipedia.org/wiki/E-mail>) was invented in the early 1970s. E-mail became the most successful application of ARPANET,\[32\] and it is probably the earliest example of a large-scale [distributed application](<https://en.wikipedia.org/wiki/Distributed_application>). In addition to ARPANET (and its successor, the global Internet), other early worldwide computer networks included [Usenet](<https://en.wikipedia.org/wiki/Usenet>) and [FidoNet](<https://en.wikipedia.org/wiki/FidoNet>) from the 1980s, both of which were used to support distributed discussion systems.\[33\]

The study of distributed computing became its own branch of computer science in the late 1970s and early 1980s. The first conference in the field, [Symposium on Principles of Distributed Computing](<https://en.wikipedia.org/wiki/Symposium_on_Principles_of_Distributed_Computing>) (PODC), dates back to 1982, and its counterpart [International Symposium on Distributed Computing](<https://en.wikipedia.org/wiki/International_Symposium_on_Distributed_Computing>) (DISC) was first held in Ottawa in 1985 as the International Workshop on Distributed Algorithms on Graphs.\[34\]

## Distributed Computing Architectures

\[[edit](<https://en.wikipedia.org/w/index.php?title=Distributed_computing&action=edit&section=6>)\]

Various hardware and software architectures are used for distributed computing. At a lower level, it is necessary to interconnect multiple CPUs with some sort of network, regardless of whether that network is printed onto a [circuit board](<https://en.wikipedia.org/wiki/Printed_circuit_board>) or made up of loosely coupled devices and cables. At a higher level, it is necessary to interconnect [processes](<https://en.wikipedia.org/wiki/Process_(computing)>) running on those CPUs with some sort of [communication system](<https://en.wikipedia.org/wiki/Communication_system>).\[35\]

Whether these CPUs share resources or not determines a first distinction between three types of architecture:

- [Shared memory](<https://en.wikipedia.org/wiki/Shared-memory_architecture>)
- [Shared disk](<https://en.wikipedia.org/wiki/Shared-disk_architecture>)
- [Shared nothing](<https://en.wikipedia.org/wiki/Shared-nothing_architecture>).

Distributed programming typically falls into one of several basic architectures: [client–server](<https://en.wikipedia.org/wiki/Client–server>), [three-tier](<https://en.wikipedia.org/wiki/Three-tier_(computing)>), [*n*-tier](<https://en.wikipedia.org/wiki/Multitier_architecture>), or [peer-to-peer](<https://en.wikipedia.org/wiki/Peer-to-peer>); or categories: [loose coupling](<https://en.wikipedia.org/wiki/Loose_coupling>), or [tight coupling](<https://en.wikipedia.org/wiki/Computer_cluster>).\[36\]

- [Client–server](<https://en.wikipedia.org/wiki/Client–server>): architectures where smart clients contact the server for data then format and display it to the users. Input at the client is committed back to the server when it represents a permanent change.
- [Three-tier](<https://en.wikipedia.org/wiki/Three-tier_(computing)>): architectures that move the client intelligence to a middle tier so that [stateless](<https://en.wikipedia.org/wiki/Stateless_protocol>) clients can be used. This simplifies application deployment. Most web applications are three-tier.
- [*n*-tier](<https://en.wikipedia.org/wiki/Multitier_architecture>): architectures that refer typically to web applications which further forward their requests to other enterprise services. This type of application is the one most responsible for the success of [application servers](<https://en.wikipedia.org/wiki/Application_server>).
- [Peer-to-peer](<https://en.wikipedia.org/wiki/Peer-to-peer>): architectures where there are no special machines that provide a service or manage the network resources.\[37\]: 227  Instead all responsibilities are uniformly divided among all machines, known as peers. Peers can serve both as clients and as servers.\[38\] Examples of this architecture include [BitTorrent](<https://en.wikipedia.org/wiki/BitTorrent>) and the [bitcoin network](<https://en.wikipedia.org/wiki/Bitcoin_network>).

Another basic aspect of distributed computing architecture is the method of communicating and coordinating work among concurrent processes. Through various message passing protocols, processes may communicate directly with one another, typically in a [main/sub](<https://en.wikipedia.org/wiki/Main/sub_(technology)?action=edit&redlink=1>) relationship. Alternatively, a ["database-centric" architecture](<https://en.wikipedia.org/wiki/Database-centric_architecture>) can enable distributed computing to be done without any form of direct [inter-process communication](<https://en.wikipedia.org/wiki/Inter-process_communication>), by utilizing a shared [database](<https://en.wikipedia.org/wiki/Database>).\[39\] Database-centric architecture in particular provides relational processing analytics in a schematic architecture allowing for live environment relay. This enables distributed computing functions both within and beyond the parameters of a networked database.\[40\]

### Cell-Based Architecture

\[[edit](<https://en.wikipedia.org/w/index.php?title=Distributed_computing&action=edit&section=7>)\]

Cell-based architecture is a distributed computing approach in which computational resources are organized into self-contained units called cells. Each cell operates independently, processing requests while maintaining scalability, fault isolation, and availability.\[41\]\[42\]\[43\]

A cell typically consists of multiple services or application components and functions as an autonomous unit. Some implementations replicate entire sets of [services](<https://en.wikipedia.org/wiki/Microservices>) across multiple cells, while others partition workloads between cells. In replicated models, requests may be rerouted to an operational cell if another experiences a failure. This design is intended to enhance system resilience by reducing the impact of localized failures.\[44\]\[45\]\[46\]

Some implementations employ [circuit breakers](<https://en.wikipedia.org/wiki/Circuit_breaker_design_pattern>) within and between cells. Within a cell, circuit breakers may be used to prevent cascading failures among services, while inter-cell circuit breakers can isolate failing cells and redirect traffic to those that remain operational.\[47\]\[48\]\[49\]

Cell-based architecture has been adopted in some large-scale distributed systems, particularly in cloud-native and high-availability environments, where fault isolation and redundancy are key design considerations. Its implementation varies depending on system requirements, infrastructure constraints, and operational objectives.\[50\]\[51\]\[52\]

## Applications

\[[edit](<https://en.wikipedia.org/w/index.php?title=Distributed_computing&action=edit&section=8>)\]

Reasons for using distributed systems and distributed computing may include:

- The very nature of an application may *require* the use of a communication network that connects several computers: for example, data produced in one physical location and required in another location.
- There are many cases in which the use of a single computer would be possible in principle, but the use of a distributed system is *beneficial* for practical reasons. For example:
  - It can allow for much larger storage and memory, faster compute, and higher bandwidth than a single machine.
  - It can provide more reliability than a non-distributed system, as there is no [single point of failure](<https://en.wikipedia.org/wiki/Single_point_of_failure>). Moreover, a distributed system may be easier to expand and manage than a monolithic uniprocessor system.\[53\]
  - It may be more cost-efficient to obtain the desired level of performance by using a [cluster](<https://en.wikipedia.org/wiki/Cluster_(computing)>) of several low-end computers, in comparison with a single high-end computer.

## Examples

\[[edit](<https://en.wikipedia.org/w/index.php?title=Distributed_computing&action=edit&section=9>)\]

Examples of distributed systems and applications of distributed computing include the following:\[54\]

- [telecommunications](<https://en.wikipedia.org/wiki/Telecommunications>) networks:
  - [telephone networks](<https://en.wikipedia.org/wiki/Telephone_network>) and [cellular networks](<https://en.wikipedia.org/wiki/Cellular_network>),
  - [computer networks](<https://en.wikipedia.org/wiki/Computer_network>) such as the [Internet](<https://en.wikipedia.org/wiki/Internet>),
  - [wireless sensor networks](<https://en.wikipedia.org/wiki/Wireless_sensor_network>),
  - [routing algorithms](<https://en.wikipedia.org/wiki/Routing_algorithm>);
- network applications:
  - [World Wide Web](<https://en.wikipedia.org/wiki/World_Wide_Web>) and [peer-to-peer networks](<https://en.wikipedia.org/wiki/Peer-to-peer_network>),
  - [massively multiplayer online games](<https://en.wikipedia.org/wiki/Massively_multiplayer_online_game>) and [virtual reality](<https://en.wikipedia.org/wiki/Virtual_reality>) communities,
  - [distributed databases](<https://en.wikipedia.org/wiki/Distributed_database>) and [distributed database management systems](<https://en.wikipedia.org/wiki/Distributed_database_management_system>),
  - [network file systems](<https://en.wikipedia.org/wiki/Distributed_file_system>),
  - distributed cache such as [burst buffers](<https://en.wikipedia.org/wiki/Burst_buffer>),
  - distributed information processing systems such as banking systems and airline reservation systems;
- real-time process control:
  - [aircraft](<https://en.wikipedia.org/wiki/Aircraft>) control systems,
  - [industrial control systems](<https://en.wikipedia.org/wiki/Industrial_control_system>);
- [parallel computation](<https://en.wikipedia.org/wiki/Parallel_computation>):
  - [scientific computing](<https://en.wikipedia.org/wiki/Scientific_computing>), including [cluster computing](<https://en.wikipedia.org/wiki/Cluster_computing>), [grid computing](<https://en.wikipedia.org/wiki/Grid_computing>), [cloud computing](<https://en.wikipedia.org/wiki/Cloud_computing>),\[55\] and various [volunteer computing projects](<https://en.wikipedia.org/wiki/List_of_volunteer_computing_projects>),
  - [distributed rendering](<https://en.wikipedia.org/wiki/Distributed_rendering>) in computer graphics.
- [peer-to-peer](<https://en.wikipedia.org/wiki/Peer-to-peer>)

## Reactive distributed systems

\[[edit](<https://en.wikipedia.org/w/index.php?title=Distributed_computing&action=edit&section=10>)\]

According to Reactive Manifesto, reactive distributed systems are responsive, resilient, elastic and message-driven. Subsequently, Reactive systems are more flexible, loosely-coupled and scalable. To make your systems reactive, you are advised to implement Reactive Principles. Reactive Principles are a set of principles and patterns which help to make your cloud native application as well as edge native applications more reactive.\[56\]

## Theoretical foundations

\[[edit](<https://en.wikipedia.org/w/index.php?title=Distributed_computing&action=edit&section=11>)\]

Main article: [Distributed algorithm](<https://en.wikipedia.org/wiki/Distributed_algorithm>)

### Models

\[[edit](<https://en.wikipedia.org/w/index.php?title=Distributed_computing&action=edit&section=12>)\]

Many tasks that we would like to automate by using a computer are of question–answer type: we would like to ask a question and the computer should produce an answer. In [theoretical computer science](<https://en.wikipedia.org/wiki/Theoretical_computer_science>), such tasks are called [computational problems](<https://en.wikipedia.org/wiki/Computational_problem>). Formally, a computational problem consists of *instances* together with a *solution* for each instance. Instances are questions that we can ask, and solutions are desired answers to these questions.

Theoretical computer science seeks to understand which computational problems can be solved by using a computer ([computability theory](<https://en.wikipedia.org/wiki/Computability_theory>)) and how efficiently ([computational complexity theory](<https://en.wikipedia.org/wiki/Computational_complexity_theory>)). Traditionally, it is said that a problem can be solved by using a computer if we can design an [algorithm](<https://en.wikipedia.org/wiki/Algorithm>) that produces a correct solution for any given instance. Such an algorithm can be implemented as a [computer program](<https://en.wikipedia.org/wiki/Computer_program>) that runs on a general-purpose computer: the program reads a problem instance from [input](<https://en.wikipedia.org/wiki/Information>), performs some computation, and produces the solution as [output](<https://en.wikipedia.org/wiki/Output_(computing)>). Formalisms such as [random-access machines](<https://en.wikipedia.org/wiki/Random-access_machine>) or [universal Turing machines](<https://en.wikipedia.org/wiki/Universal_Turing_machine>) can be used as abstract models of a sequential general-purpose computer executing such an algorithm.\[57\]\[58\]

The field of concurrent and distributed computing studies similar questions in the case of either multiple computers, or a computer that executes a network of interacting processes: which computational problems can be solved in such a network and how efficiently? However, it is not at all obvious what is meant by "solving a problem" in the case of a concurrent or distributed system: for example, what is the task of the algorithm designer, and what is the concurrent or distributed equivalent of a sequential general-purpose computer?\[*[citation needed](<https://en.wikipedia.org/wiki/Wikipedia:Citation_needed>)*\]

The discussion below focuses on the case of multiple computers, although many of the issues are the same for concurrent processes running on a single computer.

Three viewpoints are commonly used:

**Parallel algorithms in shared-memory model**

- All processors have access to a shared memory. The algorithm designer chooses the program executed by each processor.
- One theoretical model is the [parallel random-access machines](<https://en.wikipedia.org/wiki/Parallel_random-access_machine>) (PRAM) that are used.\[59\] However, the classical PRAM model assumes synchronous access to the shared memory.
- Shared-memory programs can be extended to distributed systems if the underlying operating system encapsulates the communication between nodes and virtually unifies the memory across all individual systems.
- A model that is closer to the behavior of real-world multiprocessor machines and takes into account the use of machine instructions, such as [Compare-and-swap](<https://en.wikipedia.org/wiki/Compare-and-swap>) (CAS), is that of *asynchronous shared memory*. There is a wide body of work on this model, a summary of which can be found in the literature.\[60\]\[61\]

**Parallel algorithms in message-passing model**

- The algorithm designer chooses the structure of the network, as well as the program executed by each computer.
- Models such as [Boolean circuits](<https://en.wikipedia.org/wiki/Boolean_circuits>) and [sorting networks](<https://en.wikipedia.org/wiki/Sorting_network>) are used.\[62\] A Boolean circuit can be seen as a computer network: each gate is a computer that runs an extremely simple computer program. Similarly, a sorting network can be seen as a computer network: each comparator is a computer.

**Distributed algorithms in message-passing model**

- The algorithm designer only chooses the computer program. All computers run the same program. The system must work correctly regardless of the structure of the network.
- A commonly used model is a [graph](<https://en.wikipedia.org/wiki/Graph_(discrete_mathematics)>) with one [finite-state machine](<https://en.wikipedia.org/wiki/Finite-state_machine>) per node.

In the case of distributed algorithms, computational problems are typically related to graphs. Often the graph that describes the structure of the computer network *is* the problem instance. This is illustrated in the following example.\[63\]

### An example

\[[edit](<https://en.wikipedia.org/w/index.php?title=Distributed_computing&action=edit&section=13>)\]

Consider the computational problem of finding a coloring of a given graph *G*. Different fields might take the following approaches:

**Centralized algorithms\[63\]**

- The graph *G* is encoded as a string, and the string is given as input to a computer. The computer program finds a coloring of the graph, encodes the coloring as a string, and outputs the result.

**Parallel algorithms**

- Again, the graph *G* is encoded as a string. However, multiple computers can access the same string in parallel. Each computer might focus on one part of the graph and produce a coloring for that part.
- The main focus is on high-performance computation that exploits the processing power of multiple computers in parallel.

**Distributed algorithms**

- The graph *G* is the structure of the computer network. There is one computer for each node of *G* and one communication link for each edge of *G*. Initially, each computer only knows about its immediate neighbors in the graph *G*; the computers must exchange messages with each other to discover more about the structure of *G*. Each computer must produce its own color as output.
- The main focus is on coordinating the operation of an arbitrary distributed system.\[63\]

While the field of parallel algorithms has a different focus than the field of distributed algorithms, there is much interaction between the two fields. For example, the [Cole–Vishkin algorithm](<https://en.wikipedia.org/wiki/Cole–Vishkin_algorithm>) for graph coloring\[64\] was originally presented as a parallel algorithm, but the same technique can also be used directly as a distributed algorithm.

Moreover, a parallel algorithm can be implemented either in a parallel system (using shared memory) or in a distributed system (using message passing).\[65\] The traditional boundary between parallel and distributed algorithms (choose a suitable network vs. run in any given network) does not lie in the same place as the boundary between parallel and distributed systems (shared memory vs. message passing).

### Complexity measures

\[[edit](<https://en.wikipedia.org/w/index.php?title=Distributed_computing&action=edit&section=14>)\]

In parallel algorithms, yet another resource in addition to time and space is the number of computers. Indeed, often there is a trade-off between the running time and the number of computers: the problem can be solved faster if there are more computers running in parallel (see [speedup](<https://en.wikipedia.org/wiki/Speedup>)). If a decision problem can be solved in [polylogarithmic time](<https://en.wikipedia.org/wiki/Polylogarithmic_time>) by using a polynomial number of processors, then the problem is said to be in the class [NC](<https://en.wikipedia.org/wiki/NC_(complexity)>).\[66\] The class NC can be defined equally well by using the PRAM formalism or Boolean circuits—PRAM machines can simulate Boolean circuits efficiently and vice versa.\[67\]

In the analysis of distributed algorithms, more attention is usually paid on communication operations than computational steps. Perhaps the simplest model of distributed computing is a synchronous system where all nodes operate in a lockstep fashion. This model is commonly known as the LOCAL model. During each *communication round*, all nodes in parallel (1) receive the latest messages from their neighbours, (2) perform arbitrary local computation, and (3) send new messages to their neighbors. In such systems, a central complexity measure is the number of synchronous communication rounds required to complete the task.\[68\]

This complexity measure is closely related to the [diameter](<https://en.wikipedia.org/wiki/Diameter_(graph_theory)>) of the network. Let *D* be the diameter of the network. On the one hand, any computable problem can be solved trivially in a synchronous distributed system in approximately 2*D* communication rounds: simply gather all information in one location (*D* rounds), solve the problem, and inform each node about the solution (*D* rounds).

On the other hand, if the running time of the algorithm is much smaller than *D* communication rounds, then the nodes in the network must produce their output without having the possibility to obtain information about distant parts of the network. In other words, the nodes must make globally consistent decisions based on information that is available in their *local D-neighbourhood*. Many distributed algorithms are known with the running time much smaller than *D* rounds, and understanding which problems can be solved by such algorithms is one of the central research questions of the field.\[69\] Typically an algorithm which solves a problem in polylogarithmic time in the network size is considered efficient in this model.

Another commonly used measure is the total number of bits transmitted in the network (cf. [communication complexity](<https://en.wikipedia.org/wiki/Communication_complexity>)).\[70\] The features of this concept are typically captured with the CONGEST(B) model, which is similarly defined as the LOCAL model, but where single messages can only contain B bits.

### Other problems

\[[edit](<https://en.wikipedia.org/w/index.php?title=Distributed_computing&action=edit&section=15>)\]

Traditional computational problems take the perspective that the user asks a question, a computer (or a distributed system) processes the question, then produces an answer and stops. However, there are also problems where the system is required not to stop, including the [dining philosophers problem](<https://en.wikipedia.org/wiki/Dining_philosophers_problem>) and other similar [mutual exclusion](<https://en.wikipedia.org/wiki/Mutual_exclusion>) problems. In these problems, the distributed system is supposed to continuously coordinate the use of shared resources so that no conflicts or [deadlocks](<https://en.wikipedia.org/wiki/Deadlock_(computer_science)>) occur.

There are also fundamental challenges that are unique to distributed computing, for example those related to *fault-tolerance*. Examples of related problems include [consensus problems](<https://en.wikipedia.org/wiki/Consensus_(computer_science)>),\[71\] [Byzantine fault tolerance](<https://en.wikipedia.org/wiki/Byzantine_fault_tolerance>),\[72\] and [self-stabilisation](<https://en.wikipedia.org/wiki/Self-stabilisation>).\[73\]

Much research is also focused on understanding the *asynchronous* nature of distributed systems:

- [Synchronizers](<https://en.wikipedia.org/wiki/Synchronizer_(algorithm)>) can be used to run synchronous algorithms in asynchronous systems.\[74\]
- [Logical clocks](<https://en.wikipedia.org/wiki/Logical_clock>) provide a causal [happened-before](<https://en.wikipedia.org/wiki/Happened-before>) ordering of events.\[75\]
- [Clock synchronization](<https://en.wikipedia.org/wiki/Clock_synchronization>) algorithms provide globally consistent physical time stamps.\[76\]

Note that in distributed systems, [latency](<https://en.wikipedia.org/wiki/Latency_(engineering)>) should be measured through "99th percentile" because "median" and "average" can be misleading.\[77\]

### [Election](<https://en.wikipedia.org/wiki/Leader_election>)

\[[edit](<https://en.wikipedia.org/w/index.php?title=Distributed_computing&action=edit&section=16>)\]

*Coordinator election* (or *leader election*) is the process of designating a single [process](<https://en.wikipedia.org/wiki/Process_(computing)>) as the organizer of some task distributed among several computers (nodes). Before the task is begun, all network nodes are either unaware which node will serve as the "coordinator" (or leader) of the task, or unable to communicate with the current coordinator. After a coordinator election algorithm has been run, however, each node throughout the network recognizes a particular, unique node as the task coordinator.\[78\]

The network nodes communicate among themselves in order to decide which of them will get into the "coordinator" state. For that, they need some method in order to break the symmetry among them. For example, if each node has unique and comparable identities, then the nodes can compare their identities, and decide that the node with the highest identity is the coordinator.\[78\]

The definition of this problem is often attributed to [LeLann](<https://en.wikipedia.org/wiki/Gérard_Le_Lann>), who formalized it as a method to create a new token in a token [ring network](<https://en.wikipedia.org/wiki/Ring_network>) in which the token has been lost.\[79\]

Coordinator election algorithms are designed to be economical in terms of total [bytes](<https://en.wikipedia.org/wiki/Byte>) transmitted, and time. The algorithm suggested by Gallager, Humblet, and Spira\[80\] for general undirected graphs has had a strong impact on the design of distributed algorithms in general, and won the [Dijkstra Prize](<https://en.wikipedia.org/wiki/Dijkstra_Prize>) for an influential paper in distributed computing.

Many other algorithms were suggested for different kinds of network [graphs](<https://en.wikipedia.org/wiki/Graph_(discrete_mathematics)>), such as undirected rings, unidirectional rings, complete graphs, grids, directed Euler graphs, and others. A general method that decouples the issue of the graph family from the design of the coordinator election algorithm was suggested by Korach, Kutten, and Moran.\[81\]

In order to perform coordination, distributed systems employ the concept of coordinators. The coordinator election problem is to choose a process from among a group of processes on different processors in a distributed system to act as the central coordinator. Several central coordinator election algorithms exist.\[82\]

### Properties of distributed systems

\[[edit](<https://en.wikipedia.org/w/index.php?title=Distributed_computing&action=edit&section=17>)\]

So far the focus has been on *designing* a distributed system that solves a given problem. A complementary research problem is *studying* the properties of a given distributed system.\[83\]\[84\]

The [halting problem](<https://en.wikipedia.org/wiki/Halting_problem>) is an analogous example from the field of centralised computation: we are given a computer program and the task is to decide whether it halts or runs forever. The halting problem is [undecidable](<https://en.wikipedia.org/wiki/Undecidable_problem>) in the general case, and naturally understanding the behaviour of a computer network is at least as hard as understanding the behaviour of one computer.\[85\]

However, there are many interesting special cases that are decidable. In particular, it is possible to reason about the behaviour of a network of finite-state machines. One example is telling whether a given network of interacting (asynchronous and non-deterministic) finite-state machines can reach a deadlock. This problem is [PSPACE-complete](<https://en.wikipedia.org/wiki/PSPACE-complete>),\[86\] i.e., it is decidable, but not likely that there is an efficient (centralised, parallel or distributed) algorithm that solves the problem in the case of large networks.

### Other Topics

\[[edit](<https://en.wikipedia.org/w/index.php?title=Distributed_computing&action=edit&section=18>)\]

[Linearizability](<https://en.wikipedia.org/wiki/Linearizability>)

## See also

\[[edit](<https://en.wikipedia.org/w/index.php?title=Distributed_computing&action=edit&section=19>)\]

- [Actor model](<https://en.wikipedia.org/wiki/Actor_model>) – Model of concurrent computation
- [Code mobility](<https://en.wikipedia.org/wiki/Code_mobility>) – Process in distributed computing
- [Dataflow programming](<https://en.wikipedia.org/wiki/Dataflow_programming>) – Computer programming paradigm
- [Decentralized computing](<https://en.wikipedia.org/wiki/Decentralized_computing>) – Distribution of jobs across different computers
- [Distributed algorithm](<https://en.wikipedia.org/wiki/Distributed_algorithm>) – Algorithm run on hardware built from interconnected processors
- [Distributed algorithmic mechanism design](<https://en.wikipedia.org/wiki/Distributed_algorithmic_mechanism_design>)
- [Distributed cache](<https://en.wikipedia.org/wiki/Distributed_cache>) – Type of computer cache
- [Distributed networking](<https://en.wikipedia.org/wiki/Distributed_networking>) – Multi-source interconnected computing
- [Distributed operating system](<https://en.wikipedia.org/wiki/Distributed_operating_system>) – Operating system designed to operate on multiple systems over a network computer
- [Eventual consistency](<https://en.wikipedia.org/wiki/Eventual_consistency>) – Consistency model used in distributed computing to achieve high availability
- [Edsger W. Dijkstra Prize in Distributed Computing](<https://en.wikipedia.org/wiki/Edsger_W._Dijkstra_Prize_in_Distributed_Computing>) – Annual conference on computing
- [Federation (information technology)](<https://en.wikipedia.org/wiki/Federation_(information_technology)>) – Group of network or telecommunication providers agreeing upon interoperability standards
- [Flat neighborhood network](<https://en.wikipedia.org/wiki/Flat_neighborhood_network>)
- [Fog computing](<https://en.wikipedia.org/wiki/Fog_computing>) – Architecture that uses edge devices
- [Grid computing](<https://en.wikipedia.org/wiki/Grid_computing>) – Use of widely distributed computer resources to reach a common goal
- [Internet GIS](<https://en.wikipedia.org/wiki/Internet_GIS>) – Internet technologies regarding spatial data
- [Jungle computing](<https://en.wikipedia.org/wiki/Jungle_computing>) – Type of distributed computing
- [Layered queueing network](<https://en.wikipedia.org/wiki/Layered_queueing_network>)
- [Library Oriented Architecture](<https://en.wikipedia.org/wiki/Library_Oriented_Architecture?action=edit&redlink=1>) (LOA)
- [List of distributed computing conferences](<https://en.wikipedia.org/wiki/List_of_distributed_computing_conferences>)
- [List of volunteer computing projects](<https://en.wikipedia.org/wiki/List_of_volunteer_computing_projects>)
- [Model checking](<https://en.wikipedia.org/wiki/Model_checking>) – Computer science field
- [Parallel distributed processing](<https://en.wikipedia.org/wiki/Parallel_distributed_processing>) – Cognitive science approach
- [Parallel programming model](<https://en.wikipedia.org/wiki/Parallel_programming_model>) – Abstraction of parallel computer architecture
- [Shared-nothing architecture](<https://en.wikipedia.org/wiki/Shared-nothing_architecture>) – Type of distributed computing architecture

## Notes

\[[edit](<https://en.wikipedia.org/w/index.php?title=Distributed_computing&action=edit&section=20>)\]

1. 1 2 *Tanenbaum, Andrew S.; Steen, Maarten van (2002). [*Distributed systems: principles and paradigms*](<https://www.distributed-systems.net/index.php/books/ds3/>). Upper Saddle River, NJ: Pearson Prentice Hall. [ISBN](<https://en.wikipedia.org/wiki/ISBN_(identifier)>) [0-13-088893-1](<https://en.wikipedia.org/wiki/Special:BookSources/0-13-088893-1>). [Archived](<https://web.archive.org/web/20200812174339/https://www.distributed-systems.net/index.php/books/ds3/>) from the original on 2020-08-12. Retrieved 2020-08-28.*
2. ↑ *"Distributed Programs". *Texts in Computer Science*. London: Springer London. 2010. pp. 373–406. [doi](<https://en.wikipedia.org/wiki/Doi_(identifier)>):[10.1007/978-1-84882-745-5\_11](<https://doi.org/10.1007%2F978-1-84882-745-5_11>). [ISBN](<https://en.wikipedia.org/wiki/ISBN_(identifier)>) [978-1-84882-744-8](<https://en.wikipedia.org/wiki/Special:BookSources/978-1-84882-744-8>). [ISSN](<https://en.wikipedia.org/wiki/ISSN_(identifier)>) [1868-0941](<https://search.worldcat.org/issn/1868-0941>). Systems consist of a number of physically distributed components that work independently using their private storage, but also communicate from time to time by explicit message passing. Such systems are called distributed systems.*
3. ↑ Dusseau & Dusseau 2016, p. 1–2.
4. ↑ *Ford, Neal (March 3, 2020). *Fundamentals of Software Architecture: An Engineering Approach* (1st ed.). O'Reilly Media. pp. 146–147. [ISBN](<https://en.wikipedia.org/wiki/ISBN_(identifier)>) [978-1-4920-4345-4](<https://en.wikipedia.org/wiki/Special:BookSources/978-1-4920-4345-4>).*
5. ↑ **Monolith to Microservices Evolutionary Patterns to Transform Your Monolith*. O'Reilly Media. [ISBN](<https://en.wikipedia.org/wiki/ISBN_(identifier)>) [978-1-4920-4781-0](<https://en.wikipedia.org/wiki/Special:BookSources/978-1-4920-4781-0>).*
6. ↑ **Building Serverless Applications on Knative*. O'Reilly Media. [ISBN](<https://en.wikipedia.org/wiki/ISBN_(identifier)>) [978-1-0981-4204-9](<https://en.wikipedia.org/wiki/Special:BookSources/978-1-0981-4204-9>).*
7. ↑ *"Distributed Programs". *Texts in Computer Science*. London: Springer London. 2010. pp. 373–406. [doi](<https://en.wikipedia.org/wiki/Doi_(identifier)>):[10.1007/978-1-84882-745-5\_11](<https://doi.org/10.1007%2F978-1-84882-745-5_11>). [ISBN](<https://en.wikipedia.org/wiki/ISBN_(identifier)>) [978-1-84882-744-8](<https://en.wikipedia.org/wiki/Special:BookSources/978-1-84882-744-8>). [ISSN](<https://en.wikipedia.org/wiki/ISSN_(identifier)>) [1868-0941](<https://search.worldcat.org/issn/1868-0941>). Distributed programs are abstract descriptions of distributed systems. A distributed program consists of a collection of processes that work concurrently and communicate by explicit message passing. Each process can access a set of variables which are disjoint from the variables that can be changed by any other process.*
8. ↑ Andrews (2000). Dolev (2000). Ghosh (2007), p. 10.
9. ↑ *Magnoni, L. (2015). ["Modern Messaging for Distributed Sytems (sic)"](<https://doi.org/10.1088%2F1742-6596%2F608%2F1%2F012038>). *Journal of Physics: Conference Series*. **608** (1) 012038. [Bibcode](<https://en.wikipedia.org/wiki/Bibcode_(identifier)>):[2015JPhCS.608a2038M](<https://ui.adsabs.harvard.edu/abs/2015JPhCS.608a2038M>). [doi](<https://en.wikipedia.org/wiki/Doi_(identifier)>):[10.1088/1742-6596/608/1/012038](<https://doi.org/10.1088%2F1742-6596%2F608%2F1%2F012038>). [ISSN](<https://en.wikipedia.org/wiki/ISSN_(identifier)>) [1742-6596](<https://search.worldcat.org/issn/1742-6596>).*
10. ↑ Godfrey (2002).
11. 1 2 Andrews (2000), p. 291–292. Dolev (2000), p. 5.
12. ↑ Lynch (1996), p. 1.
13. 1 2 Ghosh (2007), p. 10.
14. ↑ Andrews (2000), pp. 8–9, 291. Dolev (2000), p. 5. Ghosh (2007), p. 3. Lynch (1996), p. xix, 1. Peleg (2000), p. xv.
15. ↑ Andrews (2000), p. 291. Ghosh (2007), p. 3. Peleg (2000), p. 4.
16. ↑ Ghosh (2007), p. 3–4. Peleg (2000), p. 1.
17. ↑ Ghosh (2007), p. 4. Peleg (2000), p. 2.
18. ↑ Ghosh (2007), p. 4, 8. Lynch (1996), p. 2–3. Peleg (2000), p. 4.
19. ↑ Lynch (1996), p. 2. Peleg (2000), p. 1.
20. ↑ Ghosh (2007), p. 7. Lynch (1996), p. xix, 2. Peleg (2000), p. 4.
21. ↑ **Fundamentals of Software Architecture: An Engineering Approach*. O'Reilly Media. 2020. [ISBN](<https://en.wikipedia.org/wiki/ISBN_(identifier)>) [978-1-4920-4345-4](<https://en.wikipedia.org/wiki/Special:BookSources/978-1-4920-4345-4>).*
22. 1 2 3 4 *Kleppmann, Martin (2017). *Designing Data-Intensive Applications: The Big Ideas Behind Reliable, Scalable, and Maintainable Systems*. O'Reilly Media. [ISBN](<https://en.wikipedia.org/wiki/ISBN_(identifier)>) [978-1-4493-7332-0](<https://en.wikipedia.org/wiki/Special:BookSources/978-1-4493-7332-0>).*
23. 1 2 3 4 **Building Event-Driven Microservices: Leveraging Organizational Data at Scale*. [ISBN](<https://en.wikipedia.org/wiki/ISBN_(identifier)>) [978-1-4920-5789-5](<https://en.wikipedia.org/wiki/Special:BookSources/978-1-4920-5789-5>).*
24. ↑ Ghosh (2007), p. 10. Keidar (2008).
25. ↑ Lynch (1996), p. xix, 1–2. Peleg (2000), p. 1.
26. ↑ Peleg (2000), p. 1.
27. ↑ Papadimitriou (1994), Chapter 15. Keidar (2008).
28. ↑ See references in Introduction.
29. ↑ *Bentaleb, A.; Yifan, L.; Xin, J.; et al. (2016). ["Parallel and Distributed Algorithms"](<http://www.comp.nus.edu.sg/~rahul/allfiles/cs6234-16-pds.pdf>) (PDF). National University of Singapore. [Archived](<https://web.archive.org/web/20170326210614/http://www.comp.nus.edu.sg/~rahul/allfiles/cs6234-16-pds.pdf>) (PDF) from the original on 2017-03-26. Retrieved 20 July 2018.*
30. ↑ Andrews (2000), p. 348.
31. ↑ Andrews (2000), p. 32.
32. ↑ Peter (2004), [The history of email](<http://www.nethistory.info/History%20of%20the%20Internet/email.html>) [Archived](<https://web.archive.org/web/20090415220152/http://www.nethistory.info/History%20of%20the%20Internet/email.html>) 2009-04-15 at the [Wayback Machine](<https://en.wikipedia.org/wiki/Wayback_Machine>).
33. ↑ *Banks, M. (2012). [*On the Way to the Web: The Secret History of the Internet and its Founders*](<https://books.google.com/books?id=1J78hiHKaPoC&pg=PT67>). Apress. pp. 44–5. [ISBN](<https://en.wikipedia.org/wiki/ISBN_(identifier)>) [978-1-4302-5074-6](<https://en.wikipedia.org/wiki/Special:BookSources/978-1-4302-5074-6>). [Archived](<https://web.archive.org/web/20230120182450/https://books.google.com/books?id=1J78hiHKaPoC&pg=PT67>) from the original on 2023-01-20. Retrieved 2018-07-20.*
34. ↑ *Tel, G. (2000). [*Introduction to Distributed Algorithms*](<https://books.google.com/books?id=vlpnS25qAJQC&pg=PA35>). Cambridge University Press. pp. 35–36. [ISBN](<https://en.wikipedia.org/wiki/ISBN_(identifier)>) [978-0-521-79483-1](<https://en.wikipedia.org/wiki/Special:BookSources/978-0-521-79483-1>). [Archived](<https://web.archive.org/web/20230120182450/https://books.google.com/books?id=vlpnS25qAJQC&pg=PA35>) from the original on 2023-01-20. Retrieved 2018-07-20.*
35. ↑ *Ohlídal, M.; Jaroš, J.; Schwarz, J.; et al. (2006). "Evolutionary Design of OAB and AAB Communication Schedules for Interconnection Networks". In Rothlauf, F.; Branke, J.; Cagnoni, S. (eds.). *Applications of Evolutionary Computing*. Springer Science & Business Media. pp. 267–78. [ISBN](<https://en.wikipedia.org/wiki/ISBN_(identifier)>) [978-3-540-33237-4](<https://en.wikipedia.org/wiki/Special:BookSources/978-3-540-33237-4>).*
36. ↑ *["Real Time And Distributed Computing Systems"](<https://web.archive.org/web/20170110015222/https://pdfs.semanticscholar.org/2950/37ee46ac281590f67435380cebc385ac9749.pdf>) (PDF). [ISSN](<https://en.wikipedia.org/wiki/ISSN_(identifier)>) [2278-0661](<https://search.worldcat.org/issn/2278-0661>). Archived from [the original](<https://pdfs.semanticscholar.org/2950/37ee46ac281590f67435380cebc385ac9749.pdf>) (PDF) on 2017-01-10. Retrieved 2017-01-09.* `{{cite journal}}`: Cite journal requires `|journal=` ([help](<https://en.wikipedia.org/wiki/Help:CS1_errors#missing_periodical>))
37. ↑ Vigna P, Casey MJ. *The Age of Cryptocurrency: How Bitcoin and the Blockchain Are Challenging the Global Economic Order* St. Martin's Press January 27, 2015 [ISBN](<https://en.wikipedia.org/wiki/ISBN_(identifier)>) [9781250065636](<https://en.wikipedia.org/wiki/Special:BookSources/9781250065636>)
38. ↑ *Quang Hieu Vu; Mihai Lupu; Beng Chin Ooi (2010). *Peer-to-peer computing: principles and applications*. Heidelberg: Springer. p. 16. [ISBN](<https://en.wikipedia.org/wiki/ISBN_(identifier)>) [978-3-642-03513-5](<https://en.wikipedia.org/wiki/Special:BookSources/978-3-642-03513-5>). [OCLC](<https://en.wikipedia.org/wiki/OCLC_(identifier)>) [663093862](<https://search.worldcat.org/oclc/663093862>).*
39. ↑ *Lind P, Alm M (2006), "A database-centric virtual chemistry system", *J Chem Inf Model*, **46** (3): 1034–9, [doi](<https://en.wikipedia.org/wiki/Doi_(identifier)>):[10.1021/ci050360b](<https://doi.org/10.1021%2Fci050360b>), [PMID](<https://en.wikipedia.org/wiki/PMID_(identifier)>) [16711722](<https://pubmed.ncbi.nlm.nih.gov/16711722>).*
40. ↑ *Chiu, G (1990). "A model for optimal database allocation in distributed computing systems". *Proceedings. IEEE INFOCOM'90: Ninth Annual Joint Conference of the IEEE Computer and Communications Societies*.*
41. ↑ *Newman, Sam (2015-02-20). *Building Microservices*. O'Reilly Media. [ISBN](<https://en.wikipedia.org/wiki/ISBN_(identifier)>) [978-1-4919-5035-7](<https://en.wikipedia.org/wiki/Special:BookSources/978-1-4919-5035-7>).*
42. ↑ *Richardson, Chris (2019). *Microservices patterns: with examples in Java*. Shelter Island, NY: Manning Publications. [ISBN](<https://en.wikipedia.org/wiki/ISBN_(identifier)>) [978-1-61729-454-9](<https://en.wikipedia.org/wiki/Special:BookSources/978-1-61729-454-9>).*
43. ↑ *Christudas, Binildas (2019). *Practical Microservices Architectural Patterns: Event-Based Java Microservices with Spring Boot and Spring Cloud*. Berkeley, CA: Apress L. P. [ISBN](<https://en.wikipedia.org/wiki/ISBN_(identifier)>) [978-1-4842-4501-9](<https://en.wikipedia.org/wiki/Special:BookSources/978-1-4842-4501-9>).*
44. ↑ *Newman, Sam (2015-02-20). *Building Microservices*. O'Reilly Media. [ISBN](<https://en.wikipedia.org/wiki/ISBN_(identifier)>) [978-1-4919-5035-7](<https://en.wikipedia.org/wiki/Special:BookSources/978-1-4919-5035-7>).*
45. ↑ *Richardson, Chris (2019). *Microservices patterns: with examples in Java*. Shelter Island, NY: Manning Publications. [ISBN](<https://en.wikipedia.org/wiki/ISBN_(identifier)>) [978-1-61729-454-9](<https://en.wikipedia.org/wiki/Special:BookSources/978-1-61729-454-9>).*
46. ↑ *Christudas, Binildas (2019). *Practical Microservices Architectural Patterns: Event-Based Java Microservices with Spring Boot and Spring Cloud*. Berkeley, CA: Apress L. P. [ISBN](<https://en.wikipedia.org/wiki/ISBN_(identifier)>) [978-1-4842-4501-9](<https://en.wikipedia.org/wiki/Special:BookSources/978-1-4842-4501-9>).*
47. ↑ *Newman, Sam (2015-02-20). *Building Microservices*. O'Reilly Media. [ISBN](<https://en.wikipedia.org/wiki/ISBN_(identifier)>) [978-1-4919-5035-7](<https://en.wikipedia.org/wiki/Special:BookSources/978-1-4919-5035-7>).*
48. ↑ *Richardson, Chris (2019). *Microservices patterns: with examples in Java*. Shelter Island, NY: Manning Publications. [ISBN](<https://en.wikipedia.org/wiki/ISBN_(identifier)>) [978-1-61729-454-9](<https://en.wikipedia.org/wiki/Special:BookSources/978-1-61729-454-9>).*
49. ↑ *Christudas, Binildas (2019). *Practical Microservices Architectural Patterns: Event-Based Java Microservices with Spring Boot and Spring Cloud*. Berkeley, CA: Apress L. P. [ISBN](<https://en.wikipedia.org/wiki/ISBN_(identifier)>) [978-1-4842-4501-9](<https://en.wikipedia.org/wiki/Special:BookSources/978-1-4842-4501-9>).*
50. ↑ *Newman, Sam (2015-02-20). *Building Microservices*. O'Reilly Media. [ISBN](<https://en.wikipedia.org/wiki/ISBN_(identifier)>) [978-1-4919-5035-7](<https://en.wikipedia.org/wiki/Special:BookSources/978-1-4919-5035-7>).*
51. ↑ *Richardson, Chris (2019). *Microservices patterns: with examples in Java*. Shelter Island, NY: Manning Publications. [ISBN](<https://en.wikipedia.org/wiki/ISBN_(identifier)>) [978-1-61729-454-9](<https://en.wikipedia.org/wiki/Special:BookSources/978-1-61729-454-9>).*
52. ↑ *Christudas, Binildas (2019). *Practical Microservices Architectural Patterns: Event-Based Java Microservices with Spring Boot and Spring Cloud*. Berkeley, CA: Apress L. P. [ISBN](<https://en.wikipedia.org/wiki/ISBN_(identifier)>) [978-1-4842-4501-9](<https://en.wikipedia.org/wiki/Special:BookSources/978-1-4842-4501-9>).*
53. ↑ Elmasri & Navathe (2000), Section 24.1.2.
54. ↑ Andrews (2000), p. 10–11. Ghosh (2007), p. 4–6. Lynch (1996), p. xix, 1. Peleg (2000), p. xv. Elmasri & Navathe (2000), Section 24.
55. ↑ *Haussmann, J. (2019). "Cost-efficient parallel processing of irregularly structured problems in cloud computing environments". *Journal of Cluster Computing*. **22** (3): 887–909. [doi](<https://en.wikipedia.org/wiki/Doi_(identifier)>):[10.1007/s10586-018-2879-3](<https://doi.org/10.1007%2Fs10586-018-2879-3>). [S2CID](<https://en.wikipedia.org/wiki/S2CID_(identifier)>) [54447518](<https://api.semanticscholar.org/CorpusID:54447518>).*
56. ↑ **Reactive Application Development*. Manning. 2018. [ISBN](<https://en.wikipedia.org/wiki/ISBN_(identifier)>) [978-1-63835-581-6](<https://en.wikipedia.org/wiki/Special:BookSources/978-1-63835-581-6>).*
57. ↑ *Toomarian, N.B.; Barhen, J.; Gulati, S. (1992). ["Neural Networks for Real-Time Robotic Applications"](<https://books.google.com/books?id=CKTsCgAAQBAJ&pg=PA214>). In Fijany, A.; Bejczy, A. (eds.). *Parallel Computation Systems For Robotics: Algorithms And Architectures*. World Scientific. p. 214. [ISBN](<https://en.wikipedia.org/wiki/ISBN_(identifier)>) [978-981-4506-17-5](<https://en.wikipedia.org/wiki/Special:BookSources/978-981-4506-17-5>). [Archived](<https://web.archive.org/web/20200801024715/https://books.google.com/books?id=CKTsCgAAQBAJ&pg=PA214>) from the original on 2020-08-01. Retrieved 2018-07-20.*
58. ↑ *Savage, J.E. (1998). *Models of Computation: Exploring the Power of Computing*. Addison Wesley. p. 209. [ISBN](<https://en.wikipedia.org/wiki/ISBN_(identifier)>) [978-0-201-89539-1](<https://en.wikipedia.org/wiki/Special:BookSources/978-0-201-89539-1>).*
59. ↑ Cormen, Leiserson & Rivest (1990), Section 30.
60. ↑ Herlihy & Shavit (2008), Chapters 2–6.
61. ↑ Lynch (1996)
62. ↑ Cormen, Leiserson & Rivest (1990), Sections 28 and 29.
63. 1 2 3 TULSIRAMJI GAIKWAD-PATIL College of Engineering & Technology, Nagpur Department of Information Technology Introduction to Distributed Systems
64. ↑ Cole & Vishkin (1986). Cormen, Leiserson & Rivest (1990), Section 30.5.
65. ↑ Andrews (2000), p. ix.
66. ↑ Arora & Barak (2009), Section 6.7. Papadimitriou (1994), Section 15.3.
67. ↑ Papadimitriou (1994), Section 15.2.
68. ↑ Lynch (1996), p. 17–23.
69. ↑ Peleg (2000), Sections 2.3 and 7. Linial (1992). Naor & Stockmeyer (1995).
70. ↑ *Schneider, J.; Wattenhofer, R. (2011). ["Trading Bit, Message, and Time Complexity of Distributed Algorithms"](<https://books.google.com/books?id=dT6nwpXvES4C&pg=PA51>). In Peleg, D. (ed.). *Distributed Computing*. Springer Science & Business Media. pp. 51–65. [ISBN](<https://en.wikipedia.org/wiki/ISBN_(identifier)>) [978-3-642-24099-7](<https://en.wikipedia.org/wiki/Special:BookSources/978-3-642-24099-7>). [Archived](<https://web.archive.org/web/20200801023020/https://books.google.com/books?id=dT6nwpXvES4C&pg=PA51>) from the original on 2020-08-01. Retrieved 2018-07-20.*
71. ↑ Lynch (1996), Sections 5–7. Ghosh (2007), Chapter 13.
72. ↑ Lynch (1996), p. 99–102. Ghosh (2007), p. 192–193.
73. ↑ Dolev (2000). Ghosh (2007), Chapter 17.
74. ↑ Lynch (1996), Section 16. Peleg (2000), Section 6.
75. ↑ Lynch (1996), Section 18. Ghosh (2007), Sections 6.2–6.3.
76. ↑ Ghosh (2007), Section 6.4.
77. ↑ *Kamburugamuve, Supun; Ekanayake, Saliya (2021). *Foundations of Data Intensive Applications Large Scale Data Analytics Under the Hood*. John Wiley & Sons. [ISBN](<https://en.wikipedia.org/wiki/ISBN_(identifier)>) [978-1-119-71301-2](<https://en.wikipedia.org/wiki/Special:BookSources/978-1-119-71301-2>).*
78. 1 2 *Haloi, S. (2015). [*Apache ZooKeeper Essentials*](<https://books.google.com/books?id=Ym9uBgAAQBAJ&pg=PA100>). Packt Publishing Ltd. pp. 100–101. [ISBN](<https://en.wikipedia.org/wiki/ISBN_(identifier)>) [978-1-78439-832-3](<https://en.wikipedia.org/wiki/Special:BookSources/978-1-78439-832-3>). [Archived](<https://web.archive.org/web/20230120182456/https://books.google.com/books?id=Ym9uBgAAQBAJ&pg=PA100>) from the original on 2023-01-20. Retrieved 2018-07-20.*
79. ↑ *LeLann, G. (1977). "Distributed systems - toward a formal approach". *Information Processing*. **77**: 155·160 – via Elsevier.*
80. ↑ *[R. G. Gallager](<https://en.wikipedia.org/wiki/Robert_G._Gallager>), P. A. Humblet, and P. M. Spira (January 1983). ["A Distributed Algorithm for Minimum-Weight Spanning Trees"](<http://www.apposite-tech.com/blog/wp-content/uploads/2017/09/p66-gallager.pdf>) (PDF). *ACM Transactions on Programming Languages and Systems*. **5** (1): 66–77. [doi](<https://en.wikipedia.org/wiki/Doi_(identifier)>):[10.1145/357195.357200](<https://doi.org/10.1145%2F357195.357200>). [S2CID](<https://en.wikipedia.org/wiki/S2CID_(identifier)>) [2758285](<https://api.semanticscholar.org/CorpusID:2758285>). [Archived](<https://web.archive.org/web/20170926040957/http://www.apposite-tech.com/blog/wp-content/uploads/2017/09/p66-gallager.pdf>) (PDF) from the original on 2017-09-26.*`{{cite journal}}`: CS1 maint: multiple names: authors list ([link](<https://en.wikipedia.org/wiki/Category:CS1_maint:_multiple_names:_authors_list>))
81. ↑ *Korach, Ephraim; [Kutten, Shay](<https://en.wikipedia.org/wiki/Shay_Kutten>); [Moran, Shlomo](<https://en.wikipedia.org/wiki/Shlomo_Moran>) (1990). ["A Modular Technique for the Design of Efficient Distributed Leader Finding Algorithms"](<https://www.cs.technion.ac.il/~moran/r/PS/kkm.pdf>) (PDF). *ACM Transactions on Programming Languages and Systems*. **12** (1): 84–101. [doi](<https://en.wikipedia.org/wiki/Doi_(identifier)>):[10.1145/77606.77610](<https://doi.org/10.1145%2F77606.77610>). [S2CID](<https://en.wikipedia.org/wiki/S2CID_(identifier)>) [9175968](<https://api.semanticscholar.org/CorpusID:9175968>). [Archived](<https://web.archive.org/web/20070418150944/http://www.cs.technion.ac.il/~moran/r/PS/kkm.pdf>) (PDF) from the original on 2007-04-18.*
82. ↑ *Hamilton, Howard. ["Distributed Algorithms"](<http://www2.cs.uregina.ca/~hamilton/courses/330/notes/distributed/distributed.html>). [Archived](<https://web.archive.org/web/20121124002402/http://www2.cs.uregina.ca/~hamilton/courses/330/notes/distributed/distributed.html>) from the original on 2012-11-24. Retrieved 2013-03-03.*
83. ↑ *["Major unsolved problems in distributed systems?"](<https://cstheory.stackexchange.com/q/10045>). *cstheory.stackexchange.com*. [Archived](<https://web.archive.org/web/20230120182442/https://cstheory.stackexchange.com/questions/10045/major-unsolved-problems-in-distributed-systems>) from the original on 20 January 2023. Retrieved 16 March 2018.*
84. ↑ *["How big data and distributed systems solve traditional scalability problems"](<http://www.theserverside.com/feature/How-big-data-and-distributed-systems-solve-traditional-scalability-problems>). *theserverside.com*. [Archived](<https://web.archive.org/web/20180317232027/http://www.theserverside.com/feature/How-big-data-and-distributed-systems-solve-traditional-scalability-problems>) from the original on 17 March 2018. Retrieved 16 March 2018.*
85. ↑ *Svozil, K. (2011). ["Indeterminism and Randomness Through Physics"](<https://books.google.com/books?id=ep_FCgAAQBAJ&pg=PA112>). In Hector, Z. (ed.). *Randomness Through Computation: Some Answers, More Questions*. World Scientific. pp. 112–3. [ISBN](<https://en.wikipedia.org/wiki/ISBN_(identifier)>) [978-981-4462-63-1](<https://en.wikipedia.org/wiki/Special:BookSources/978-981-4462-63-1>). [Archived](<https://web.archive.org/web/20200801024745/https://books.google.com/books?id=ep_FCgAAQBAJ&pg=PA112>) from the original on 2020-08-01. Retrieved 2018-07-20.*
86. ↑ Papadimitriou (1994), Section 19.3.

## References

\[[edit](<https://en.wikipedia.org/w/index.php?title=Distributed_computing&action=edit&section=21>)\]

**Books**

- *Andrews, Gregory R. (2000), [*Foundations of Multithreaded, Parallel, and Distributed Programming*](<https://archive.org/details/foundationsofmul0000andr>), [Addison–Wesley](<https://en.wikipedia.org/wiki/Addison–Wesley>), [ISBN](<https://en.wikipedia.org/wiki/ISBN_(identifier)>) [978-0-201-35752-3](<https://en.wikipedia.org/wiki/Special:BookSources/978-0-201-35752-3>)*.
- *[Arora, Sanjeev](<https://en.wikipedia.org/wiki/Sanjeev_Arora_(computer_scientist)>); Barak, Boaz (2009), *Computational Complexity – A Modern Approach*, [Cambridge](<https://en.wikipedia.org/wiki/Cambridge_University_Press>), [ISBN](<https://en.wikipedia.org/wiki/ISBN_(identifier)>) [978-0-521-42426-4](<https://en.wikipedia.org/wiki/Special:BookSources/978-0-521-42426-4>)*.
- *[Cormen, Thomas H.](<https://en.wikipedia.org/wiki/Thomas_H._Cormen>); [Leiserson, Charles E.](<https://en.wikipedia.org/wiki/Charles_E._Leiserson>); [Rivest, Ronald L.](<https://en.wikipedia.org/wiki/Ron_Rivest>) (1990), [*Introduction to Algorithms*](<https://en.wikipedia.org/wiki/Introduction_to_Algorithms>) (1st ed.), [MIT Press](<https://en.wikipedia.org/wiki/MIT_Press>), [Bibcode](<https://en.wikipedia.org/wiki/Bibcode_(identifier)>):[1990ita..book.....C](<https://ui.adsabs.harvard.edu/abs/1990ita..book.....C>), [ISBN](<https://en.wikipedia.org/wiki/ISBN_(identifier)>) [978-0-262-03141-7](<https://en.wikipedia.org/wiki/Special:BookSources/978-0-262-03141-7>)*.
- *[Dolev, Shlomi](<https://en.wikipedia.org/wiki/Shlomi_Dolev>) (2000), *Self-Stabilization*, [MIT Press](<https://en.wikipedia.org/wiki/MIT_Press>), [ISBN](<https://en.wikipedia.org/wiki/ISBN_(identifier)>) [978-0-262-04178-2](<https://en.wikipedia.org/wiki/Special:BookSources/978-0-262-04178-2>)*.
- *Elmasri, Ramez; [Navathe, Shamkant B.](<https://en.wikipedia.org/wiki/Shamkant_Navathe>) (2000), *Fundamentals of Database Systems* (3rd ed.), [Addison–Wesley](<https://en.wikipedia.org/wiki/Addison–Wesley>), [ISBN](<https://en.wikipedia.org/wiki/ISBN_(identifier)>) [978-0-201-54263-9](<https://en.wikipedia.org/wiki/Special:BookSources/978-0-201-54263-9>)*.
- *Ghosh, Sukumar (2007), *Distributed Systems – An Algorithmic Approach*, Chapman & Hall/CRC, [ISBN](<https://en.wikipedia.org/wiki/ISBN_(identifier)>) [978-1-58488-564-1](<https://en.wikipedia.org/wiki/Special:BookSources/978-1-58488-564-1>)*.
- *[Lynch, Nancy A.](<https://en.wikipedia.org/wiki/Nancy_Lynch>) (1996), [*Distributed Algorithms*](<https://archive.org/details/distributedalgor0000lync>), [Morgan Kaufmann](<https://en.wikipedia.org/wiki/Morgan_Kaufmann>), [ISBN](<https://en.wikipedia.org/wiki/ISBN_(identifier)>) [978-1-55860-348-6](<https://en.wikipedia.org/wiki/Special:BookSources/978-1-55860-348-6>)*.
- *[Herlihy, Maurice P.](<https://en.wikipedia.org/wiki/Maurice_Herlihy>); [Shavit, Nir N.](<https://en.wikipedia.org/wiki/Nir_Shavit>) (2008), *The Art of Multiprocessor Programming*, [Morgan Kaufmann](<https://en.wikipedia.org/wiki/Morgan_Kaufmann>), [ISBN](<https://en.wikipedia.org/wiki/ISBN_(identifier)>) [978-0-12-370591-4](<https://en.wikipedia.org/wiki/Special:BookSources/978-0-12-370591-4>)*.
- *[Papadimitriou, Christos H.](<https://en.wikipedia.org/wiki/Christos_Papadimitriou>) (1994), *Computational Complexity*, [Addison–Wesley](<https://en.wikipedia.org/wiki/Addison–Wesley>), [ISBN](<https://en.wikipedia.org/wiki/ISBN_(identifier)>) [978-0-201-53082-7](<https://en.wikipedia.org/wiki/Special:BookSources/978-0-201-53082-7>)*.
- *[Peleg, David](<https://en.wikipedia.org/wiki/David_Peleg_(scientist)>) (2000), [*Distributed Computing: A Locality-Sensitive Approach*](<https://web.archive.org/web/20090806070332/http://www.ec-securehost.com/SIAM/DT05.html>), [SIAM](<https://en.wikipedia.org/wiki/SIAM>), [ISBN](<https://en.wikipedia.org/wiki/ISBN_(identifier)>) [978-0-89871-464-7](<https://en.wikipedia.org/wiki/Special:BookSources/978-0-89871-464-7>), archived from [the original](<http://www.ec-securehost.com/SIAM/DT05.html>) on 2009-08-06, retrieved 2009-07-16*.

**Articles**

- *Cole, Richard; [Vishkin, Uzi](<https://en.wikipedia.org/wiki/Uzi_Vishkin>) (1986), "Deterministic coin tossing with applications to optimal parallel list ranking", *Information and Control*, **70** (1): 32–53, [doi](<https://en.wikipedia.org/wiki/Doi_(identifier)>):[10.1016/S0019-9958(86)80023-7](<https://doi.org/10.1016%2FS0019-9958%2886%2980023-7>)*.
- *[Keidar, Idit](<https://en.wikipedia.org/wiki/Idit_Keidar>) (2008), ["Distributed computing column 32 – The year in review"](<https://web.archive.org/web/20140116123634/http://webee.technion.ac.il/~idish/sigactNews/#column%2032>), *[ACM SIGACT News](<https://en.wikipedia.org/wiki/ACM_SIGACT_News>)*, **39** (4): 53–54, [doi](<https://en.wikipedia.org/wiki/Doi_(identifier)>):[10.1145/1466390.1466402](<https://doi.org/10.1145%2F1466390.1466402>), [S2CID](<https://en.wikipedia.org/wiki/S2CID_(identifier)>) [7607391](<https://api.semanticscholar.org/CorpusID:7607391>), archived from [the original](<http://webee.technion.ac.il/~idish/sigactNews/#column%2032>) on 2014-01-16, retrieved 2009-08-20*.
- *[Linial, Nathan](<https://en.wikipedia.org/wiki/Nati_Linial>) (1992), "Locality in distributed graph algorithms", *SIAM Journal on Computing*, **21** (1): 193–201, [doi](<https://en.wikipedia.org/wiki/Doi_(identifier)>):[10.1137/0221015](<https://doi.org/10.1137%2F0221015>)*.
- *[Naor, Moni](<https://en.wikipedia.org/wiki/Moni_Naor>); [Stockmeyer, Larry](<https://en.wikipedia.org/wiki/Larry_Stockmeyer>) (1995), ["What can be computed locally?"](<http://www.wisdom.weizmann.ac.il/~naor/PAPERS/lcl.pdf>) (PDF), *SIAM Journal on Computing*, **24** (6): 1259–1277, [doi](<https://en.wikipedia.org/wiki/Doi_(identifier)>):[10.1137/S0097539793254571](<https://doi.org/10.1137%2FS0097539793254571>), [archived](<https://web.archive.org/web/20130108030723/http://www.wisdom.weizmann.ac.il/~naor/PAPERS/lcl.pdf>) (PDF) from the original on 2013-01-08*.

**Web sites**

- *Godfrey, Bill (2002). ["A primer on distributed computing"](<https://billpg.com/bacchae-co-uk/docs/dist.html>). [Archived](<https://web.archive.org/web/20210513163858/https://billpg.com/bacchae-co-uk/docs/dist.html>) from the original on 2021-05-13. Retrieved 2021-05-13.*
- *Peter, Ian (2004). ["Ian Peter's History of the Internet"](<http://www.nethistory.info/History%20of%20the%20Internet/>). [Archived](<https://web.archive.org/web/20100120220410/http://www.nethistory.info/History%20of%20the%20Internet/>) from the original on 2010-01-20. Retrieved 2009-08-04.*

## Further reading

\[[edit](<https://en.wikipedia.org/w/index.php?title=Distributed_computing&action=edit&section=22>)\]

**Books**

- *[Attiya, Hagit](<https://en.wikipedia.org/wiki/Hagit_Attiya>) and Jennifer Welch (2004), *Distributed Computing: Fundamentals, Simulations, and Advanced Topics*, Wiley-Interscience* [ISBN](<https://en.wikipedia.org/wiki/ISBN_(identifier)>) [0-471-45324-2](<https://en.wikipedia.org/wiki/Special:BookSources/0-471-45324-2>).
- *Christian Cachin; Rachid Guerraoui; Luís Rodrigues (2011), *Introduction to Reliable and Secure Distributed Programming* (2. ed.), Springer, [Bibcode](<https://en.wikipedia.org/wiki/Bibcode_(identifier)>):[2011itra.book.....C](<https://ui.adsabs.harvard.edu/abs/2011itra.book.....C>), [ISBN](<https://en.wikipedia.org/wiki/ISBN_(identifier)>) [978-3-642-15259-7](<https://en.wikipedia.org/wiki/Special:BookSources/978-3-642-15259-7>)*
- *Coulouris, George; et al. (2011), *Distributed Systems: Concepts and Design (5th Edition)*, Addison-Wesley* [ISBN](<https://en.wikipedia.org/wiki/ISBN_(identifier)>) [0-132-14301-1](<https://en.wikipedia.org/wiki/Special:BookSources/0-132-14301-1>).
- *Faber, Jim (1998), [*Java Distributed Computing*](<http://docstore.mik.ua/orelly/java-ent/dist/index.htm>), O'Reilly, [archived](<https://web.archive.org/web/20100824170917/http://docstore.mik.ua/orelly/java-ent/dist/index.htm>) from the original on 2010-08-24, retrieved 2010-09-29*: [Java Distributed Computing by Jim Faber, 1998](<http://docstore.mik.ua/orelly/java-ent/dist/index.htm>) [Archived](<https://web.archive.org/web/20100824170917/http://docstore.mik.ua/orelly/java-ent/dist/index.htm>) 2010-08-24 at the [Wayback Machine](<https://en.wikipedia.org/wiki/Wayback_Machine>)
- *Garg, Vijay K. (2002), *Elements of Distributed Computing*, Wiley-IEEE Press* [ISBN](<https://en.wikipedia.org/wiki/ISBN_(identifier)>) [0-471-03600-5](<https://en.wikipedia.org/wiki/Special:BookSources/0-471-03600-5>).
- *Tel, Gerard (1994), *Introduction to Distributed Algorithms*, Cambridge University Press*
- *[Chandy, Mani](<https://en.wikipedia.org/wiki/K._Mani_Chandy>); et al. (1988), *Parallel Program Design*, Addison-Wesley* [ISBN](<https://en.wikipedia.org/wiki/ISBN_(identifier)>) [0201058669](<https://en.wikipedia.org/wiki/Special:BookSources/0201058669>)
- *Dusseau, Remzi H.; Dusseau, Andrea (2016). [*Operating Systems: Three Easy Pieces, Chapter 48 Distributed Systems*](<https://web.archive.org/web/20210831013525/https://pages.cs.wisc.edu/~remzi/OSTEP/dist-intro.pdf>) (PDF). Archived from [the original](<https://pages.cs.wisc.edu/~remzi/OSTEP/dist-intro.pdf>) (PDF) on 31 August 2021. Retrieved 8 October 2021.*

**Articles**

- *Keidar, Idit; Rajsbaum, Sergio, eds. (2000–2009), ["Distributed computing column"](<https://web.archive.org/web/20140116123634/http://webee.technion.ac.il/~idish/sigactNews/>), [*ACM SIGACT News*](<https://en.wikipedia.org/wiki/ACM_SIGACT_News>), archived from [the original](<http://webee.technion.ac.il/~idish/sigactNews/>) on 2014-01-16, retrieved 2009-08-16*.
- *Birrell, A. D.; Levin, R.; Schroeder, M. D.; [Needham, R. M.](<https://en.wikipedia.org/wiki/Roger_M._Needham>) (April 1982). ["Grapevine: An exercise in distributed computing"](<http://web.cs.wpi.edu/~cs4513/d07/Papers/Birrell,%20Levin,%20et.%20al.,%20Grapevine.pdf>) (PDF). *[Communications of the ACM](<https://en.wikipedia.org/wiki/Communications_of_the_ACM>)*. **25** (4): 260–274. [doi](<https://en.wikipedia.org/wiki/Doi_(identifier)>):[10.1145/358468.358487](<https://doi.org/10.1145%2F358468.358487>). [S2CID](<https://en.wikipedia.org/wiki/S2CID_(identifier)>) [16066616](<https://api.semanticscholar.org/CorpusID:16066616>). [Archived](<https://web.archive.org/web/20160730064812/http://web.cs.wpi.edu/~cs4513/d07/Papers/Birrell,%20Levin,%20et.%20al.,%20Grapevine.pdf>) (PDF) from the original on 2016-07-30.*

**Conference Papers**

- *Rodriguez, Carlos; Villagra, Marcos; Baran, Benjamin (2007). "Asynchronous team algorithms for Boolean Satisfiability". *2007 2nd Bio-Inspired Models of Network, Information and Computing Systems*. pp. 66–69. [doi](<https://en.wikipedia.org/wiki/Doi_(identifier)>):[10.1109/BIMNICS.2007.4610083](<https://doi.org/10.1109%2FBIMNICS.2007.4610083>). [S2CID](<https://en.wikipedia.org/wiki/S2CID_(identifier)>) [15185219](<https://api.semanticscholar.org/CorpusID:15185219>).*

## External links

\[[edit](<https://en.wikipedia.org/w/index.php?title=Distributed_computing&action=edit&section=23>)\]

[image: ](<https://thumb.wikimedia.org/wikipedia/commons/thumb/f/fa/Wikiquote-logo.svg/40px-Wikiquote-logo.svg.png?utm_source=en.wikipedia.org&utm_campaign=parser&utm_content=thumbnail>)

Wikiquote has quotations related to ***[Distributed computing](<https://en.wikiquote.org/wiki/Special:Search/Distributed%20computing>)***.

- [[image: Wikimedia Commons logo](<https://thumb.wikimedia.org/wikipedia/en/thumb/4/4a/Commons-logo.svg/20px-Commons-logo.svg.png?utm_source=en.wikipedia.org&utm_campaign=parser&utm_content=thumbnail>)](<https://en.wikipedia.org/wiki/File:Commons-logo.svg>) Media related to [Distributed computing](<https://commons.wikimedia.org/wiki/Category:Distributed%20computing>) at Wikimedia Commons

Retrieved from "[https://en.wikipedia.org/w/index.php?title=Distributed\_computing&oldid=1376696275](<https://en.wikipedia.org/w/index.php?title=Distributed_computing&oldid=1376696275>)"

[Category](<https://en.wikipedia.org/wiki/Help:Category>):

- [Distributed computing](<https://en.wikipedia.org/wiki/Category:Distributed_computing>)

Hidden categories:

- [Articles with short description](<https://en.wikipedia.org/wiki/Category:Articles_with_short_description>)
- [Short description is different from Wikidata](<https://en.wikipedia.org/wiki/Category:Short_description_is_different_from_Wikidata>)
- [Webarchive template wayback links](<https://en.wikipedia.org/wiki/Category:Webarchive_template_wayback_links>)
- [CS1 errors: missing periodical](<https://en.wikipedia.org/wiki/Category:CS1_errors:_missing_periodical>)
- [All articles with unsourced statements](<https://en.wikipedia.org/wiki/Category:All_articles_with_unsourced_statements>)
- [Articles with unsourced statements from October 2016](<https://en.wikipedia.org/wiki/Category:Articles_with_unsourced_statements_from_October_2016>)
- [CS1 maint: multiple names: authors list](<https://en.wikipedia.org/wiki/Category:CS1_maint:_multiple_names:_authors_list>)
- [Pages displaying short descriptions of redirect targets via Module:Annotated link](<https://en.wikipedia.org/wiki/Category:Pages_displaying_short_descriptions_of_redirect_targets_via_Module:Annotated_link>)
- [Commons category link from Wikidata](<https://en.wikipedia.org/wiki/Category:Commons_category_link_from_Wikidata>)


<!-- sllmwiki:original-source -->
## Original source

- URL: <https://en.wikipedia.org/wiki/Distributed_computing>
- License: [CC-BY-SA-4.0](<https://creativecommons.org/licenses/by-sa/4.0/>)
