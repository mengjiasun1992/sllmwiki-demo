# Thundering herd problem - Wikipedia

From Wikipedia, the free encyclopedia

The **thundering herd problem** is a performance-degrading phenomenon in [computer science](<https://en.wikipedia.org/wiki/Computer_science>) and [computer networking](<https://en.wikipedia.org/wiki/Computer_network>) that occurs when a large number of processes or threads are simultaneously awakened, typically in response to a specific event or the availability of a resource. However, only one process is able to respond to the event or access the new resource, causing most other processes to fail and go back to sleep. This unnecessary awakening consumes CPU and other system resources, potentially reducing overall performance and slowing the process that succeeds.\[1\]

When the thundering herd problem occurs while attempting to access a cache, it is often referred to as a [cache stampede](<https://en.wikipedia.org/wiki/Cache_stampede>).\[2\]

## Mitigation

\[[edit](<https://en.wikipedia.org/w/index.php?title=Thundering_herd_problem&action=edit&section=1>)\]

The [Linux kernel](<https://en.wikipedia.org/wiki/Linux_kernel>) serializes responses for requests to a single [file descriptor](<https://en.wikipedia.org/wiki/File_descriptor>), so only one thread or process is woken up.\[3\] For `epoll()` in version 4.5 of the Linux kernel, the EPOLLEXCLUSIVE flag was added. Thus several epoll sets (different threads or different processes) may wait on the same resource and only one set will be woken up. For certain workloads this flag can give significant processing time reduction.\[4\]

Similarly in Microsoft Windows, [I/O completion ports](<https://en.wikipedia.org/wiki/Input/output_completion_port>) can mitigate the thundering herd problem, as they can be configured such that only one of the threads waiting on the completion port is woken up when an event occurs.\[5\]

In systems that rely on a backoff mechanism (e.g. [exponential backoff](<https://en.wikipedia.org/wiki/Exponential_backoff>)), the clients will retry failed calls by waiting a specific amount of time between consecutive retries. In order to avoid the thundering herd problem, [jitter](<https://en.wikipedia.org/wiki/Jitter>) can be purposefully introduced in order to break the synchronization across the clients, thereby avoiding collisions. In this approach, randomness is added to the wait intervals between retries, so that clients are no longer synchronized.

## See also

\[[edit](<https://en.wikipedia.org/w/index.php?title=Thundering_herd_problem&action=edit&section=2>)\]

- [Process management (computing)](<https://en.wikipedia.org/wiki/Process_management_(computing)>)
- [Lock convoy](<https://en.wikipedia.org/wiki/Lock_convoy>)
- [Sleeping barber problem](<https://en.wikipedia.org/wiki/Sleeping_barber_problem>)
- [TCP global synchronization](<https://en.wikipedia.org/wiki/TCP_global_synchronization>)
- [Cache stampede](<https://en.wikipedia.org/wiki/Cache_stampede>)

## References

\[[edit](<https://en.wikipedia.org/w/index.php?title=Thundering_herd_problem&action=edit&section=3>)\]

1. ↑ *["Thundering Herd Problem"](<http://www.catb.org/jargon/html/T/thundering-herd-problem.html>). *The Jargon File (version 4.4.7)*. Retrieved 9 July 2019.*
2. ↑ *Platform, The Tech (2023-06-06). ["How to prevent cache Stampede ( thundering herd) problems"](<https://www.thetechplatform.com/post/how-to-prevent-cache-stampede-thundering-herd-problems>). *The Tech Platform*. Retrieved 2025-11-10.*
3. ↑ *["Does the Thundering Herd Problem exist on Linux anymore"](<https://stackoverflow.com/a/23002010/56280>). *stackoverflow.com*. Retrieved 2019-07-09.*
4. ↑ *Madars, Vitolins (2015-12-05). ["EPOLLEXCLUSIVE Linux Kernel patch testing"](<https://mvitolin.wordpress.com/2015/12/05/endurox-testing-epollexclusive-flag/>). *mvitolin*. Retrieved 2020-08-11.*
5. ↑ *["IO Completion Ports — Matt Godbolt's blog"](<https://xania.org/200807/iocp>). *xania.org*. Retrieved 2019-01-23.*

## External links

\[[edit](<https://en.wikipedia.org/w/index.php?title=Thundering_herd_problem&action=edit&section=4>)\]

- [A discussion of this observation on Linux](<https://lkml.org/lkml/2004/5/2/108>)
- [Better Retries with Exponential Backoff and Jitter](<https://www.baeldung.com/resilience4j-backoff-jitter>)

Retrieved from "[https://en.wikipedia.org/w/index.php?title=Thundering\_herd\_problem&oldid=1356321785](<https://en.wikipedia.org/w/index.php?title=Thundering_herd_problem&oldid=1356321785>)"

[Category](<https://en.wikipedia.org/wiki/Help:Category>):

- [Concurrency control](<https://en.wikipedia.org/wiki/Category:Concurrency_control>)

Hidden categories:

- [Articles with short description](<https://en.wikipedia.org/wiki/Category:Articles_with_short_description>)
- [Short description matches Wikidata](<https://en.wikipedia.org/wiki/Category:Short_description_matches_Wikidata>)


<!-- sllmwiki:original-source -->
## Original source

- URL: <https://en.wikipedia.org/wiki/Thundering_herd_problem>
- License: [CC-BY-SA-4.0](<https://creativecommons.org/licenses/by-sa/4.0/>)
