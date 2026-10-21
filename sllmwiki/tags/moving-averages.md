# moving-averages

<!-- sllmwiki:generated: rewritten by wiki-check.py --fix; edit tags.yaml and the manifests instead -->

Moving averages in general: simple, weighted, cumulative and exponential, and how they compare

**Path:** [time-series-smoothing](time-series-smoothing.md) / moving-averages

## Subtopics

- [ewma](ewma.md): The exponentially weighted moving average: its recurrence, smoothing factor, bias and memory
- [ewma-in-software](ewma-in-software.md): Using EWMAs in software: load averages, RTT estimation, latency and rate metrics

## Narratives

- [What is an EWMA (exponentially weighted moving average), and how does it work?](../narratives/what-is-an-ewma-and-how-does-it-work.md) (supporting)
- [When should software use an EWMA instead of other moving averages?](../narratives/when-should-software-use-an-ewma-instead-of-other-moving-averages.md) (supporting)

## Sources

- [ewma-nist-single-exponential-smoothing](../sources/ewma-nist-single-exponential-smoothing.md): Section 6.4.3.1 of the NIST/SEMATECH e-Handbook of Statistical Methods introduces single exponential smoothing, which it equates with the EWMA.
- [moving-averages-wikipedia-moving-average](../sources/moving-averages-wikipedia-moving-average.md): The English Wikipedia article 'Moving average' surveys moving averages (rolling, running averages) used to smooth time series and highlight longer-term trends; mathematically a moving average is a convolution, a low-pass…
- [ewma-wikipedia-exponential-smoothing](../sources/ewma-wikipedia-exponential-smoothing.md): The English Wikipedia article 'Exponential smoothing' describes exponential smoothing, or the exponential moving average, which smooths time series with an exponential window function, assigning exponentially decreasing weights over time where…
