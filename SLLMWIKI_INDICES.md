# Index of this wiki

<!-- sllmwiki:generated: rewritten by wiki-check.py --fix -->

The topics this wiki covers. Each opens a page with its narratives, sources and who to ask.

```mermaid
graph LR
    t0["time-series-smoothing"]
    t1["moving-averages"]
    t2["ewma"]
    t3["ewma-in-software"]
    t0 --> t1
    t1 --> t2
    t1 --> t3
    click t0 "sllmwiki/tags/time-series-smoothing.md"
    click t1 "sllmwiki/tags/moving-averages.md"
    click t2 "sllmwiki/tags/ewma.md"
    click t3 "sllmwiki/tags/ewma-in-software.md"
```

- [time-series-smoothing](sllmwiki/tags/time-series-smoothing.md): Smoothing noisy sequences of measurements over time
  - [moving-averages](sllmwiki/tags/moving-averages.md): Moving averages in general: simple, weighted, cumulative and exponential, and how they compare
    - [ewma](sllmwiki/tags/ewma.md): The exponentially weighted moving average: its recurrence, smoothing factor, bias and memory
    - [ewma-in-software](sllmwiki/tags/ewma-in-software.md): Using EWMAs in software: load averages, RTT estimation, latency and rate metrics
