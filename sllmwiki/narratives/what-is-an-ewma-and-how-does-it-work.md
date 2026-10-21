# What is an EWMA (exponentially weighted moving average), and how does it work?

## 1. Abstract

An EWMA is a running average of a sequence of measurements in which each older measurement counts for less than the one after it, by a constant factor, so the weights fall off exponentially and never quite reach zero. It works through one update rule: the new average is a fraction alpha of the newest observation plus the fraction 1 − alpha of the previous average. That rule keeps only one number of state, yet unrolling it shows that the average is a weighted sum of every past observation, with weights alpha, alpha(1 − alpha), alpha(1 − alpha)², and so on, which sum to almost one. The smoothing factor alpha, between 0 and 1, sets the depth of memory: close to 1, the average follows the latest data and smooths little; close to 0, it remembers far back and smooths a lot. Alpha can be chosen by judgment, from a time constant, or by minimising forecast error. Because the recurrence needs a starting value, the first average must be initialised, by the first observation, a target or the mean of the first few points, and the smaller alpha is, the longer that choice matters. In signal-processing terms an EWMA is a first-order infinite impulse response low-pass filter, unlike the simple moving average, which weights a fixed window equally.

## 2. Terminology

| Term | Definition |
|---|---|
| **EWMA** | Exponentially weighted moving average; also called exponential moving average (EMA) or simple exponential smoothing. |
| **Smoothing factor (alpha, or lambda)** | The weight, between 0 and 1, given to the newest observation in each update. |
| **Recurrence** | A rule that computes each value from the previous one. |
| **SMA** | Simple moving average: the unweighted mean of the last k observations. |
| **IIR / FIR filter** | Infinite / finite impulse response filter: a filter whose output depends on all past inputs, or only on a fixed number of them. |
| **Time constant** | The time an EWMA's response to a step change in its input takes to reach a set fraction of the final value. |
| **[sllmwiki_reasoning]** | The narrative's own reasoning: a conclusion, comparison or reconstruction no cited source states. |
| **[sllmwiki_gap]** | Something the question needs that the sources don't answer. |

## 3. Definition

