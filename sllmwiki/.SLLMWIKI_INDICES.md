# Index of this wiki

<!-- sllmwiki:generated: rewritten by wiki-check.py --fix -->

The topics this wiki covers. Each opens a page with its narratives, sources and who to ask.

```mermaid
graph LR
    t0["distributed-systems"]
    t1["distributed-system-design"]
    t2["calm-theorem"]
    t3["moving-averages"]
    t4["ewma"]
    t5["simple-moving-average"]
    t0 --> t1
    t1 --> t2
    t3 --> t4
    t3 --> t5
    click t0 "tags/distributed-systems.md"
    click t1 "tags/distributed-system-design.md"
    click t2 "tags/calm-theorem.md"
    click t3 "tags/moving-averages.md"
    click t4 "tags/ewma.md"
    click t5 "tags/simple-moving-average.md"
```

- [distributed-systems](tags/distributed-systems.md): Systems of independent networked computers that coordinate by passing messages to appear as one system (2 narratives)
  - [distributed-system-design](tags/distributed-system-design.md): Design tenets and principles for building distributed systems: failure, consistency, scalability, idempotency, backpressure (2 narratives)
    - [calm-theorem](tags/calm-theorem.md): The CALM theorem: consistency as logical monotonicity, which distributed programs need no coordination (2 narratives)
- [moving-averages](tags/moving-averages.md): Moving averages for smoothing a series of measurements over time (1 narrative)
  - [ewma](tags/ewma.md): The exponentially weighted moving average: its recurrence, smoothing factor, and uses in software (2 narratives)
  - [simple-moving-average](tags/simple-moving-average.md): Simple and weighted windowed moving averages, the alternatives to an EWMA (2 narratives)
