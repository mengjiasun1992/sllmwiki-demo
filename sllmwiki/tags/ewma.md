# ewma

<!-- sllmwiki:generated: rewritten by wiki-check.py --fix; edit tags.yaml and the manifests instead -->

The exponentially weighted moving average: its recurrence, smoothing factor, and uses in software

**Path:** [moving-averages](moving-averages.md) / ewma

## Narratives

- [What is an EWMA (exponentially weighted moving average), and how does it work?](../narratives/what-is-an-ewma-exponentially-weighted-moving-average-and-how-does-it-work.md) · 9 min
- [When should software use an EWMA instead of other moving averages?](../narratives/when-should-software-use-an-ewma-instead-of-other-moving-averages.md) · 7 min

## Sources

- [ewma-nist-ewma-control-charts](<https://www.itl.nist.gov/div898/handbook/pmc/section3/pmc324.htm>): Section 6.3.2.4 of the NIST/SEMATECH e-Handbook of Statistical Methods introduces the Exponentially Weighted Moving Average (EWMA) as a process-monitoring statistic that averages data while giving less and less weight to…
- [ewma-nist-single-exponential-smoothing](<https://www.itl.nist.gov/div898/handbook/pmc/section4/pmc431.htm>): Section 6.4.3.1 of the NIST/SEMATECH e-Handbook of Statistical Methods explains single exponential smoothing, which forecasts by weighting past observations with exponentially decreasing weights.
- [moving-averages-nist-single-moving-average](<https://www.itl.nist.gov/div898/handbook/pmc/section4/pmc421.htm>): Section 6.4.2.1 of the NIST/SEMATECH e-Handbook of Statistical Methods introduces the single moving average as a smoothing process: instead of summarising all past data at once, it takes the mean…
- [ewma-rfc6298-tcp-retransmission-timer](<https://www.rfc-editor.org/rfc/rfc6298.txt>): RFC 6298 (Paxson, Allman, Chu and Sargent, IETF Standards Track, June 2011) defines the standard algorithm that TCP senders are required to use to compute and manage their retransmission timer,…
- [moving-averages-wikipedia-moving-average](../sources/moving-averages-wikipedia-moving-average.md): The English Wikipedia article on the moving average (rolling, running or moving mean) describes it as a series of averages of different subsets of a data set, used with time…
- [ewma-wikipedia-exponential-smoothing](../sources/ewma-wikipedia-exponential-smoothing.md): The English Wikipedia article on exponential smoothing, or the exponential moving average (EMA), describes it as a technique for smoothing time series with an exponential window function: where a simple…
- [ewma-linkerd-beyond-round-robin](<https://linkerd.io/2016/03/16/beyond-round-robin-load-balancing-for-latency/>): A March 2016 Linkerd (Buoyant) blog post by Steve Jenson and Ruben Oanta argues that load balancing should protect against latency as well as provide scalability and resilience, and compares…
- [ewma-brendan-gregg-linux-load-averages](<https://www.brendangregg.com/blog/2017-08-08/linux-load-averages.html>): In this August 2017 blog post Brendan Gregg explains Linux load averages, the three 1, 5 and 15 minute numbers shown by uptime, top and /proc/loadavg, and solves the mystery…
- [ewma-fpp3-simple-exponential-smoothing](<https://otexts.com/fpp3/ses.html>): Chapter 8 of Hyndman and Athanasopoulos's Forecasting: Principles and Practice (3rd edition) presents exponential smoothing, proposed by Brown, Holt and Winters in the late 1950s, whose forecasts are weighted averages…
- [ewma-jacobson-congestion-avoidance](<https://ee.lbl.gov/papers/congavoid.pdf>): Van Jacobson and Michael Karels, 'Congestion Avoidance and Control' (revised November 1988 from the SIGCOMM '88 paper), the copy on LBL's site.
- [ewma-finch-incremental-weighted-mean-variance](<https://fanf2.user.srcf.net/hermes/doc/antiforgery/stats.pdf>): Tony Finch, 'Incremental calculation of weighted mean and variance' (University of Cambridge Computing Service, February 2009).
- [ewma-smith-dspguide-recursive-filters](<https://www.dspguide.com/CH19.PDF>): Steven W.
- [ewma-linux-kernel-average-h-source](<https://raw.githubusercontent.com/torvalds/linux/master/include/linux/average.h>): The Linux kernel's include/linux/average.h (GPL-2.0), a fixed-precision exponentially weighted moving average implemented as the DECLARE_EWMA(name, precision, weight_rcp) macro.
- [ewma-finagle-ema-source](../sources/ewma-finagle-ema-source.md): Ema.scala from Twitter's Finagle library (Apache-2.0, pinned to one commit): a small class that maintains an exponential moving average of Double values over a given window on a user-defined clock.
- [ewma-finagle-peak-ewma-source](../sources/ewma-finagle-peak-ewma-source.md): PeakEwma.scala from Twitter's Finagle library (Apache-2.0, pinned to one commit): the load metric behind Finagle's peak EWMA load balancers.
- [ewma-wikipedia-load-computing](../sources/ewma-wikipedia-load-computing.md): Wikipedia's article on load in UNIX computing.
- [ewma-wikipedia-ewma-chart](../sources/ewma-wikipedia-ewma-chart.md): Wikipedia's article on the EWMA chart, a control chart in statistical quality control that monitors variables or attribute data using the whole history of a process's output, each earlier subgroup…
- [ewma-wikipedia-low-pass-filter](../sources/ewma-wikipedia-low-pass-filter.md): Wikipedia's article on low-pass filters, which pass signals below a cutoff frequency and attenuate higher ones, with examples in acoustics, electronics, optics and radio.
- [ewma-finagle-clients-docs](../sources/ewma-finagle-clients-docs.md): The Clients page of Twitter's Finagle user guide (Apache-2.0), describing how a Finagle client is built from a protocol implementation, a transport (with TLS, HTTP and SOCKS5 proxies) and a…