The NIST/SEMATECH handbook defines the EWMA as "a statistic for monitoring the process that averages the data in a way that gives less and less weight to data as they are further removed in time." [\[1\]](../sources/ewma-nist-ewma-control-charts.md#6324-ewma-control-charts) It computes it as EWMA_t = lambda·Y_t + (1 − lambda)·EWMA_(t−1), where Y_t is the observation at time t and 0 < lambda ≤ 1 "determines the depth of memory of the EWMA". [\[1\]](../sources/ewma-nist-ewma-control-charts.md#6324-ewma-control-charts)

In plain words: keep one running number; each time a new measurement arrives, move that number a fixed fraction of the way towards it. The same idea goes by several names: exponential smoothing, the exponential moving average (EMA) and the EWMA. [\[2\]](../sources/ewma-wikipedia-exponential-smoothing.md#basic-simple-exponential-smoothing) [\[3\]](../sources/moving-averages-wikipedia-moving-average.md#exponential-moving-average)

## 4. Why it matters

A moving average smooths short-term fluctuations in a time series so that longer-term trends stand out. [\[4\]](../sources/moving-averages-wikipedia-moving-average.md#moving-average---wikipedia) The EWMA does this while favouring recent data, which makes it good at noticing change. In process monitoring, a Shewhart chart decides from the latest measurement alone, whereas an EWMA chart decides from an average of all prior data, and by its choice of lambda it "can be made sensitive to a small or gradual drift in the process". [\[1\]](../sources/ewma-nist-ewma-control-charts.md#6324-ewma-control-charts)

A worked example from NIST: a process has a historical mean of 50, used as the starting EWMA, and lambda = 0.3. The first two measurements are 52.0 and 47.0, giving EWMA values of 50.60 and then 49.52; over 20 points it stays within the control limits but rises at the end, hinting at an upward trend. [\[1\]](../sources/ewma-nist-ewma-control-charts.md#6324-ewma-control-charts)

## 5. Key ideas

**One rule, all of the past.** Substituting the rule into itself again and again shows that the average is a weighted sum of every past observation, with weights alpha(1 − alpha)^i that decrease geometrically. [\[5\]](../sources/ewma-nist-single-exponential-smoothing.md#6431-single-exponential-smoothing) A geometric progression is the discrete form of an exponential function, which is where the name comes from. [\[6\]](../sources/ewma-wikipedia-exponential-smoothing.md#exponential-naming) With alpha = 0.3, the four most recent past observations get weights of 0.21, 0.147, 0.103 and 0.072, and the weights sum to 1 − (1 − alpha)^t, which approaches one. [\[5\]](../sources/ewma-nist-single-exponential-smoothing.md#6431-single-exponential-smoothing) The weight of an old observation decreases but never reaches zero. [\[3\]](../sources/moving-averages-wikipedia-moving-average.md#exponential-moving-average)

**Alpha sets the memory.** With lambda = 1 only the latest measurement counts; a larger lambda weights recent data more, a smaller one weights older data more. [\[1\]](../sources/ewma-nist-ewma-control-charts.md#6324-ewma-control-charts) Calling alpha a smoothing factor is something of a misnomer, since larger values smooth less and respond faster to recent changes. [\[2\]](../sources/ewma-wikipedia-exponential-smoothing.md#basic-simple-exponential-smoothing) The time constant links alpha to time: alpha follows from the time constant and the interval between samples, and a simpler approximation holds when that interval is very small next to the time constant. [\[7\]](../sources/ewma-wikipedia-exponential-smoothing.md#time-constant)

**Choosing alpha.** For control charts, lambda is usually set between 0.2 and 0.3, a choice NIST calls "somewhat arbitrary", with published tables to help. [\[1\]](../sources/ewma-nist-ewma-control-charts.md#6324-ewma-control-charts) For forecasting, alpha can be chosen as the value with the smallest mean squared one-step forecast error, by a grid search or a nonlinear optimiser; in NIST's 12-point example, alpha = 0.5 beats alpha = 0.1 (an MSE of 16.29 against 19.0). [\[5\]](../sources/ewma-nist-single-exponential-smoothing.md#6431-single-exponential-smoothing) [\[8\]](../sources/ewma-wikipedia-exponential-smoothing.md#optimization)

**Starting it.** The recurrence needs a previous value, so the first one must be chosen: the first observation, the process target, or an average of the first few observations; the smaller alpha is, the more that choice matters. [\[5\]](../sources/ewma-nist-single-exponential-smoothing.md#6431-single-exponential-smoothing) Starting from the first observation gives it an unreasonably large effect on early values, which averaging ten or more initial periods avoids. [\[9\]](../sources/ewma-wikipedia-exponential-smoothing.md#choosing-the-initial-smoothed-value) The EWMA needs no minimum number of observations before it produces a value, but a good average takes several samples, and for a constant signal the number of steps to reach 95% of its value depends on alpha. [\[2\]](../sources/ewma-wikipedia-exponential-smoothing.md#basic-simple-exponential-smoothing)

**Two indexing conventions.** Hunter's form updates with the previous observation y_(t−1); Roberts' form, used in control charts, with the current one, y_t. [\[5\]](../sources/ewma-nist-single-exponential-smoothing.md#6431-single-exponential-smoothing)

**Less noise.** Once t is not small, the variance of the EWMA is about lambda / (2 − lambda) times the variance of the data, so with lambda = 0.3 it is about 0.18 times as large. [\[1\]](../sources/ewma-nist-ewma-control-charts.md#6324-ewma-control-charts)

## 6. Boundaries

**Not a simple moving average.** An SMA is the unweighted mean of the last k observations, computed over a sliding window. [\[10\]](../sources/moving-averages-wikipedia-moving-average.md#simple-moving-average) An EWMA weights all past observations, with weights that decrease, while an SMA weights only the last k, equally; to keep going, an SMA must keep its last k points, an EWMA only its latest value. [\[11\]](../sources/ewma-wikipedia-exponential-smoothing.md#comparison-with-moving-average) In filter terms, an EWMA is a first-order IIR filter and an SMA a FIR filter with equal weights. [\[11\]](../sources/ewma-wikipedia-exponential-smoothing.md#comparison-with-moving-average) The two give roughly the same forecast error when alpha = 2/(k + 1). [\[11\]](../sources/ewma-wikipedia-exponential-smoothing.md#comparison-with-moving-average)

**Not a weighted moving average (WMA).** In finance a WMA's weights decrease in arithmetic progression over a fixed window, from n for the latest point down to one. [\[12\]](../sources/moving-averages-wikipedia-moving-average.md#weighted-moving-average)

**Not a trend model.** Simple exponential smoothing can't forecast a trend; double (Holt) and triple (Holt–Winters) exponential smoothing add an estimate of the trend so they can. [\[13\]](../sources/ewma-wikipedia-exponential-smoothing.md#exponential-smoothing---wikipedia)

## 7. Where to go next

- When should software use an EWMA instead of other moving averages? ([narrative](when-should-software-use-an-ewma-instead-of-other-moving-averages.md))
- [sllmwiki_gap] How double and triple exponential smoothing forecast trends and seasons is not covered here; it is a question of its own.
- [sllmwiki_gap] Bias-corrected EWMAs, which divide by 1 − (1 − alpha)^t to remove the start-up bias, are not described by the saved sources.

## 8. References

### 8.1 Sources read

- [moving-averages-wikipedia-moving-average](../sources/moving-averages-wikipedia-moving-average.md): what moving averages are; the SMA and WMA to compare with; the EMA as an IIR filter.
- [ewma-wikipedia-exponential-smoothing](../sources/ewma-wikipedia-exponential-smoothing.md): names, alpha's effect, time constant, initialisation, warm-up, comparison with the SMA.
- [ewma-nist-single-exponential-smoothing](../sources/ewma-nist-single-exponential-smoothing.md): the recurrence, its expansion into exponential weights, choosing alpha by MSE, initialisation.
- [ewma-nist-ewma-control-charts](../sources/ewma-nist-ewma-control-charts.md): the definition, lambda as depth of memory, variance, the worked example and drift detection.

### 8.2 Citations

1. [ewma-nist-ewma-control-charts: 6.3.2.4. EWMA Control Charts](../sources/ewma-nist-ewma-control-charts.md#6324-ewma-control-charts)
2. [ewma-wikipedia-exponential-smoothing: Basic (simple) exponential smoothing](../sources/ewma-wikipedia-exponential-smoothing.md#basic-simple-exponential-smoothing)
3. [moving-averages-wikipedia-moving-average: Exponential moving average](../sources/moving-averages-wikipedia-moving-average.md#exponential-moving-average)
4. [moving-averages-wikipedia-moving-average: Moving average - Wikipedia](../sources/moving-averages-wikipedia-moving-average.md#moving-average---wikipedia)
5. [ewma-nist-single-exponential-smoothing: 6.4.3.1. Single Exponential Smoothing](../sources/ewma-nist-single-exponential-smoothing.md#6431-single-exponential-smoothing)
6. [ewma-wikipedia-exponential-smoothing: "Exponential" naming](../sources/ewma-wikipedia-exponential-smoothing.md#exponential-naming)
7. [ewma-wikipedia-exponential-smoothing: Time constant](../sources/ewma-wikipedia-exponential-smoothing.md#time-constant)
8. [ewma-wikipedia-exponential-smoothing: Optimization](../sources/ewma-wikipedia-exponential-smoothing.md#optimization)
9. [ewma-wikipedia-exponential-smoothing: Choosing the initial smoothed value](../sources/ewma-wikipedia-exponential-smoothing.md#choosing-the-initial-smoothed-value)
10. [moving-averages-wikipedia-moving-average: Simple moving average](../sources/moving-averages-wikipedia-moving-average.md#simple-moving-average)
11. [ewma-wikipedia-exponential-smoothing: Comparison with moving average](../sources/ewma-wikipedia-exponential-smoothing.md#comparison-with-moving-average)
12. [moving-averages-wikipedia-moving-average: Weighted moving average](../sources/moving-averages-wikipedia-moving-average.md#weighted-moving-average)
13. [ewma-wikipedia-exponential-smoothing: Exponential smoothing - Wikipedia](../sources/ewma-wikipedia-exponential-smoothing.md#exponential-smoothing---wikipedia)

## 9. Metadata

- **Version:** 1.0
- **Template:** concept_brief_overview
- **Created:** 2026-10-20
- **Last modified:** 2026-10-20
- **History:**
  - 1.0 (2026-10-20): First version.
