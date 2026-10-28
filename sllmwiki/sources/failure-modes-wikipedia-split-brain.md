# Split-brain (computing) - Wikipedia

From Wikipedia, the free encyclopedia

| [image: ](<https://thumb.wikimedia.org/wikipedia/en/thumb/b/b4/Ambox_important.svg/40px-Ambox_important.svg.png?utm_source=en.wikipedia.org&utm_campaign=parser&utm_content=thumbnail>) | **This article has multiple issues.** Please help **[improve it](<https://en.wikipedia.org/wiki/Special:EditPage/Split-brain_(computing)>)** or discuss these issues on the **[talk page](<https://en.wikipedia.org/wiki/Talk:Split-brain_(computing)>)**. *([Learn how and when to remove these messages](<https://en.wikipedia.org/wiki/Help:Maintenance_template_removal>))* [image: ](<https://thumb.wikimedia.org/wikipedia/commons/thumb/1/1b/Ambox_question.svg/40px-Ambox_question.svg.png?utm_source=en.wikipedia.org&utm_campaign=parser&utm_content=thumbnail>)This article or section **possibly contains original [synthesis](<https://en.wikipedia.org/wiki/Wikipedia:SYNTH>).** Source material should [verifiably mention](<https://en.wikipedia.org/wiki/Wikipedia:Verifiability>) and [relate](<https://en.wikipedia.org/wiki/Wikipedia:Content_removal#Irrelevant_information>) to the main topic. Relevant discussion may be found on the [talk page](<https://en.wikipedia.org/wiki/Talk:Split-brain_(computing)#>). *(December 2011)**([Learn how and when to remove this message](<https://en.wikipedia.org/wiki/Help:Maintenance_template_removal>))*[[image: icon](<https://thumb.wikimedia.org/wikipedia/en/thumb/9/99/Question_book-new.svg/60px-Question_book-new.svg.png?utm_source=en.wikipedia.org&utm_campaign=parser&utm_content=thumbnail>)](<https://en.wikipedia.org/wiki/File:Question_book-new.svg>)This article **needs [more citations](<https://en.wikipedia.org/wiki/Wikipedia:Verifiability>)**. Please help [improve this article](<https://en.wikipedia.org/wiki/Special:EditPage/Split-brain_(computing)>) by [adding citations to reliable sources](<https://en.wikipedia.org/wiki/Help:Referencing_for_beginners>). Unsourced material may be challenged and [removed](<https://en.wikipedia.org/wiki/Wikipedia:Verifiability#Burden_of_evidence>).\ *Find sources:* ["Split-brain" computing](<https://www.google.com/search?as_eq=wikipedia&q=%22Split-brain%22+computing>) – [news](<https://www.google.com/search?tbm=nws&q=%22Split-brain%22+computing+-wikipedia&tbs=ar:1>) **·** [newspapers](<https://www.google.com/search?&q=%22Split-brain%22+computing&tbs=bkt:s&tbm=bks>) **·** [books](<https://www.google.com/search?tbs=bks:1&q=%22Split-brain%22+computing+-wikipedia>) **·** [scholar](<https://scholar.google.com/scholar?q=%22Split-brain%22+computing>) **·** [JSTOR](<https://www.jstor.org/action/doBasicSearch?Query=%22Split-brain%22+computing&acc=on&wc=on>) *(December 2011)**([Learn how and when to remove this message](<https://en.wikipedia.org/wiki/Help:Maintenance_template_removal>))* *([Learn how and when to remove this message](<https://en.wikipedia.org/wiki/Help:Maintenance_template_removal>))* |
|---|---|

In [computing](<https://en.wikipedia.org/wiki/Computing>), **split-brain** is a state indicating data or availability inconsistencies originating from the maintenance of two separate data sets with overlap in scope, either because of servers in a [network design](<https://en.wikipedia.org/wiki/Network_planning_and_design>), or a failure condition based on servers not communicating and synchronizing their data to each other. This last case is also commonly referred to as a [network partition](<https://en.wikipedia.org/wiki/Network_partition>).\[*[citation needed](<https://en.wikipedia.org/wiki/Wikipedia:Citation_needed>)*\] The name is based on an analogy with the medical [split-brain](<https://en.wikipedia.org/wiki/Split-brain>) syndrome.

## State

\[[edit](<https://en.wikipedia.org/w/index.php?title=Split-brain_(computing)&action=edit&section=1>)\]

Although the term *split-brain* typically refers to an error state, *[split-brain DNS](<https://en.wikipedia.org/wiki/Split-horizon_DNS>)* (or *split-horizon DNS*) is sometimes used to describe a deliberate situation where internal and external [Domain Name System](<https://en.wikipedia.org/wiki/Domain_Name_System>) services (DNS services) for a corporate network are not communicating, so that separate DNS name spaces are to be administered for external computers and for internal ones. This requires a double administration, and if there is domain overlap in the computer names, there is a risk that the same [fully qualified domain name](<https://en.wikipedia.org/wiki/Fully_qualified_domain_name>) (FQDN), may ambiguously occur in both name spaces referring to different computer IP addresses.\[1\]

[High-availability clusters](<https://en.wikipedia.org/wiki/High-availability_cluster>) (HA clusters) usually use a [heartbeat private network](<https://en.wikipedia.org/wiki/Heartbeat_private_network>) connection which is used to monitor the health and status of each node in the cluster. For example, the split-brain syndrome may occur when all of the private links go down simultaneously, but the cluster nodes are still running, each one believing they are the only one running. The data sets of each cluster may then randomly serve clients by their own "idiosyncratic" data set updates, without any coordination with the other data sets. This may lead to [data corruption](<https://en.wikipedia.org/wiki/Data_corruption>) or other data inconsistencies that might require operator intervention and cleanup.

## Approaches for dealing with split-brain

\[[edit](<https://en.wikipedia.org/w/index.php?title=Split-brain_(computing)&action=edit&section=2>)\]

Davidson et al.,\[2\] after surveying several approaches to handle the problem, classify them as either optimistic or pessimistic.

The optimistic approaches simply let the partitioned nodes work as usual; this provides a greater level of availability, at the cost of sacrificing correctness. Once the problem has ended, automatic or manual reconciliation might be required in order to have the cluster in a consistent state. One current implementation for this approach is [Hazelcast](<https://en.wikipedia.org/wiki/Hazelcast>), which does automatic reconciliation of its key-value store.\[3\]

The pessimistic approaches sacrifice availability in exchange for consistency. Once a network partitioning has been detected, access to the sub-partitions is limited in order to guarantee consistency. A typical approach, as described by Coulouris et al.,\[4\] is to use a [quorum](<https://en.wikipedia.org/wiki/Quorum_(distributed_computing)>)-consensus approach. This allows the sub-partition with a majority of the votes to remain available, while the remaining sub-partitions should fall down to an auto-[fencing](<https://en.wikipedia.org/wiki/Fencing_(computing)>) mode. One current implementation for this approach is the one used by [MongoDB](<https://en.wikipedia.org/wiki/MongoDB>) replica sets.\[5\] And another such implementation is Galera replication for [MariaDB](<https://en.wikipedia.org/wiki/MariaDB>) and [MySQL](<https://en.wikipedia.org/wiki/MySQL>).\[6\]

Modern commercial general-purpose [HA clusters](<https://en.wikipedia.org/wiki/High-availability_cluster>) typically use a combination of heartbeat network connections between cluster hosts, and [quorum](<https://en.wikipedia.org/wiki/Quorum_(distributed_computing)>) witness storage. The challenge with two-node clusters is that adding a witness device adds cost and complexity (even if implemented in the cloud), but without it, if heartbeat fails, cluster members cannot determine which should be active. In such clusters (without quorum), if a member fails, even if the members normally assign primary and secondary statuses to the hosts, there is at least a 50% probability that a 2-node HA cluster will totally fail until human intervention is provided, to prevent multiple members becoming active independently and either directly conflicting or corrupting data.

## References

\[[edit](<https://en.wikipedia.org/w/index.php?title=Split-brain_(computing)&action=edit&section=3>)\]

1. ↑ Windows Server 2008 Active Directory, Configuring (2nd Edition) [ISBN](<https://en.wikipedia.org/wiki/ISBN_(identifier)>) [978-0-7356-5193-7](<https://en.wikipedia.org/wiki/Special:BookSources/978-0-7356-5193-7>)
2. ↑ *Davidson, Susan; Garcia-Molina, Hector; Skeen, Dale (1985). "Consistency In A Partitioned Network: A Survey". *ACM Computing Surveys*. **17** (3): 341–370. [doi](<https://en.wikipedia.org/wiki/Doi_(identifier)>):[10.1145/5505.5508](<https://doi.org/10.1145%2F5505.5508>). [hdl](<https://en.wikipedia.org/wiki/Hdl_(identifier)>):[1813/6456](<https://hdl.handle.net/1813%2F6456>). [S2CID](<https://en.wikipedia.org/wiki/S2CID_(identifier)>) [8424228](<https://api.semanticscholar.org/CorpusID:8424228>).*
3. ↑ *["Hazelcast Documentation"](<http://docs.hazelcast.org/docs/3.4/manual/html/networkpartitioning.html>). Retrieved 16 February 2015.*
4. ↑ *Coulouris, George; Dollimore, Jean; Kindberg, Tim (2001). *Distributed systems: concepts and design* (3. ed., 1st, 2nd and 3rd impression. ed.). Harlow \[u.a.\]: Addison-Wesley. [ISBN](<https://en.wikipedia.org/wiki/ISBN_(identifier)>) [0201-61918-0](<https://en.wikipedia.org/wiki/Special:BookSources/0201-61918-0>).*
5. ↑ *["MongoDB Replication Fundamentals"](<http://docs.mongodb.org/manual/core/replication/>). Retrieved 12 December 2012.*
6. ↑ *["Weighted Quorum in Galera Cluster"](<http://galeracluster.com/documentation-webpages/weightedquorum.html>). Retrieved 17 December 2015.*

Retrieved from "[https://en.wikipedia.org/w/index.php?title=Split-brain\_(computing)&oldid=1371632674](<https://en.wikipedia.org/w/index.php?title=Split-brain_(computing)&oldid=1371632674>)"

[Category](<https://en.wikipedia.org/wiki/Help:Category>):

- [High-availability cluster computing](<https://en.wikipedia.org/wiki/Category:High-availability_cluster_computing>)

Hidden categories:

- [Articles with short description](<https://en.wikipedia.org/wiki/Category:Articles_with_short_description>)
- [Short description matches Wikidata](<https://en.wikipedia.org/wiki/Category:Short_description_matches_Wikidata>)
- [Articles that may contain original research from December 2011](<https://en.wikipedia.org/wiki/Category:Articles_that_may_contain_original_research_from_December_2011>)
- [Articles needing additional references from December 2011](<https://en.wikipedia.org/wiki/Category:Articles_needing_additional_references_from_December_2011>)
- [All articles needing additional references](<https://en.wikipedia.org/wiki/Category:All_articles_needing_additional_references>)
- [Articles with multiple maintenance issues](<https://en.wikipedia.org/wiki/Category:Articles_with_multiple_maintenance_issues>)
- [All articles with unsourced statements](<https://en.wikipedia.org/wiki/Category:All_articles_with_unsourced_statements>)
- [Articles with unsourced statements from October 2025](<https://en.wikipedia.org/wiki/Category:Articles_with_unsourced_statements_from_October_2025>)


<!-- sllmwiki:original-source -->
## Original source

- URL: <https://en.wikipedia.org/wiki/Split-brain_(computing)>
- License: [CC-BY-SA-4.0](<https://creativecommons.org/licenses/by-sa/4.0/>)
