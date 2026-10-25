# Cascading failure - Wikipedia

From Wikipedia, the free encyclopedia

[[image: ](<https://thumb.wikimedia.org/wikipedia/commons/thumb/b/bd/Networkfailure.gif/250px-Networkfailure.gif?utm_source=en.wikipedia.org&utm_campaign=parser&utm_content=thumbnail>)](<https://en.wikipedia.org/wiki/File:Networkfailure.gif>)

An animation demonstrating a typical cascading failure from the loss of a single terminal in a [computer network](<https://en.wikipedia.org/wiki/Computer_network>)

A **cascading failure** is a failure in a [system](<https://en.wikipedia.org/wiki/System>) of [interconnected](<https://en.wikipedia.org/wiki/Interconnection>) parts in which the failure of one or few parts leads to the failure of other parts, growing progressively as a result of [positive feedback](<https://en.wikipedia.org/wiki/Positive_feedback>). This can occur when a single part fails, increasing the probability that other portions of the system fail.\[1\]\[2\]\[3\] Such a failure may happen in many types of systems, including power transmission, computer networking, finance, transportation systems, organisms, the human body, and ecosystems.

Cascading failures may occur when one part of the system fails. When this happens, other parts must then compensate for the failed component. This in turn overloads these nodes, causing them to fail as well, prompting additional nodes to fail one after another.

## In power transmission

\[[edit](<https://en.wikipedia.org/w/index.php?title=Cascading_failure&action=edit&section=1>)\]

Cascading failure is common in [power grids](<https://en.wikipedia.org/wiki/Power_grid>) when one of the elements fails (completely or partially) and shifts its load to nearby elements in the system. Those nearby elements are then pushed beyond their capacity so they become overloaded and shift their load onto other elements. Cascading failure is a common effect seen in [high voltage](<https://en.wikipedia.org/wiki/High_voltage>) systems, where a [single point of failure](<https://en.wikipedia.org/wiki/Single_point_of_failure>) (SPF) on a fully loaded or slightly overloaded system results in a sudden spike across all nodes of the system. This surge current can induce the already overloaded nodes into failure, setting off more overloads and thereby taking down the entire system in a very short time.

This failure process cascades through the elements of the system like a ripple on a pond and continues until substantially all of the elements in the system are compromised and/or the system becomes functionally disconnected from the source of its load. For example, under certain conditions a large power grid can collapse after the failure of a single transformer.

Monitoring the operation of a system, in [real-time](<https://en.wikipedia.org/wiki/Real-time_computing>), and judicious disconnection of parts can help stop a cascade. Another common technique is to calculate a safety margin for the system by computer simulation of possible failures, to establish safe operating levels below which none of the calculated scenarios is predicted to cause cascading failure, and to identify the parts of the network which are most likely to cause cascading failures.\[4\]

One of the primary problems with preventing electrical grid failures is that the speed of the control signal is no faster than the speed of the propagating power overload, i.e. since both the control signal and the electrical power are moving at the same speed, it is not possible to isolate the outage by sending a warning ahead to isolate the element.

### Examples

\[[edit](<https://en.wikipedia.org/w/index.php?title=Cascading_failure&action=edit&section=2>)\]

Cascading failure caused the following [power outages](<https://en.wikipedia.org/wiki/Power_outage>):

- [Blackout in Northeast America in 1965](<https://en.wikipedia.org/wiki/Northeast_blackout_of_1965>)
- [Blackout in Southern Brazil in 1999](<https://en.wikipedia.org/wiki/1999_Southern_Brazil_blackout>)
- [Blackout in Northeast America in 2003](<https://en.wikipedia.org/wiki/Northeast_blackout_of_2003>)
- [Blackout in Italy in 2003](<https://en.wikipedia.org/wiki/2003_Italy_blackout>)
- [Blackout in London in 2003](<https://en.wikipedia.org/wiki/2003_London_blackout>)
- [European Blackout in 2006](<https://en.wikipedia.org/wiki/2006_European_blackout>)
- [Blackout in Northern India in 2012](<https://en.wikipedia.org/wiki/2012_northern_India_power_grid_failure>)
- [Blackout in South Australia in 2016](<https://en.wikipedia.org/wiki/2016_South_Australian_blackout>)
- [Blackout in southeast South America in 2019](<https://en.wikipedia.org/wiki/2019_Argentina,_Paraguay_and_Uruguay_blackout>)
- [Blackout in Chile in 2025](<https://en.wikipedia.org/wiki/2025_Chile_blackout>)
- [Blackout in Spain, Portugal and France in 2025](<https://en.wikipedia.org/wiki/2025_European_power_outage>)

## In computer networks

\[[edit](<https://en.wikipedia.org/w/index.php?title=Cascading_failure&action=edit&section=3>)\]

Cascading failures can also occur in [computer networks](<https://en.wikipedia.org/wiki/Computer_network>) (such as the [Internet](<https://en.wikipedia.org/wiki/Internet>)) in which [network traffic](<https://en.wikipedia.org/wiki/Network_traffic_control>) is severely impaired or halted to or between larger sections of the network, caused by failing or disconnected hardware or software. In this context, the cascading failure is known by the term **cascade failure**. A cascade failure can affect large groups of people and systems.

The cause of a cascade failure is usually the overloading of a single, crucial [router](<https://en.wikipedia.org/wiki/Router_(computing)>) or node, which causes the node to go down, even briefly. It can also be caused by taking a node down for maintenance or upgrades. In either case, traffic is [routed](<https://en.wikipedia.org/wiki/Routing>) to or through another (alternative) path. This alternative path, as a result, becomes overloaded, causing it to go down, and so on. It will also affect systems which depend on the node for regular operation.

### Symptoms

\[[edit](<https://en.wikipedia.org/w/index.php?title=Cascading_failure&action=edit&section=4>)\]

The symptoms of a cascade failure include: [packet loss](<https://en.wikipedia.org/wiki/Packet_loss>) and high [network latency](<https://en.wikipedia.org/wiki/Network_latency>), not just to single systems, but to whole sections of a network or the internet. The high latency and packet loss is caused by the nodes that fail to operate due to [congestion collapse](<https://en.wikipedia.org/wiki/Congestion_collapse>), which causes them to still be present in the network but without much or any useful communication going through them. As a result, routes can still be considered valid, without them actually providing communication.

If enough routes go down because of a cascade failure, a complete section of the network or internet can become unreachable. Although undesired, this can help speed up the recovery from this failure as connections will time out, and other nodes will give up trying to establish connections to the section(s) that have become cut off, decreasing load on the involved nodes.

A common occurrence during a cascade failure is a walking failure, where sections go down, causing the next section to fail, after which the first section comes back up. This ripple can make several passes through the same sections or connecting nodes before stability is restored.

### History

\[[edit](<https://en.wikipedia.org/w/index.php?title=Cascading_failure&action=edit&section=5>)\]

Cascade failures are a relatively recent development, with the massive increase in traffic and the high interconnectivity between systems and networks. The term was first applied in this context in the late 1990s by a Dutch IT professional and has slowly become a relatively common term for this kind of large-scale failure.\[*[citation needed](<https://en.wikipedia.org/wiki/Wikipedia:Citation_needed>)*\]

### Example

\[[edit](<https://en.wikipedia.org/w/index.php?title=Cascading_failure&action=edit&section=6>)\]

Network failures typically start when a single network node fails. Initially, the traffic that would normally go through the node is stopped. Systems and users get errors about not being able to reach hosts. Usually, the redundant systems of an ISP respond very quickly, choosing another path through a different backbone. The routing path through this alternative route is longer, with more [hops](<https://en.wikipedia.org/wiki/Hop_(telecommunications)>) and subsequently going through more systems that normally do not process the amount of traffic suddenly offered.

This can cause one or more systems along the alternative route to go down, creating similar problems of their own.

Related systems are also affected in this case. As an example, [DNS](<https://en.wikipedia.org/wiki/Domain_name_system>) resolution might fail and what would normally cause systems to be interconnected, might break connections that are not even directly involved in the actual systems that went down. This, in turn, may cause seemingly unrelated nodes to develop problems, that can cause another cascade failure all on its own.

In December 2012, a partial loss (40%) of [Gmail](<https://en.wikipedia.org/wiki/Gmail>) service occurred globally, for 18 minutes. This loss of service was caused by a routine update of load balancing software which contained faulty logic—in this case, the error was caused by logic using an inappropriate 'all' instead of the more appropriate 'some'.\[5\] The cascading error was fixed by fully updating a single node in the network instead of partially updating all nodes at one time.

## Cascading structural failure

\[[edit](<https://en.wikipedia.org/w/index.php?title=Cascading_failure&action=edit&section=7>)\]

Certain load-bearing structures with discrete structural components can be subject to the "zipper effect", where the failure of a single structural member increases the load on adjacent members. In the case of the [Hyatt Regency walkway collapse](<https://en.wikipedia.org/wiki/Hyatt_Regency_walkway_collapse>), a suspended walkway (which was already overstressed due to an error in construction) failed when a single vertical suspension rod failed, overloading the neighboring rods which failed sequentially (i.e. like a [zipper](<https://en.wikipedia.org/wiki/Zipper>)). A bridge that can have such a failure is called fracture critical, and numerous bridge collapses have been caused by the failure of a single part. Properly designed structures use an adequate [factor of safety](<https://en.wikipedia.org/wiki/Factor_of_safety>) and/or alternate load paths to prevent this type of mechanical cascade failure.\[6\]

### Fracture cascade

\[[edit](<https://en.wikipedia.org/w/index.php?title=Cascading_failure&action=edit&section=8>)\]

[[image: ](<https://thumb.wikimedia.org/wikipedia/commons/thumb/b/b3/Sequential_fractures_-_Fracture_cascade_--_Smart-Servier.jpg/250px-Sequential_fractures_-_Fracture_cascade_--_Smart-Servier.jpg?utm_source=en.wikipedia.org&utm_campaign=parser&utm_content=thumbnail>)](<https://en.wikipedia.org/wiki/File:Sequential_fractures_-_Fracture_cascade_--_Smart-Servier.jpg>)

Chain reaction of osteoporotic fractures

Fracture cascade is a phenomenon in the context of geology and describes triggering a chain reaction of subsequent fractures by a single fracture.\[7\] The initial fracture leads to the propagation of additional fractures, causing a cascading effect throughout the material.

Fracture cascades can occur in various materials, including rocks, ice, metals, and ceramics.\[8\] A common example is the bending of dry [spaghetti](<https://en.wikipedia.org/wiki/Spaghetti>), which in most cases breaks into more than 2 pieces, as first observed by [Richard Feynman](<https://en.wikipedia.org/wiki/Richard_Feynman>).\[8\]

In the context of [osteoporosis](<https://en.wikipedia.org/wiki/Osteoporosis>), a fracture cascade is the increased risk of subsequent bone fractures after an initial one.\[9\]

## Other examples

\[[edit](<https://en.wikipedia.org/w/index.php?title=Cascading_failure&action=edit&section=9>)\]

### Biology

\[[edit](<https://en.wikipedia.org/w/index.php?title=Cascading_failure&action=edit&section=10>)\]

[Biochemical cascades](<https://en.wikipedia.org/wiki/Biochemical_cascade>) exist in biology, where a small reaction can have system-wide implications. One negative example is [ischemic cascade](<https://en.wikipedia.org/wiki/Ischemic_cascade>), in which a small [ischemic](<https://en.wikipedia.org/wiki/Ischemia>) attack releases [toxins](<https://en.wikipedia.org/wiki/Toxin>) which kill off far more cells than the initial damage, resulting in more toxins being released. Current research is to find a way to block this cascade in [stroke](<https://en.wikipedia.org/wiki/Stroke>) patients to minimize the damage.

In the study of extinction, sometimes the extinction of one species will cause many other extinctions to happen. Such a species is known as a [keystone species](<https://en.wikipedia.org/wiki/Keystone_species>).

### Electronics

\[[edit](<https://en.wikipedia.org/w/index.php?title=Cascading_failure&action=edit&section=11>)\]

Another example is the [Cockcroft–Walton generator](<https://en.wikipedia.org/wiki/Cockcroft–Walton_generator>), which can also experience cascade failures wherein one failed [diode](<https://en.wikipedia.org/wiki/Diode>) can result in all the diodes failing in a fraction of a second.

Yet another example of this effect in a scientific experiment was the [implosion](<https://en.wikipedia.org/wiki/Implosion_(mechanical_process)>) in 2001 of several thousand fragile glass photomultiplier tubes used in the [Super-Kamiokande](<https://en.wikipedia.org/wiki/Super-Kamiokande>) experiment, where the shock wave caused by the failure of a single detector appears to have triggered the implosion of the other detectors in a chain reaction.

### Finance

\[[edit](<https://en.wikipedia.org/w/index.php?title=Cascading_failure&action=edit&section=12>)\]

Main article: [Systemic risk](<https://en.wikipedia.org/wiki/Systemic_risk>)

Main article: [Cascades in financial networks](<https://en.wikipedia.org/wiki/Cascades_in_financial_networks>)

In [finance](<https://en.wikipedia.org/wiki/Finance>), the risk of cascading failures of financial institutions is referred to as *[systemic risk](<https://en.wikipedia.org/wiki/Systemic_risk>):* the failure of one financial institution may cause other financial institutions (its [counterparties](<https://en.wikipedia.org/wiki/Counterparty>)) to fail, cascading throughout the system. Institutions that are believed to pose systemic risk are deemed either "[too big to fail](<https://en.wikipedia.org/wiki/Too_big_to_fail>)" (TBTF) or "too interconnected to fail" (TICTF), depending on why they appear to pose a threat.

Note however that systemic risk is not due to individual institutions per se, but due to the interconnections. Frameworks to study and predict the effects of cascading failures have been developed in the research literature.\[10\]\[11\]\[12\]

A related (though distinct) type of cascading failure in finance occurs in the stock market, exemplified by the [2010 Flash Crash](<https://en.wikipedia.org/wiki/2010_Flash_Crash>).\[12\]

## Interdependent cascading failures

\[[edit](<https://en.wikipedia.org/w/index.php?title=Cascading_failure&action=edit&section=13>)\]

[[image: ](<https://thumb.wikimedia.org/wikipedia/commons/thumb/5/52/Interdependent_relationship_among_different_infrastructures.tif/lossless-page1-250px-Interdependent_relationship_among_different_infrastructures.tif.png?utm_source=en.wikipedia.org&utm_campaign=parser&utm_content=thumbnail>)](<https://en.wikipedia.org/wiki/File:Interdependent_relationship_among_different_infrastructures.tif>)

Illustration of the interdependent relationship among different infrastructures

Diverse [infrastructures](<https://en.wikipedia.org/wiki/Infrastructure>) such as [water supply](<https://en.wikipedia.org/wiki/Water_supply>), [transportation](<https://en.wikipedia.org/wiki/Transportation>), fuel and [power stations](<https://en.wikipedia.org/wiki/Power_station>) are coupled together and depend on each other for functioning, see Fig. 1. Owing to this coupling, interdependent networks are extremely sensitive to random failures, and in particular to [targeted attacks](<https://en.wikipedia.org/wiki/Targeted_threat>), such that a failure of a small fraction of nodes in one network can trigger an iterative cascade of failures in several interdependent networks.\[13\]\[14\] [Electrical blackouts](<https://en.wikipedia.org/wiki/Power_outage>) frequently result from a cascade of failures between interdependent networks, and the problem has been dramatically exemplified by the several large-scale blackouts that have occurred in recent years. Blackouts are a fascinating demonstration of the important role played by the dependencies between networks. For example, the [2003 Italy blackout](<https://en.wikipedia.org/wiki/2003_Italy_blackout>) resulted in a widespread failure of the [railway network](<https://en.wikipedia.org/wiki/Rail_transport>), [health care systems](<https://en.wikipedia.org/wiki/Health_system>), and [financial services](<https://en.wikipedia.org/wiki/Financial_services>) and, in addition, severely influenced the [telecommunication networks](<https://en.wikipedia.org/wiki/Telecommunication_network>). The partial failure of the communication system in turn further impaired the [electrical grid](<https://en.wikipedia.org/wiki/Electrical_grid>) management system, thus producing a positive feedback on the power grid.\[15\] This example emphasizes how inter-dependence can significantly magnify the damage in an interacting network system.

## Model for overload cascading failures

\[[edit](<https://en.wikipedia.org/w/index.php?title=Cascading_failure&action=edit&section=14>)\]

A model for cascading failures due to overload propagation is the Motter–Lai model.\[16\]

## See also

\[[edit](<https://en.wikipedia.org/w/index.php?title=Cascading_failure&action=edit&section=15>)\]

- [Blackouts](<https://en.wikipedia.org/wiki/Power_outage>) – Loss of electric power to an area
- [Brittle system](<https://en.wikipedia.org/wiki/Brittle_system>) – Concept in systems theory
- [Butterfly effect](<https://en.wikipedia.org/wiki/Butterfly_effect>) – Idea that small causes can have large effects
- [Byzantine failure](<https://en.wikipedia.org/wiki/Byzantine_failure>) – Fault in a computer system that presents different symptoms to different observers
- [Cascading rollback](<https://en.wikipedia.org/wiki/Cascading_rollback>) – Database operation that restores a previous state
- [Chain reaction](<https://en.wikipedia.org/wiki/Chain_reaction>) – Self-amplifying chain of events
- [Chaos theory](<https://en.wikipedia.org/wiki/Chaos_theory>) – Field of mathematics and science based on non-linear systems and initial conditions
- [Cache stampede](<https://en.wikipedia.org/wiki/Cache_stampede>) – Parallel computing failure
- [Congestion collapse](<https://en.wikipedia.org/wiki/Congestion_collapse>) – Reduced quality of service due to high network traffic
- [Domino effect](<https://en.wikipedia.org/wiki/Domino_effect>) – Cumulative effect produced when one event sets off a chain of other events
- [For want of a nail](<https://en.wikipedia.org/wiki/For_want_of_a_nail>) – Proverb
- [Network science](<https://en.wikipedia.org/wiki/Network_science>) – Academic field
- [Network theory](<https://en.wikipedia.org/wiki/Network_theory>) – Study of graphs as a representation of relations between discrete objects
- [Interdependent networks](<https://en.wikipedia.org/wiki/Interdependent_networks>) – Subfield of network science
- [Kessler syndrome](<https://en.wikipedia.org/wiki/Kessler_syndrome>) – Theoretical satellite collision cascade
- [Percolation theory](<https://en.wikipedia.org/wiki/Percolation_theory>) – Mathematical theory on behavior of connected clusters in a random graph
- [Progressive collapse](<https://en.wikipedia.org/wiki/Progressive_collapse>) – Building collapse type
- [Virtuous circle and vicious circle](<https://en.wikipedia.org/wiki/Virtuous_circle_and_vicious_circle>) – Self-reinforcing sequence of events
- [Wicked problem](<https://en.wikipedia.org/wiki/Wicked_problem>) – Problem that is difficult or impossible to solve

## References

\[[edit](<https://en.wikipedia.org/w/index.php?title=Cascading_failure&action=edit&section=16>)\]

1. ↑ *Mahmoud, Magdi S.; Xia, Yuanqing (2019). "Cyberphysical Security Methods". *Networked Control Systems*. pp. 389–456. [doi](<https://en.wikipedia.org/wiki/Doi_(identifier)>):[10.1016/B978-0-12-816119-7.00017-4](<https://doi.org/10.1016%2FB978-0-12-816119-7.00017-4>). [ISBN](<https://en.wikipedia.org/wiki/ISBN_(identifier)>) [978-0-12-816119-7](<https://en.wikipedia.org/wiki/Special:BookSources/978-0-12-816119-7>). Cascading failure is kind of failure in a system comprising interconnected parts, in which the failure of a part can trigger the failure of successive parts. Such a failure is common in computer networks and power systems.*
2. ↑ *Farrell, Alexander E.; Zerriffi, Hisham (2004). "Electric Power: Critical Infrastructure Protection". *Encyclopedia of Energy*. pp. 203–215. [doi](<https://en.wikipedia.org/wiki/Doi_(identifier)>):[10.1016/B0-12-176480-X/00516-7](<https://doi.org/10.1016%2FB0-12-176480-X%2F00516-7>). [ISBN](<https://en.wikipedia.org/wiki/ISBN_(identifier)>) [978-0-12-176480-7](<https://en.wikipedia.org/wiki/Special:BookSources/978-0-12-176480-7>). cascading failure: A disruption at one point in a network that causes a disruption other points, leading to a catastrophic system failure.*
3. ↑ *Ulrich, Mike (2016). ["Addressing Cascading Failures"](<https://sre.google/sre-book/addressing-cascading-failures/>). In Murphy, Niall Richard; Beyer, Betsy; Jones, Chris; Petoff, Jennifer (eds.). *Site Reliability Engineering: How Google Runs Production Systems*. O'Reilly. [ISBN](<https://en.wikipedia.org/wiki/ISBN_(identifier)>) [978-1-4919-5117-0](<https://en.wikipedia.org/wiki/Special:BookSources/978-1-4919-5117-0>).*
4. ↑ *Zhai, Chao (2017). "Modeling and Identification of Worst-Case Cascading Failures in Power Systems". [arXiv](<https://en.wikipedia.org/wiki/ArXiv_(identifier)>):[1703.05232](<https://arxiv.org/abs/1703.05232>) \[[cs.SY](<https://arxiv.org/archive/cs.SY>)\].*
5. ↑ *["Why Gmail went down: Google misconfigured load balancing servers (Updated)"](<https://arstechnica.com/information-technology/2012/12/why-gmail-went-down-google-misconfigured-chromes-sync-server/>). 11 December 2012.*
6. ↑ *Petroski, Henry (1992). [*To Engineer Is Human: The Role of Failure in Structural Design*](<https://archive.org/details/toengineerishuma00petr>). Vintage. [ISBN](<https://en.wikipedia.org/wiki/ISBN_(identifier)>) [978-0-679-73416-1](<https://en.wikipedia.org/wiki/Special:BookSources/978-0-679-73416-1>).*\[*[page needed](<https://en.wikipedia.org/wiki/Wikipedia:Citing_sources>)*\]
7. ↑ *Baveye, P; Boast, C W (2017). "Fractal Geometry, Fragmentation Processes and the Physics of Scale-Invariance: An Introduction". In Baveye, Philippe; Parlange, Jean-Yves; Stewart, Bobby A (eds.). *Fractals in Soil Science*. [doi](<https://en.wikipedia.org/wiki/Doi_(identifier)>):[10.1201/9781315151052](<https://doi.org/10.1201%2F9781315151052>). [ISBN](<https://en.wikipedia.org/wiki/ISBN_(identifier)>) [978-1-315-15105-2](<https://en.wikipedia.org/wiki/Special:BookSources/978-1-315-15105-2>).*
8. 1 2 *Heisser, Ronald H.; Patil, Vishal P.; Stoop, Norbert; Villermaux, Emmanuel; Dunkel, Jörn (28 August 2018). ["Controlling fracture cascades through twisting and quenching"](<https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6126751>). *Proceedings of the National Academy of Sciences*. **115** (35): 8665–8670. [arXiv](<https://en.wikipedia.org/wiki/ArXiv_(identifier)>):[1802.05402](<https://arxiv.org/abs/1802.05402>). [Bibcode](<https://en.wikipedia.org/wiki/Bibcode_(identifier)>):[2018PNAS..115.8665H](<https://ui.adsabs.harvard.edu/abs/2018PNAS..115.8665H>). [doi](<https://en.wikipedia.org/wiki/Doi_(identifier)>):[10.1073/pnas.1802831115](<https://doi.org/10.1073%2Fpnas.1802831115>). [PMC](<https://en.wikipedia.org/wiki/PMC_(identifier)>) [6126751](<https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6126751>). [PMID](<https://en.wikipedia.org/wiki/PMID_(identifier)>) [30104353](<https://pubmed.ncbi.nlm.nih.gov/30104353>).*
9. ↑ *Melton, L Joseph; Amin, Shreyasee (26 June 2013). ["Is there a specific fracture 'cascade'?"](<https://www.ncbi.nlm.nih.gov/pmc/articles/PMC3935254>). *BoneKEy Reports*. **2**: 367. [doi](<https://en.wikipedia.org/wiki/Doi_(identifier)>):[10.1038/bonekey.2013.101](<https://doi.org/10.1038%2Fbonekey.2013.101>). [PMC](<https://en.wikipedia.org/wiki/PMC_(identifier)>) [3935254](<https://www.ncbi.nlm.nih.gov/pmc/articles/PMC3935254>). [PMID](<https://en.wikipedia.org/wiki/PMID_(identifier)>) [24575296](<https://pubmed.ncbi.nlm.nih.gov/24575296>).*
10. ↑ *Acemoglu, Daron; Ozdaglar, Asuman; Tahbaz-Salehi, Alireza (February 2015). "Systemic Risk and Stability in Financial Networks". *American Economic Review*. **105** (2): 564–608. [doi](<https://en.wikipedia.org/wiki/Doi_(identifier)>):[10.1257/aer.20130456](<https://doi.org/10.1257%2Faer.20130456>). [hdl](<https://en.wikipedia.org/wiki/Hdl_(identifier)>):[1721.1/100979](<https://hdl.handle.net/1721.1%2F100979>).*
11. ↑ *Gai, Prasanna; Kapadia, Sujit (8 August 2010). "Contagion in financial networks". *Proceedings of the Royal Society A: Mathematical, Physical and Engineering Sciences*. **466** (2120): 2401–2423. [Bibcode](<https://en.wikipedia.org/wiki/Bibcode_(identifier)>):[2010RSPSA.466.2401G](<https://ui.adsabs.harvard.edu/abs/2010RSPSA.466.2401G>). [doi](<https://en.wikipedia.org/wiki/Doi_(identifier)>):[10.1098/rspa.2009.0410](<https://doi.org/10.1098%2Frspa.2009.0410>).*
12. 1 2 *Elliott, Matthew; Golub, Benjamin; Jackson, Matthew O. (October 2014). ["Financial Networks and Contagion"](<https://resolver.caltech.edu/CaltechAUTHORS:20141114-080019333>). *American Economic Review*. **104** (10): 3115–3153. [doi](<https://en.wikipedia.org/wiki/Doi_(identifier)>):[10.1257/aer.104.10.3115](<https://doi.org/10.1257%2Faer.104.10.3115>).*
13. ↑ *["Report of the Commission to Assess the Threat to the United States from Electromagnetic Pulse (EMP) Attack"](<http://empcommission.org/docs/A2473-EMP_Commission-7MB.pdf>) (PDF).*
14. ↑ *Rinaldi, S.M.; Peerenboom, J.P.; Kelly, T.K. (2001). "Identifying, understanding, and analyzing critical infrastructure interdependencies". *IEEE Control Systems Magazine*. **21** (6): 11–25. [Bibcode](<https://en.wikipedia.org/wiki/Bibcode_(identifier)>):[2001ICSys..21f..11R](<https://ui.adsabs.harvard.edu/abs/2001ICSys..21f..11R>). [doi](<https://en.wikipedia.org/wiki/Doi_(identifier)>):[10.1109/37.969131](<https://doi.org/10.1109%2F37.969131>).*
15. ↑ *V. Rosato, Issacharoff, L., Tiriticco, F., Meloni, S., Porcellinis, S.D., & Setola, R. (2008). "Modelling interdependent infrastructures using interacting dynamical models". *International Journal of Critical Infrastructures*. **4** (1–2) 16092: 63–79. [Bibcode](<https://en.wikipedia.org/wiki/Bibcode_(identifier)>):[2008IJCI....4...63R](<https://ui.adsabs.harvard.edu/abs/2008IJCI....4...63R>). [doi](<https://en.wikipedia.org/wiki/Doi_(identifier)>):[10.1504/IJCIS.2008.016092](<https://doi.org/10.1504%2FIJCIS.2008.016092>).*`{{cite journal}}`: CS1 maint: multiple names: authors list ([link](<https://en.wikipedia.org/wiki/Category:CS1_maint:_multiple_names:_authors_list>))
16. ↑ *Motter, Adilson E.; Lai, Ying-Cheng (20 December 2002). "Cascade-based attacks on complex networks". *Physical Review E*. **66** (6) 065102. [arXiv](<https://en.wikipedia.org/wiki/ArXiv_(identifier)>):[cond-mat/0301086](<https://arxiv.org/abs/cond-mat/0301086>). [Bibcode](<https://en.wikipedia.org/wiki/Bibcode_(identifier)>):[2002PhRvE..66f5102M](<https://ui.adsabs.harvard.edu/abs/2002PhRvE..66f5102M>). [doi](<https://en.wikipedia.org/wiki/Doi_(identifier)>):[10.1103/PhysRevE.66.065102](<https://doi.org/10.1103%2FPhysRevE.66.065102>). [PMID](<https://en.wikipedia.org/wiki/PMID_(identifier)>) [12513335](<https://pubmed.ncbi.nlm.nih.gov/12513335>).*

## Further reading

\[[edit](<https://en.wikipedia.org/w/index.php?title=Cascading_failure&action=edit&section=17>)\]

- *Guo, Hengdao; Zheng, Ciyan; Iu, Herbert Ho-Ching; Fernando, Tyrone (December 2017). "A critical review of cascading failure analysis and modeling of power system". *Renewable and Sustainable Energy Reviews*. **80**: 9–22. [Bibcode](<https://en.wikipedia.org/wiki/Bibcode_(identifier)>):[2017RSERv..80....9G](<https://ui.adsabs.harvard.edu/abs/2017RSERv..80....9G>). [doi](<https://en.wikipedia.org/wiki/Doi_(identifier)>):[10.1016/j.rser.2017.05.206](<https://doi.org/10.1016%2Fj.rser.2017.05.206>).*
- *宮崎, 敏幸 (March 2005). *結合相関を持つSFネットワークにおけるカスケード故障に対する防御戦略の比較* \[*Comparison of defense strategies for cascade breakdown on SF networks with degree correlations*\] (Thesis) (in Japanese). [hdl](<https://en.wikipedia.org/wiki/Hdl_(identifier)>):[10119/542](<https://hdl.handle.net/10119%2F542>).*
- *Russ Cooper (1 June 2005). ["(In)Secure Shell?"](<https://web.archive.org/web/20070928164525/http://redmondmag.com/columns/print.asp?EditorialsID=1000>). RedmondMag.com. Archived from [the original](<http://redmondmag.com/columns/print.asp?EditorialsID=1000>) on 2007-09-28. Retrieved 2007-09-08.*
- *US Department of Homeland Security (5 February 2007). ["Cascade Net (simulation program)"](<https://web.archive.org/web/20081228044520/http://www.chds.us/?research%2Fsoftware&d=list>). Center for Homeland Defense and Security. Archived from [the original](<http://www.chds.us/?research/software&d=list>) on 2008-12-28. Retrieved 2007-09-08.*

## External links

\[[edit](<https://en.wikipedia.org/w/index.php?title=Cascading_failure&action=edit&section=18>)\]

- [Space Weather: Blackout — Massive Power Grid Failure](<https://web.archive.org/web/20060827050151/http://www.windows.ucar.edu/spaceweather/blackout.html>)
- [Cascading failure demo applet](<https://web.archive.org/web/20071022110507/http://vlab.infotech.monash.edu.au/simulations/networks/cascading-failure/>) (Monash University's Virtual Lab)
- *Crucitti, Paolo; Latora, Vito; Marchiori, Massimo (29 April 2004). "Model for cascading failures in complex networks". *Physical Review E*. **69** (4) 045104. [arXiv](<https://en.wikipedia.org/wiki/ArXiv_(identifier)>):[cond-mat/0309141](<https://arxiv.org/abs/cond-mat/0309141>). [Bibcode](<https://en.wikipedia.org/wiki/Bibcode_(identifier)>):[2004PhRvE..69d5104C](<https://ui.adsabs.harvard.edu/abs/2004PhRvE..69d5104C>). [doi](<https://en.wikipedia.org/wiki/Doi_(identifier)>):[10.1103/PhysRevE.69.045104](<https://doi.org/10.1103%2FPhysRevE.69.045104>). [PMID](<https://en.wikipedia.org/wiki/PMID_(identifier)>) [15169056](<https://pubmed.ncbi.nlm.nih.gov/15169056>).*
- [Protection Strategies for Cascading Grid Failures — A Shortcut Approach](<https://web.archive.org/web/20040704132003/http://www.epri.com/programHigh.asp?objid=261741>)
- *Dobson, Ian; Carreras, Benjamin A.; Newman, David E. (January 2005). "A Loading-Dependent Model of Probabilistic Cascading Failure". *Probability in the Engineering and Informational Sciences*. **19** (1): 15–32. [doi](<https://en.wikipedia.org/wiki/Doi_(identifier)>):[10.1017/S0269964805050023](<https://doi.org/10.1017%2FS0269964805050023>).*
- [Nova: Crash of Flight 111](<https://www.pbs.org/wgbh/nova/transcripts/3105_aircrash.html>) on September 2, 1998. [Swissair Flight 111](<https://en.wikipedia.org/wiki/Swissair_Flight_111>) flying from New York to Geneva slammed into the Atlantic Ocean off the coast of Nova Scotia with 229 people aboard. Originally believed a terrorist act. After $39 million investigation, insurance settlement of $1.5 billion and more than four years, investigators unravel the puzzle: cascading failure. What is the legacy of Swissair 111? "We have a window into the internal structure of design, checks and balances, protection, and safety." -David Evans, Editor-in-Chief of Air Safety Week.
- PhysicsWeb story: [Accident grounds neutrino lab](<http://physicsweb.org/articles/news/5/11/9>)
- [The Structure and Dynamics of Large Scale Organizational Networks (Dan Braha, New England Complex Systems Institute)](<http://necsi.edu/affiliates/braha/StructureandDynamics.htm>)
- [From Single Network to Network of Networks](<http://havlin.biu.ac.il/Pdf/Bremen070715a.pdf>) [Archived](<https://web.archive.org/web/20151114085329/http://havlin.biu.ac.il/Pdf/Bremen070715a.pdf>) 2015-11-14 at the [Wayback Machine](<https://en.wikipedia.org/wiki/Wayback_Machine>)

Retrieved from "[https://en.wikipedia.org/w/index.php?title=Cascading\_failure&oldid=1362425937](<https://en.wikipedia.org/w/index.php?title=Cascading_failure&oldid=1362425937>)"

[Categories](<https://en.wikipedia.org/wiki/Help:Category>):

- [Failure](<https://en.wikipedia.org/wiki/Category:Failure>)
- [Reliability engineering](<https://en.wikipedia.org/wiki/Category:Reliability_engineering>)
- [Electric power transmission](<https://en.wikipedia.org/wiki/Category:Electric_power_transmission>)
- [Systemic risk](<https://en.wikipedia.org/wiki/Category:Systemic_risk>)
- [Systems science](<https://en.wikipedia.org/wiki/Category:Systems_science>)

Hidden categories:

- [Articles with short description](<https://en.wikipedia.org/wiki/Category:Articles_with_short_description>)
- [Short description is different from Wikidata](<https://en.wikipedia.org/wiki/Category:Short_description_is_different_from_Wikidata>)
- [All articles with unsourced statements](<https://en.wikipedia.org/wiki/Category:All_articles_with_unsourced_statements>)
- [Articles with unsourced statements from January 2009](<https://en.wikipedia.org/wiki/Category:Articles_with_unsourced_statements_from_January_2009>)
- [Wikipedia articles needing page number citations from March 2025](<https://en.wikipedia.org/wiki/Category:Wikipedia_articles_needing_page_number_citations_from_March_2025>)
- [CS1 maint: multiple names: authors list](<https://en.wikipedia.org/wiki/Category:CS1_maint:_multiple_names:_authors_list>)
- [Pages displaying short descriptions of redirect targets via Module:Annotated link](<https://en.wikipedia.org/wiki/Category:Pages_displaying_short_descriptions_of_redirect_targets_via_Module:Annotated_link>)
- [Pages displaying short descriptions with no spaces via Module:Annotated link](<https://en.wikipedia.org/wiki/Category:Pages_displaying_short_descriptions_with_no_spaces_via_Module:Annotated_link>)
- [CS1 Japanese-language sources (ja)](<https://en.wikipedia.org/wiki/Category:CS1_Japanese-language_sources_(ja)>)
- [Webarchive template wayback links](<https://en.wikipedia.org/wiki/Category:Webarchive_template_wayback_links>)


<!-- sllmwiki:original-source -->
## Original source

- URL: <https://en.wikipedia.org/wiki/Cascading_failure>
- License: [CC-BY-SA-4.0](<https://creativecommons.org/licenses/by-sa/4.0/>)
