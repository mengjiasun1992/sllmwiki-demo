# Fallacies of distributed computing - Wikipedia

From Wikipedia, the free encyclopedia

The **fallacies of distributed computing** are a set of assertions made by [L. Peter Deutsch](<https://en.wikipedia.org/wiki/L._Peter_Deutsch>) and others at [Sun Microsystems](<https://en.wikipedia.org/wiki/Sun_Microsystems>) describing false assumptions that [programmers](<https://en.wikipedia.org/wiki/Programmer>) new to [distributed](<https://en.wikipedia.org/wiki/Distributed_computing>) [applications](<https://en.wikipedia.org/wiki/Application_software>) invariably make.

## The fallacies

\[[edit](<https://en.wikipedia.org/w/index.php?title=Fallacies_of_distributed_computing&action=edit&section=1>)\]

The originally listed [fallacies](<https://en.wikipedia.org/wiki/Fallacy>) are\[1\]

1. The [network](<https://en.wikipedia.org/wiki/Computer_network>) is reliable;
2. [Latency](<https://en.wikipedia.org/wiki/Latency_(engineering)>) is zero;
3. [Bandwidth](<https://en.wikipedia.org/wiki/Throughput>) is infinite;
4. The network is [secure](<https://en.wikipedia.org/wiki/Computer_security>);
5. [Topology](<https://en.wikipedia.org/wiki/Network_topology>) doesn't change;
6. There is one [administrator](<https://en.wikipedia.org/wiki/Network_administrator>);
7. Transport cost is zero;
8. The network is homogeneous;

## The effects of the fallacies

\[[edit](<https://en.wikipedia.org/w/index.php?title=Fallacies_of_distributed_computing&action=edit&section=2>)\]

1. Software applications are written with little error-handling on networking errors. During a network outage, such applications may stall or infinitely wait for an answer packet, permanently consuming memory or other resources. When the failed network becomes available, those applications may also fail to retry any stalled operations or require a (manual) restart.
2. Ignorance of network latency, and of the [packet loss](<https://en.wikipedia.org/wiki/Packet_loss>) it can cause, induces application- and transport-layer developers to allow unbounded traffic, greatly increasing dropped packets and wasting bandwidth.
3. Ignorance of bandwidth limits on the part of traffic senders can result in bottlenecks.
4. Complacency regarding network security results in being blindsided by malicious users and programs that continually adapt to security measures.
5. Changes in [network topology](<https://en.wikipedia.org/wiki/Network_topology>) can have effects on both bandwidth and latency issues, and therefore can have similar problems.
6. Multiple administrators, as with [subnets](<https://en.wikipedia.org/wiki/Subnetwork>) for rival companies, may institute conflicting policies of which senders of network traffic must be aware in order to complete their desired paths.
7. The "hidden" costs of building and maintaining a network or subnet are non-negligible and must consequently be noted in budgets to avoid vast shortfalls.
8. If a system assumes a homogeneous network, then it can lead to the same problems that result from the first three fallacies.

## History

\[[edit](<https://en.wikipedia.org/w/index.php?title=Fallacies_of_distributed_computing&action=edit&section=3>)\]

The list of fallacies originated at [Sun Microsystems](<https://en.wikipedia.org/wiki/Sun_Microsystems>). [L. Peter Deutsch](<https://en.wikipedia.org/wiki/L._Peter_Deutsch>), one of the original Sun "[Fellows](<https://en.wikipedia.org/wiki/Fellow>)", first created a list of seven fallacies in 1994; incorporating four fallacies [Bill Joy](<https://en.wikipedia.org/wiki/Bill_Joy>) and Dave Lyon had already identified in "The Fallacies of Networked Computing".\[2\] Around 1997, [James Gosling](<https://en.wikipedia.org/wiki/James_Gosling>), another Sun Fellow and the inventor of [Java](<https://en.wikipedia.org/wiki/Java_(programming_language)>), added the eighth fallacy.\[2\]

In an episode of "Software Engineering Radio" \[3\] Peter Deutsch added a ninth fallacy: "It's really an expansion of number 4. It extends beyond the boundaries of the physical network. ... The party you are communicating with is trustworthy."

Later in 2020, Mark Richards and Neal Ford expanded upon the original "Fallacies of Distributed Computing" by introducing three additional fallacies to address contemporary challenges in distributed systems: \[4\]

- Versioning is simple
- Compensating updates always work
- Observability is optional

## See also

\[[edit](<https://en.wikipedia.org/w/index.php?title=Fallacies_of_distributed_computing&action=edit&section=4>)\]

- [CAP theorem](<https://en.wikipedia.org/wiki/CAP_theorem>)
- [PACELC design principle](<https://en.wikipedia.org/wiki/PACELC_design_principle>)
- [Distributed computing](<https://en.wikipedia.org/wiki/Distributed_computing>)
- [Fine *vs* coarse grained SOA](<https://en.wikipedia.org/wiki/Microservices#Criticism_and_concerns>)

## References

\[[edit](<https://en.wikipedia.org/w/index.php?title=Fallacies_of_distributed_computing&action=edit&section=5>)\]

1. ↑ *Wilson, Gareth (2015-02-06). ["The Eight Fallacies of Distributed Computing - Tech Talk"](<https://web.archive.org/web/20171107014323/http://blog.fogcreek.com/eight-fallacies-of-distributed-computing-tech-talk/>). Archived from [the original](<http://blog.fogcreek.com/eight-fallacies-of-distributed-computing-tech-talk/>) on 2017-11-07. Retrieved 2017-06-18. The Eight Fallacies are something that I heard about at a Java One conference a long time ago by a guy named James Gosling. He attributed them to someone named Peter Deutsch and basically a bunch of guys at Sun had come up with a list of these fallacies.*
2. 1 2 *Van Den Hoogen, Ingrid (2004-01-08). ["Deutsch's Fallacies, 10 Years After"](<https://web.archive.org/web/20070811082651/http://java.sys-con.com/read/38665.htm>). Archived from [the original](<http://java.sys-con.com/read/38665.htm>) on 2007-08-11. Retrieved 2005-12-03.*
3. ↑ *[*L. Peter Deutsch on the Fallacies of Distributed Computing*](<https://se-radio.net/2021/07/episode-470-l-peter-deutsch-on-the-fallacies-of-distributed-computing/>). 2021-07-27. Event occurs at 57:10.*
4. ↑ *Richards, Mark. *Fundamentals of Software Architecture: An Engineering Approach*. O'Reilly Media. [ISBN](<https://en.wikipedia.org/wiki/ISBN_(identifier)>) [978-1492043454](<https://en.wikipedia.org/wiki/Special:BookSources/978-1492043454>).*

## External links

\[[edit](<https://en.wikipedia.org/w/index.php?title=Fallacies_of_distributed_computing&action=edit&section=6>)\]

- *Deutsch, Peter digital ability. ["The Eight Fallacies of Distributed Computing"](<https://nighthacks.com/jag/res/Fallacies.html>). *nighthacks.com*. Retrieved 2024-07-24.*
- *Yoavi, Neli (January 2008). ["Fallacies of Distributed Computing Explained"](<https://www.researchgate.net/publication/322500050_Fallacies_of_Distributed_Computing_Explained>). Retrieved 2024-07-24 – via [ResearchGate](<https://en.wikipedia.org/wiki/ResearchGate>).*

Retrieved from "[https://en.wikipedia.org/w/index.php?title=Fallacies\_of\_distributed\_computing&oldid=1332248533](<https://en.wikipedia.org/w/index.php?title=Fallacies_of_distributed_computing&oldid=1332248533>)"

[Categories](<https://en.wikipedia.org/wiki/Help:Category>):

- [Distributed computing architecture](<https://en.wikipedia.org/wiki/Category:Distributed_computing_architecture>)
- [Distributed computing problems](<https://en.wikipedia.org/wiki/Category:Distributed_computing_problems>)

Hidden categories:

- [Articles with short description](<https://en.wikipedia.org/wiki/Category:Articles_with_short_description>)
- [Short description matches Wikidata](<https://en.wikipedia.org/wiki/Category:Short_description_matches_Wikidata>)


<!-- sllmwiki:original-source -->
## Original source

- URL: <https://en.wikipedia.org/wiki/Fallacies_of_distributed_computing>
- License: [CC-BY-SA-4.0](<https://creativecommons.org/licenses/by-sa/4.0/>)
