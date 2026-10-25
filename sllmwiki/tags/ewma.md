# ewma

<!-- sllmwiki:generated: rewritten by wiki-check.py --fix; edit tags.yaml and the manifests instead -->

The exponentially weighted moving average: its recurrence, smoothing factor, and uses in software

**Path:** [moving-averages](moving-averages.md) / ewma

## Narratives

- [What is an EWMA (exponentially weighted moving average), and how does it work?](../narratives/what-is-an-ewma-exponentially-weighted-moving-average-and-how-does-it-work.md) · 9 min
- [When should software use an EWMA instead of other moving averages?](../narratives/when-should-software-use-an-ewma-instead-of-other-moving-averages.md) · 7 min

## Sources

- [ewma-nist-ewma-control-charts](../sources/ewma-nist-ewma-control-charts.md): Section 6.3.2.4 of the NIST/SEMATECH e-Handbook of Statistical Methods introduces the Exponentially Weighted Moving Average (EWMA) as a process-monitoring statistic that averages data while giving less and less weight to…
- [ewma-nist-single-exponential-smoothing](../sources/ewma-nist-single-exponential-smoothing.md): Section 6.4.3.1 of the NIST/SEMATECH e-Handbook of Statistical Methods explains single exponential smoothing, which forecasts by weighting past observations with exponentially decreasing weights.
- [moving-averages-nist-single-moving-average](../sources/moving-averages-nist-single-moving-average.md): Section 6.4.2.1 of the NIST/SEMATECH e-Handbook of Statistical Methods introduces the single moving average as a smoothing process: instead of summarising all past data at once, it takes the mean…
- [ewma-rfc6298-tcp-retransmission-timer](../sources/ewma-rfc6298-tcp-retransmission-timer.md): RFC 6298 (Paxson, Allman, Chu and Sargent, IETF Standards Track, June 2011) defines the standard algorithm that TCP senders are required to use to compute and manage their retransmission timer,…
- [moving-averages-wikipedia-moving-average](../sources/moving-averages-wikipedia-moving-average.md): The English Wikipedia article on the moving average (rolling, running or moving mean) describes it as a series of averages of different subsets of a data set, used with time…
- [ewma-wikipedia-exponential-smoothing](../sources/ewma-wikipedia-exponential-smoothing.md): The English Wikipedia article on exponential smoothing, or the exponential moving average (EMA), describes it as a technique for smoothing time series with an exponential window function: where a simple…
- [ewma-linkerd-beyond-round-robin](../sources/ewma-linkerd-beyond-round-robin.md): A March 2016 Linkerd (Buoyant) blog post by Steve Jenson and Ruben Oanta argues that load balancing should protect against latency as well as provide scalability and resilience, and compares…
- [ewma-brendan-gregg-linux-load-averages](../sources/ewma-brendan-gregg-linux-load-averages.md): In this August 2017 blog post Brendan Gregg explains Linux load averages, the three 1, 5 and 15 minute numbers shown by uptime, top and /proc/loadavg, and solves the mystery…
- [ewma-fpp3-simple-exponential-smoothing](../sources/ewma-fpp3-simple-exponential-smoothing.md): Chapter 8 of Hyndman and Athanasopoulos's Forecasting: Principles and Practice (3rd edition) presents exponential smoothing, proposed by Brown, Holt and Winters in the late 1950s, whose forecasts are weighted averages…
- [ewma-jacobson-congestion-avoidance](../sources/ewma-jacobson-congestion-avoidance.md): Van Jacobson and Michael Karels, 'Congestion Avoidance and Control' (revised November 1988 from the SIGCOMM '88 paper), the copy on LBL's site.
- [ewma-finch-incremental-weighted-mean-variance](../sources/ewma-finch-incremental-weighted-mean-variance.md): Tony Finch, 'Incremental calculation of weighted mean and variance' (University of Cambridge Computing Service, February 2009).
- [ewma-smith-dspguide-recursive-filters](../sources/ewma-smith-dspguide-recursive-filters.md): Steven W.
- [ewma-linux-kernel-average-h-source](../sources/ewma-linux-kernel-average-h-source.md): The Linux kernel's include/linux/average.h (GPL-2.0), a fixed-precision exponentially weighted moving average implemented as the DECLARE_EWMA(name, precision, weight_rcp) macro.
