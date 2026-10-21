# When should software use an EWMA instead of other moving averages?

## 1. Abstract

Use an EWMA when software has to keep a running estimate of a noisy quantity that changes over time, such as a round-trip time, a request rate or a system load, and the estimate must be cheap to update and to store, react to recent change, and need no window of history. An EWMA keeps one number per estimate and updates it with one multiply-add per sample, where a simple moving average (SMA) of k points must keep those k points. That is why the TCP retransmission timer, the Linux load average and the Dropwizard Metrics library all use EWMAs: TCP smooths every connection's round-trip time and its variation with alpha = 1/8 and beta = 1/4; Linux and Dropwizard keep 1-, 5- and 15-minute averages from a single input; and Linux catches up over intervals it skipped in one closed-form step. Choose alpha from the time window you want, as Dropwizard does with alpha = 1 − exp(−interval/window). Prefer another average when its shape matters more than its cost: an SMA when you need a hard window that forgets old data completely, or that cancels a cycle of known period; a cumulative average when every sample since the start should count equally; a centred or symmetric filter when output must not lag, since an EWMA's lag can't be removed by a fixed shift. Mind the start-up: initialise from the first sample or a known target, and expect the estimate to take several samples to settle.

## 2. Terminology

| Term | Definition |
|---|---|
| **EWMA** | Exponentially weighted moving average: s_t = s_(t−1) + alpha·(x_t − s_(t−1)), where x_t is the sample at step t and s_t the average. |
| **alpha** | The smoothing factor, 0 < alpha ≤ 1: the share of each new sample's difference from the average that the average takes in. |
| **SMA** | Simple moving average: the unweighted mean of the last k samples. |
| **WMA** | Weighted moving average: a mean over a fixed window with fixed, unequal weights. |
| **Cumulative average** | The mean of every sample since the start. |
| **RTT, SRTT, RTTVAR, RTO** | In TCP: round-trip time, its smoothed value, its smoothed variation, and the retransmission timeout computed from them. |
| **Tick** | One periodic update of a time-based EWMA. |
| **Time window (tau)** | The time scale an EWMA averages over, such as one minute; with a tick every delta-t, alpha = 1 − exp(−delta-t / tau). |
| **[sllmwiki_reasoning]** | The narrative's own reasoning: a conclusion, comparison or reconstruction no cited source states. |
| **[sllmwiki_gap]** | Something the question needs that the sources don't answer. |

## 3. Background

Software often watches a quantity that is noisy and drifts. A TCP sender must decide how long to wait for an acknowledgement before it retransmits: too short and it resends needlessly, too long and it stalls. It measures round-trip times, but each measurement varies, and the path's real delay changes over time. RFC 6298, the standard for TCP's retransmission timer, solves this by keeping a smoothed round-trip time and a smoothed variation for each connection, both EWMAs, and setting the timeout from them. [\[1\]](../sources/ewma-rfc6298-tcp-retransmission-timer.md)

