# ewma-in-software

<!-- sllmwiki:generated: rewritten by wiki-check.py --fix; edit tags.yaml and the manifests instead -->

Using EWMAs in software: load averages, RTT estimation, latency and rate metrics

**Path:** [time-series-smoothing](time-series-smoothing.md) / [moving-averages](moving-averages.md) / ewma-in-software

## Narratives

- [When should software use an EWMA instead of other moving averages?](../narratives/when-should-software-use-an-ewma-instead-of-other-moving-averages.md)

## Sources

- [ewma-rfc6298-tcp-retransmission-timer](../sources/ewma-rfc6298-tcp-retransmission-timer.md): RFC 6298 (IETF Standards Track, June 2011; Paxson, Allman, Chu, Sargent) defines the algorithm TCP senders must use to compute and manage their retransmission timer (RTO), obsoleting RFC 2988 and…
- [ewma-dropwizard-metrics-ewma](../sources/ewma-dropwizard-metrics-ewma.md): EWMA.java from Dropwizard Metrics 4.2.x (Apache-2.0), a widely used Java metrics library, implements an exponentially weighted moving average of an event rate.
- [ewma-linux-kernel-loadavg](../sources/ewma-linux-kernel-loadavg.md): kernel/sched/loadavg.c from the Linux kernel (GPL-2.0) computes the global load average shown by uptime and /proc/loadavg.