Every moving average smooths short-term fluctuations to show the longer-term trend; they differ in how they weight the past. [\[2\]](../sources/moving-averages-wikipedia-moving-average.md#moving-average---wikipedia) What an EWMA is, and how its weights fall off exponentially, is the subject of [What is an EWMA (exponentially weighted moving average), and how does it work?](what-is-an-ewma-and-how-does-it-work.md).

## 4. How does it work?

In software, an EWMA is one stored number, updated in place on each sample or each tick: move the average a fraction alpha of the way towards the new sample. Nothing else is kept.

```mermaid
graph LR
    A[New sample x_t] --> B[Difference x_t − s_t−1]
    B --> C[s_t = s_t−1 + alpha · difference]
    C --> D[Read s_t, or derive a decision from it]
```

1. **A sample arrives.** It may be one measurement, as each RTT measurement in TCP [\[1\]](../sources/ewma-rfc6298-tcp-retransmission-timer.md), or a count over a tick: Dropwizard counts events between ticks and divides by the tick interval to get an instant rate. [\[3\]](../sources/ewma-dropwizard-metrics-ewma.md)
2. **Take the difference** between the sample and the current average.
3. **Update in place.** Dropwizard computes rate = oldRate + alpha·(instantRate − oldRate) [\[3\]](../sources/ewma-dropwizard-metrics-ewma.md); TCP computes SRTT = (1 − alpha)·SRTT + alpha·R', with alpha = 1/8. [\[1\]](../sources/ewma-rfc6298-tcp-retransmission-timer.md) [sllmwiki_reasoning] The two forms are the same equation rearranged; steps 2 and 3 are one line of code.
4. **Use it.** TCP sets the timeout to SRTT + max(G, 4·RTTVAR), where G is the clock granularity, rounded up to at least one second [\[1\]](../sources/ewma-rfc6298-tcp-retransmission-timer.md); Dropwizard reports the rate in any time unit. [\[3\]](../sources/ewma-dropwizard-metrics-ewma.md)

**Variant: tracking the spread too.** TCP keeps a second EWMA, RTTVAR, of the absolute difference between SRTT and each new RTT, with beta = 1/4, and must update it before SRTT. [\[1\]](../sources/ewma-rfc6298-tcp-retransmission-timer.md)

**Variant: several time scales.** The Linux load average is "an exponentially decaying average" of the number of running and uninterruptible tasks, kept as three averages with different decay constants (EXP_1, EXP_5, EXP_15) for 1, 5 and 15 minutes, all fed by the same count. [\[4\]](../sources/ewma-linux-kernel-loadavg.md) Dropwizard offers the same three windows, ticked every five seconds. [\[3\]](../sources/ewma-dropwizard-metrics-ewma.md)

## 5. Design decisions and trade-offs

**EWMA or SMA: memory and cost.** An SMA of k points must keep the past k points (or the point at lag k + 1), while an EWMA keeps only its latest value. [\[5\]](../sources/ewma-wikipedia-exponential-smoothing.md#comparison-with-moving-average) An SMA can be updated cheaply with a circular buffer, but the buffer still holds k values. [\[6\]](../sources/moving-averages-wikipedia-moving-average.md#simple-moving-average) [sllmwiki_reasoning] The difference grows with the number of estimates: one per TCP connection, or three per metric, makes a window of history per estimate expensive, and a single number cheap. The EWMA wins when you keep many estimates, need long windows, or update on hot paths; the SMA's cost is acceptable for a few estimates over short windows.

**EWMA or SMA: how the past is weighted.** An EWMA weights all past data, with weights that decrease, while an SMA weights only the last k points, equally. [\[5\]](../sources/ewma-wikipedia-exponential-smoothing.md#comparison-with-moving-average) An SMA can be disproportionately moved by old data dropping out of its window, and it lets through much of the signal shorter than its window, which can put peaks where the data had troughs. [\[6\]](../sources/moving-averages-wikipedia-moving-average.md#simple-moving-average) An SMA does have one property an EWMA lacks: an SMA whose window matches a periodic fluctuation removes it completely. [\[6\]](../sources/moving-averages-wikipedia-moving-average.md#simple-moving-average) [sllmwiki_reasoning] Choose the SMA when you need a hard window, where data older than k samples must stop counting entirely, such as a limit defined over "the last 100 requests", or when you must cancel a known cycle.

**Matching an SMA's window.** The two have roughly the same forecast error when alpha = 2/(k + 1), with k the SMA's window. [\[5\]](../sources/ewma-wikipedia-exponential-smoothing.md#comparison-with-moving-average) [sllmwiki_reasoning] Rule of thumb: to replace a 15-point SMA, start from alpha = 2/16 = 0.125.

**Choosing alpha from time.** For a time-based EWMA, derive alpha from the tick interval and the window you want: Dropwizard uses alpha = 1 − exp(−interval/window), so a one-minute EWMA ticked every 5 seconds has alpha = 1 − exp(−5/60). [\[3\]](../sources/ewma-dropwizard-metrics-ewma.md) A larger alpha follows recent data and smooths less; a smaller one smooths more and responds more slowly. [\[7\]](../sources/ewma-wikipedia-exponential-smoothing.md#basic-simple-exponential-smoothing) [sllmwiki_reasoning] Too high, and the estimate jitters with every sample; too low, and it lags real change. For values from standards, TCP uses alpha = 1/8 for the mean and beta = 1/4 for the variation [\[1\]](../sources/ewma-rfc6298-tcp-retransmission-timer.md); quality-control charts usually use 0.2 to 0.3. [\[8\]](../sources/ewma-nist-ewma-control-charts.md#6324-ewma-control-charts) Alpha can also be fitted by minimising one-step forecast errors when you have data. [\[9\]](../sources/ewma-wikipedia-exponential-smoothing.md#optimization)

**EWMA or cumulative average.** A cumulative average gives every sample since the start equal weight and can be updated incrementally. [\[10\]](../sources/moving-averages-wikipedia-moving-average.md#cumulative-average) [sllmwiki_reasoning] It costs as little as an EWMA but never forgets, so it suits totals over a whole run (the mean latency of a batch job) and not a quantity that drifts.

**EWMA or WMA.** A WMA uses fixed weights over a fixed window; in finance they fall in arithmetic progression. [\[11\]](../sources/moving-averages-wikipedia-moving-average.md#weighted-moving-average) [sllmwiki_reasoning] It favours recent data like an EWMA but needs the window's history like an SMA, so in software it rarely beats either.

**Detecting drift.** By its choice of lambda, an EWMA can detect a small or gradual drift that a check on the latest value alone would miss. [\[8\]](../sources/ewma-nist-ewma-control-charts.md#6324-ewma-control-charts) Its variance is about lambda/(2 − lambda) times the data's, so thresholds on it can be set tighter than on raw samples. [\[8\]](../sources/ewma-nist-ewma-control-charts.md#6324-ewma-control-charts) [sllmwiki_reasoning] This suits alerting on latency or error rates.

## 6. Limitations and failure modes

**Lag that can't be shifted away.** Both an EWMA and an SMA lag the input; a symmetric filter's lag can be corrected by shifting by half its window, but an EWMA's can't, because its delay depends on frequency. [\[5\]](../sources/ewma-wikipedia-exponential-smoothing.md#comparison-with-moving-average) When output must line up with input, as in offline analysis, use a centred moving average instead. [\[6\]](../sources/moving-averages-wikipedia-moving-average.md#simple-moving-average)

**Start-up.** An EWMA needs a starting value: initialising from the first observation gives that value an unreasonably large effect on early estimates, and the smaller alpha is, the more the choice matters. [\[12\]](../sources/ewma-wikipedia-exponential-smoothing.md#choosing-the-initial-smoothed-value) Dropwizard and TCP both start from the first sample (SRTT = R, RTTVAR = R/2) [\[3\]](../sources/ewma-dropwizard-metrics-ewma.md) [\[1\]](../sources/ewma-rfc6298-tcp-retransmission-timer.md); TCP uses a one-second timeout until it has a measurement. [\[1\]](../sources/ewma-rfc6298-tcp-retransmission-timer.md) [sllmwiki_reasoning] Starting at zero instead makes the estimate read low for its first few windows.

**Missed samples.** Every sample must be applied, because no sample's weight ever falls to zero; an SMA tolerates skipped samples better. [\[7\]](../sources/ewma-wikipedia-exponential-smoothing.md#basic-simple-exponential-smoothing) For time-based EWMAs with no input during a gap, there is a closed form: Linux, when ticks were missed for n intervals on a tickless kernel, applies them in one step by raising the decay factor to the nth power, a_n = a_0·e^n + a·(1 − e^n), in O(log n) time. [\[4\]](../sources/ewma-linux-kernel-loadavg.md) Dropwizard's reset() avoids calling tick a large number of times. [\[3\]](../sources/ewma-dropwizard-metrics-ewma.md)

**Not a forecast of trends.** Simple exponential smoothing can't forecast a trend; double and triple exponential smoothing add one. [\[13\]](../sources/ewma-wikipedia-exponential-smoothing.md#exponential-smoothing---wikipedia)

[sllmwiki_gap] The saved sources measure no CPU or memory costs, and none covers EWMAs over irregularly spaced samples, beyond Linux's catch-up for whole missed intervals.

## 7. References

### 7.1 Sources read

- [what-is-an-ewma-and-how-does-it-work](what-is-an-ewma-and-how-does-it-work.md): background on the EWMA itself.
- [ewma-nist-ewma-control-charts](../sources/ewma-nist-ewma-control-charts.md): drift detection, typical lambda, variance.
- [moving-averages-wikipedia-moving-average](../sources/moving-averages-wikipedia-moving-average.md): the SMA, cumulative and weighted averages and their drawbacks.
- [ewma-wikipedia-exponential-smoothing](../sources/ewma-wikipedia-exponential-smoothing.md): comparison with the SMA, memory, lag, alpha, start-up, missed samples.
- [ewma-rfc6298-tcp-retransmission-timer](../sources/ewma-rfc6298-tcp-retransmission-timer.md): TCP's SRTT and RTTVAR (section 2, "The Basic Algorithm").
- [ewma-dropwizard-metrics-ewma](../sources/ewma-dropwizard-metrics-ewma.md): rate EWMAs, alpha from a time window, initialisation.
- [ewma-linux-kernel-loadavg](../sources/ewma-linux-kernel-loadavg.md): the load average, three time scales, catch-up over missed intervals.

### 7.2 Citations

1. [ewma-rfc6298-tcp-retransmission-timer: section 2, The Basic Algorithm](../sources/ewma-rfc6298-tcp-retransmission-timer.md)
2. [moving-averages-wikipedia-moving-average: Moving average - Wikipedia](../sources/moving-averages-wikipedia-moving-average.md#moving-average---wikipedia)
3. [ewma-dropwizard-metrics-ewma: class EWMA](../sources/ewma-dropwizard-metrics-ewma.md)
4. [ewma-linux-kernel-loadavg: the global load-average comment, calc_load_n and calc_global_load](../sources/ewma-linux-kernel-loadavg.md)
5. [ewma-wikipedia-exponential-smoothing: Comparison with moving average](../sources/ewma-wikipedia-exponential-smoothing.md#comparison-with-moving-average)
6. [moving-averages-wikipedia-moving-average: Simple moving average](../sources/moving-averages-wikipedia-moving-average.md#simple-moving-average)
7. [ewma-wikipedia-exponential-smoothing: Basic (simple) exponential smoothing](../sources/ewma-wikipedia-exponential-smoothing.md#basic-simple-exponential-smoothing)
8. [ewma-nist-ewma-control-charts: 6.3.2.4. EWMA Control Charts](../sources/ewma-nist-ewma-control-charts.md#6324-ewma-control-charts)
9. [ewma-wikipedia-exponential-smoothing: Optimization](../sources/ewma-wikipedia-exponential-smoothing.md#optimization)
10. [moving-averages-wikipedia-moving-average: Cumulative average](../sources/moving-averages-wikipedia-moving-average.md#cumulative-average)
11. [moving-averages-wikipedia-moving-average: Weighted moving average](../sources/moving-averages-wikipedia-moving-average.md#weighted-moving-average)
12. [ewma-wikipedia-exponential-smoothing: Choosing the initial smoothed value](../sources/ewma-wikipedia-exponential-smoothing.md#choosing-the-initial-smoothed-value)
13. [ewma-wikipedia-exponential-smoothing: Exponential smoothing - Wikipedia](../sources/ewma-wikipedia-exponential-smoothing.md#exponential-smoothing---wikipedia)

## 8. Metadata

- **Version:** 1.0
- **Template:** technical_brief_overview
- **Created:** 2026-10-20
- **Last modified:** 2026-10-20
- **History:**
  - 1.0 (2026-10-20): First version.
