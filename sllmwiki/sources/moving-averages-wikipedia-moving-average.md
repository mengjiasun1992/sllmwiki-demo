# Moving average - Wikipedia

From Wikipedia, the free encyclopedia

For other uses, see [Moving-average model](<https://en.wikipedia.org/wiki/Moving-average_model>) and [Moving average (disambiguation)](<https://en.wikipedia.org/wiki/Moving_average_(disambiguation)>).

[[image: ](<https://thumb.wikimedia.org/wikipedia/commons/thumb/6/6d/Lissage_sinus_bruite_moyenne_glissante.svg/250px-Lissage_sinus_bruite_moyenne_glissante.svg.png?utm_source=en.wikipedia.org&utm_campaign=parser&utm_content=thumbnail>)](<https://en.wikipedia.org/wiki/File:Lissage_sinus_bruite_moyenne_glissante.svg>)

Smoothing of a noisy sine (blue curve) with a moving average (red curve).

In [statistics](<https://en.wikipedia.org/wiki/Statistics>), a **moving average** (**rolling average** or **running average** or **moving mean**\[1\] or **rolling mean**) is a calculation to analyze data points by creating a series of [averages](<https://en.wikipedia.org/wiki/Average>) of different selections of the full data set. Variations include: simple, cumulative, or weighted forms.

Mathematically, a moving average is a type of [convolution](<https://en.wikipedia.org/wiki/Convolution>). Thus in [signal processing](<https://en.wikipedia.org/wiki/Signal_processing>) it is viewed as a [low-pass](<https://en.wikipedia.org/wiki/Low-pass_filter>) [finite impulse response](<https://en.wikipedia.org/wiki/Finite_impulse_response>) filter. Because the [boxcar function](<https://en.wikipedia.org/wiki/Boxcar_function>) outlines its filter coefficients, it is called a **boxcar filter**. It is sometimes followed by [downsampling](<https://en.wikipedia.org/wiki/Downsampling_(signal_processing)>).

Given a series of numbers and a fixed subset size, the first element of the moving average is obtained by taking the average of the initial fixed subset of the number series. Then the subset is modified by "shifting forward"; that is, excluding the first number of the series and including the next value in the series.

A moving average is commonly used with [time series](<https://en.wikipedia.org/wiki/Time_series>) data to smooth out short-term fluctuations and highlight longer-term trends or cycles - in this case the calculation is sometimes called a **time average**. The threshold between short-term and long-term depends on the application, and the parameters of the moving average will be set accordingly. It is also used in [economics](<https://en.wikipedia.org/wiki/Economics>) to examine gross domestic product, employment or other macroeconomic time series. When used with non-time series data, a moving average filters higher frequency components without any specific connection to time, although typically some kind of ordering is implied. Viewed simplistically it can be regarded as smoothing the data.

## History

\[[edit](<https://en.wikipedia.org/w/index.php?title=Moving_average&action=edit&section=1>)\]

The technique of moving averages was invented by the [Bank of England](<https://en.wikipedia.org/wiki/Bank_of_England>) in 1833 to conceal the state of its bullion reserves.\[2\]

## Simple moving average

\[[edit](<https://en.wikipedia.org/w/index.php?title=Moving_average&action=edit&section=2>)\]

[[image: ](<https://thumb.wikimedia.org/wikipedia/commons/thumb/9/93/Moving_Average_Types_comparison_-_Simple_and_Exponential.png/250px-Moving_Average_Types_comparison_-_Simple_and_Exponential.png?utm_source=en.wikipedia.org&utm_campaign=parser&utm_content=thumbnail>)](<https://en.wikipedia.org/wiki/File:Moving_Average_Types_comparison_-_Simple_and_Exponential.png>)

In financial applications a **simple moving average** (**SMA**) is the unweighted [mean](<https://en.wikipedia.org/wiki/Arithmetic_mean>) of the previous  data-points. However, in science and engineering, the mean is normally taken from an equal number of data on either side of a central value. This ensures that variations in the mean are aligned with the variations in the data rather than being shifted in time. An example of a simple equally weighted running mean is the mean over the last  entries of a data-set containing  entries. Let those data-points be . This could be closing prices of a stock. The mean over the last  data-points (days in this example) is denoted as  and calculated as:

When calculating the next mean  with the same sampling width  the range from  to  is considered. A new value  comes into the sum and the oldest value  drops out. This simplifies the calculations by reusing the previous mean .  This means that the moving average filter can be computed quite cheaply on real time data with a FIFO / [circular buffer](<https://en.wikipedia.org/wiki/Circular_buffer>) and only 3 arithmetic steps.

During the initial filling of the FIFO / circular buffer the sampling window is equal to the data-set size thus  and the average calculation is performed as a cumulative moving average.

The period selected () depends on the type of movement of interest, such as short, intermediate, or long-term.

If the data used are not centered around the mean, a simple moving average lags behind the latest datum by half the sample width. An SMA can also be disproportionately influenced by old data dropping out or new data coming in. One characteristic of the SMA is that if the data has a periodic fluctuation, then applying an SMA of that period will eliminate that variation (the average always containing one complete cycle). But a perfectly regular cycle is rarely encountered.\[3\]

For a number of applications, it is advantageous to avoid the shifting induced by using only "past" data. Hence a **central moving average** can be computed, using data equally spaced on either side of the point in the series where the mean is calculated.\[4\] This requires using an odd number of points in the sample window.

A major drawback of the SMA is that it lets through a significant amount of the signal shorter than the window length. This can lead to unexpected artifacts, such as peaks in the smoothed result appearing where there were troughs in the data. It also leads to the result being less smooth than expected since some of the higher frequencies are not properly removed.

Its frequency response is a type of low-pass filter called [sinc-in-frequency](<https://en.wikipedia.org/wiki/Sinc_filter#Frequency-domain_sinc>) (or, in discrete time, is a [Dirichlet kernel](<https://en.wikipedia.org/wiki/Dirichlet_kernel>)).

## Continuous moving average

\[[edit](<https://en.wikipedia.org/w/index.php?title=Moving_average&action=edit&section=3>)\]

The continuous moving average of an integrable function  is defined via integration as:

where the  environment  around  defines the intensity of smoothing of the graph of the integrable function. A larger  smooths the source graph of the function (blue)  more. The animations below show the moving average as animation in dependency of different values for . The fraction  is used, because  is the interval width for the integral. Naturally,  by [fundamental theorem of calculus](<https://en.wikipedia.org/wiki/Fundamental_theorem_of_calculus>) and [L'Hôpital's rule](<https://en.wikipedia.org/wiki/L'Hôpital's_rule>).

- [[image: Continuous moving average sine and polynom - visualization of the smoothing with a small interval for integration](<https://thumb.wikimedia.org/wikipedia/commons/thumb/3/37/Moving_average_sin_polynom_mov_av.gif/250px-Moving_average_sin_polynom_mov_av.gif?utm_source=en.wikipedia.org&utm_campaign=parser&utm_content=thumbnail>)](<https://en.wikipedia.org/wiki/File:Moving_average_sin_polynom_mov_av.gif>)

  Continuous moving average sine and polynom - visualization of the smoothing with a small interval for integration
- [[image: Continuous moving average sine and polynom - visualization of the smoothing with a larger interval for integration](<https://thumb.wikimedia.org/wikipedia/commons/thumb/4/4d/Moving_average_sine_and_polynom_with_a_larger_interval.gif/250px-Moving_average_sine_and_polynom_with_a_larger_interval.gif?utm_source=en.wikipedia.org&utm_campaign=parser&utm_content=thumbnail>)](<https://en.wikipedia.org/wiki/File:Moving_average_sine_and_polynom_with_a_larger_interval.gif>)

  Continuous moving average sine and polynom - visualization of the smoothing with a larger interval for integration
- [[image: Animation showing the impact of interval width and smoothing by moving average.](<https://thumb.wikimedia.org/wikipedia/commons/thumb/5/5c/Moving_average_sine_and_polynom_-_visualization_of_interval_width.gif/250px-Moving_average_sine_and_polynom_-_visualization_of_interval_width.gif?utm_source=en.wikipedia.org&utm_campaign=parser&utm_content=thumbnail>)](<https://en.wikipedia.org/wiki/File:Moving_average_sine_and_polynom_-_visualization_of_interval_width.gif>)

  Animation showing the impact of interval width and smoothing by moving average.

## Cumulative average

\[[edit](<https://en.wikipedia.org/w/index.php?title=Moving_average&action=edit&section=4>)\]

In a **cumulative average** (**CA**), the data arrive in an ordered datum stream, and the user would like to get the average of all of the data up until the current datum. For example, an investor may want the average price of all of the stock transactions for a particular stock up until the current time. As each new transaction occurs, the average price at the time of the transaction can be calculated for all of the transactions up to that point using the cumulative average, typically an equally weighted [average](<https://en.wikipedia.org/wiki/Average>) of the sequence of *n* values  up to the current time:

The brute-force method to calculate this would be to store all of the data and calculate the sum and divide by the number of points every time a new datum arrived. However, it is possible to simply update cumulative average as a new value,  becomes available, using the formula

Thus the current cumulative average for a new datum is equal to the previous cumulative average, times *n*, plus the latest datum, all divided by the number of points received so far, *n*+1. When all of the data arrive (*n* = *N*), then the cumulative average will equal the final average. It is also possible to store a running total of the data as well as the number of points and dividing the total by the number of points to get the CA each time a new datum arrives.

The derivation of the cumulative average formula is straightforward. Using  and similarly for *n* + 1, it is seen that

Solving this equation for  results in

## Weighted moving average

\[[edit](<https://en.wikipedia.org/w/index.php?title=Moving_average&action=edit&section=5>)\]

A weighted average is an average that has multiplying factors to give different weights to data at different positions in the sample window. Mathematically, the weighted moving average is the [convolution](<https://en.wikipedia.org/wiki/Convolution>) of the data with a fixed weighting function. One application is removing [pixelization](<https://en.wikipedia.org/wiki/Pixelization>) from a digital graphical image. This is also known as [Anti-aliasing](<https://en.wikipedia.org/wiki/Anti-aliasing>) \[*[citation needed](<https://en.wikipedia.org/wiki/Wikipedia:Citation_needed>)*\]

In the financial field, and more specifically in the analyses of financial data, a **weighted moving average** (WMA) has the specific meaning of weights that decrease in arithmetical progression.\[5\] In an *n*-day WMA the latest day has weight *n*, the second latest , etc., down to one.

[[image: ](<https://thumb.wikimedia.org/wikipedia/commons/thumb/7/78/Weighted_moving_average_weights_N%3D15.svg/250px-Weighted_moving_average_weights_N%3D15.svg.png?utm_source=en.wikipedia.org&utm_campaign=parser&utm_content=thumbnail>)](<https://en.wikipedia.org/wiki/File:Weighted_moving_average_weights_N=15.svg>)

WMA weights *n* = 15

The denominator is a [triangle number](<https://en.wikipedia.org/wiki/Triangle_number>) equal to  In the more general case the denominator will always be the sum of the individual weights.

When calculating the WMA across successive values, the difference between the numerators of  and  is . If we denote the sum  by , then

The graph at the right shows how the weights decrease, from highest weight for the most recent data, down to zero. It can be compared to the weights in the exponential moving average which follows.

## Exponential moving average

\[[edit](<https://en.wikipedia.org/w/index.php?title=Moving_average&action=edit&section=6>)\]

Main article: [Exponential smoothing](<https://en.wikipedia.org/wiki/Exponential_smoothing>)

Further information: [EWMA chart](<https://en.wikipedia.org/wiki/EWMA_chart>)

An **exponential moving average (EMA)**, also known as an **exponentially weighted moving average (EWMA)**,\[6\] is a first-order [infinite impulse response](<https://en.wikipedia.org/wiki/Infinite_impulse_response>) filter that applies weighting factors which decrease [exponentially](<https://en.wikipedia.org/wiki/Exponential_decay>). The weighting for each older [datum](<https://en.wikipedia.org/wiki/Data>) decreases exponentially, never reaching zero. This formulation is according to [John Stuart Hunter](<https://en.wikipedia.org/wiki/John_Stuart_Hunter>) in a 1986 paper.\[7\]\[8\]

There is also a multivariate implementation of EWMA, known as MEWMA.\[9\]

## Power-law moving average

\[[edit](<https://en.wikipedia.org/w/index.php?title=Moving_average&action=edit&section=7>)\]

One can calculate a moving average where the weights are given by a [power law](<https://en.wikipedia.org/wiki/Power_law>). For example, if we wanted to calculate a trailing average over  we would set  for some . Compared to the exponential moving average, the power-law moving average has the property that very distant-in-the-past points  have similar impacts on the average, since the ratio  goes to zero as . Sometimes, models that use exponential moving averages at multiple lags can be replaced by a single power-law moving average.\[10\]

The naive implementation of a power-law moving average takes  time, but by implementing the kernel with a [fast fourier transform](<https://en.wikipedia.org/wiki/Fast_fourier_transform>) improves runtime to . The exponential moving average maintains the advantage of being able to be updated live in  time however.

## Other weightings

\[[edit](<https://en.wikipedia.org/w/index.php?title=Moving_average&action=edit&section=8>)\]

Other weighting systems are used occasionally – for example, in share trading a **volume weighting** will weight each time period in proportion to its trading volume.

A further weighting, used by actuaries, is Spencer's 15-Point Moving Average\[11\] (a central moving average). Its symmetric weight coefficients are \[−3, −6, −5, 3, 21, 46, 67, 74, 67, 46, 21, 3, −5, −6, −3\], which factors as ⁠\[1, 1, 1, 1\]×\[1, 1, 1, 1\]×\[1, 1, 1, 1, 1\]×\[−3, 3, 4, 3, −3\]/320⁠ and leaves samples of any quadratic or cubic polynomial unchanged.\[12\]\[13\]

Outside the world of finance, weighted running means have many forms and applications. Each weighting function or "kernel" has its own characteristics. In engineering and science the frequency and phase response of the filter is often of primary importance in understanding the desired and undesired distortions that a particular filter will apply to the data.

A mean does not just "smooth" the data. A mean is a form of low-pass filter. The effects of the particular filter used should be understood in order to make an appropriate choice.\[*[citation needed](<https://en.wikipedia.org/wiki/Wikipedia:Citation_needed>)*\]

## Moving median

\[[edit](<https://en.wikipedia.org/w/index.php?title=Moving_average&action=edit&section=9>)\]

From a statistical point of view, the moving average, when used to estimate the underlying trend in a time series, is susceptible to rare events such as rapid shocks or other anomalies. A more robust estimate of the trend is the **simple moving median** over *n* time points:  where the [median](<https://en.wikipedia.org/wiki/Median>) is found by, for example, sorting the values inside the brackets and finding the value in the middle. For larger values of *n*, the median can be efficiently computed by updating an [indexable skiplist](<https://en.wikipedia.org/wiki/Skip_list#Indexable_skiplist>).\[14\]

Statistically, the moving average is optimal for recovering the underlying trend of the time series when the fluctuations about the trend are [normally distributed](<https://en.wikipedia.org/wiki/Normal_distribution>). However, the normal distribution does not place high probability on very large deviations from the trend which explains why such deviations will have a disproportionately large effect on the trend estimate. It can be shown that if the fluctuations are instead assumed to be [Laplace distributed](<https://en.wikipedia.org/wiki/Laplace_distribution>), then the moving median is statistically optimal.\[15\] For a given variance, the Laplace distribution places higher probability on rare events than does the normal, which explains why the moving median tolerates shocks better than the moving mean.

When the simple moving median above is central, the smoothing is identical to the [median filter](<https://en.wikipedia.org/wiki/Median_filter>) which has applications in, for example, image signal processing. The Moving Median is a more robust alternative to the Moving Average when it comes to estimating the underlying trend in a time series. While the Moving Average is optimal for recovering the trend if the fluctuations around the trend are normally distributed, it is susceptible to the impact of rare events such as rapid shocks or anomalies. In contrast, the Moving Median, which is found by sorting the values inside the time window and finding the value in the middle, is more resistant to the impact of such rare events. This is because, for a given variance, the Laplace distribution, which the Moving Median assumes, places higher probability on rare events than the normal distribution that the Moving Average assumes. As a result, the Moving Median provides a more reliable and stable estimate of the underlying trend even when the time series is affected by large deviations from the trend. Additionally, the Moving Median smoothing is identical to the Median Filter, which has various applications in image signal processing.

## Moving average regression model

\[[edit](<https://en.wikipedia.org/w/index.php?title=Moving_average&action=edit&section=10>)\]

Main article: [Moving-average model](<https://en.wikipedia.org/wiki/Moving-average_model>)

In a [moving average regression model](<https://en.wikipedia.org/wiki/Moving_average_model>), a variable of interest is assumed to be a weighted moving average of unobserved independent error terms; the weights in the moving average are parameters to be estimated.

Those two concepts are often confused due to their name, but while they share many similarities, they represent distinct methods and are used in very different contexts.

## See also

\[[edit](<https://en.wikipedia.org/w/index.php?title=Moving_average&action=edit&section=11>)\]

[[image: Wikimedia Commons logo](<https://thumb.wikimedia.org/wikipedia/en/thumb/4/4a/Commons-logo.svg/40px-Commons-logo.svg.png?utm_source=en.wikipedia.org&utm_campaign=parser&utm_content=thumbnail>)](<https://en.wikipedia.org/wiki/File:Commons-logo.svg>)

Wikimedia Commons has media related to [Moving averages](<https://commons.wikimedia.org/wiki/Category:Moving%20averages>).

- [Exponential smoothing](<https://en.wikipedia.org/wiki/Exponential_smoothing>)
- [Local regression](<https://en.wikipedia.org/wiki/Local_regression>) (LOESS and LOWESS)
- [Kernel smoothing](<https://en.wikipedia.org/wiki/Kernel_smoothing>)
- [Moving average convergence/divergence indicator](<https://en.wikipedia.org/wiki/MACD>)
- [Martingale (probability theory)](<https://en.wikipedia.org/wiki/Martingale_(probability_theory)>)
- [Moving average crossover](<https://en.wikipedia.org/wiki/Moving_average_crossover>)
- [Moving least squares](<https://en.wikipedia.org/wiki/Moving_least_squares>)
- [Rising moving average](<https://en.wikipedia.org/wiki/Rising_moving_average>)
- [Rolling hash](<https://en.wikipedia.org/wiki/Rolling_hash>)
- [Running total](<https://en.wikipedia.org/wiki/Running_total>)
- [Savitzky–Golay filter](<https://en.wikipedia.org/wiki/Savitzky–Golay_filter>)
- [Window function](<https://en.wikipedia.org/wiki/Window_function>)
- [Zero lag exponential moving average](<https://en.wikipedia.org/wiki/Zero_lag_exponential_moving_average>)

## References

\[[edit](<https://en.wikipedia.org/w/index.php?title=Moving_average&action=edit&section=12>)\]

1. ↑ [Hydrologic Variability of the Cosumnes River Floodplain](<http://www.waterboards.ca.gov/waterrights/water_issues/programs/bay_delta/docs/cmnt091412/sldmwa/booth_et_al_2006.pdf>) (Booth et al., San Francisco Estuary and Watershed Science, Volume 4, Issue 2, 2006)
2. ↑ *Klein, Judy L. (1997-10-28). *Statistical Visions in Time*. [Cambridge University Press](<https://en.wikipedia.org/wiki/Cambridge_University_Press>). p. 88. [ISBN](<https://en.wikipedia.org/wiki/ISBN_(identifier)>) [9780521420464](<https://en.wikipedia.org/wiki/Special:BookSources/9780521420464>).*
3. ↑ *Statistical Analysis*, Ya-lun Chou, Holt International, 1975, [ISBN](<https://en.wikipedia.org/wiki/ISBN_(identifier)>) [0-03-089422-0](<https://en.wikipedia.org/wiki/Special:BookSources/0-03-089422-0>), section 17.9.
4. ↑ The derivation and properties of the simple central moving average are given in full at [Savitzky–Golay filter](<https://en.wikipedia.org/wiki/Savitzky–Golay_filter>).
5. ↑ *["Weighted Moving Averages: The Basics"](<http://www.investopedia.com/articles/technical/060401.asp>). Investopedia.*
6. ↑ *["DEALING WITH MEASUREMENT NOISE - Averaging Filter"](<https://web.archive.org/web/20100329135531/http://lorien.ncl.ac.uk/ming/filter/filewma.htm>). Archived from [the original](<http://lorien.ncl.ac.uk/ming/filter/filewma.htm>) on 2010-03-29. Retrieved 2010-10-26.*
7. ↑ [NIST/SEMATECH e-Handbook of Statistical Methods: Single Exponential Smoothing](<http://www.itl.nist.gov/div898/handbook/pmc/section4/pmc431.htm>) at the [National Institute of Standards and Technology](<https://en.wikipedia.org/wiki/National_Institute_of_Standards_and_Technology>)
8. ↑ *Hunter, J. Stuart (1986). ["The Exponentially Weighted Moving Average"](<https://www.tandfonline.com/doi/full/10.1080/00224065.1986.11979014>). *Journal of Quality Technology*. **18** (4): 203–210. [doi](<https://en.wikipedia.org/wiki/Doi_(identifier)>):[10.1080/00224065.1986.11979014](<https://doi.org/10.1080%2F00224065.1986.11979014>). [ISSN](<https://en.wikipedia.org/wiki/ISSN_(identifier)>) [0022-4065](<https://search.worldcat.org/issn/0022-4065>).*
9. ↑ *Yeh, A.; Lin, D.; Zhou, H.; Venkataramani, C. (2003). ["A multivariate exponentially weighted moving average control chart for monitoring process variability"](<https://www.stat.purdue.edu/~dkjlin/documents/publications/2003/2003_AppStat1.pdf>) (PDF). *[Journal of Applied Statistics](<https://en.wikipedia.org/wiki/Journal_of_Applied_Statistics>)*. **30** (5): 507–536. [Bibcode](<https://en.wikipedia.org/wiki/Bibcode_(identifier)>):[2003JApSt..30..507Y](<https://ui.adsabs.harvard.edu/abs/2003JApSt..30..507Y>). [doi](<https://en.wikipedia.org/wiki/Doi_(identifier)>):[10.1080/0266476032000053655](<https://doi.org/10.1080%2F0266476032000053655>). [ISSN](<https://en.wikipedia.org/wiki/ISSN_(identifier)>) [0266-4763](<https://search.worldcat.org/issn/0266-4763>). Retrieved 16 January 2025.*
10. ↑ *Bochud, Thierry; Challet, Damien (2007-12-01). ["Optimal approximations of power laws with exponentials: application to volatility models with long memory"](<https://doi.org/10.1080/14697680701278291>). *Quantitative Finance*. **7** (6). Routledge: 585–589. [doi](<https://en.wikipedia.org/wiki/Doi_(identifier)>):[10.1080/14697680701278291](<https://doi.org/10.1080%2F14697680701278291>). [ISSN](<https://en.wikipedia.org/wiki/ISSN_(identifier)>) [1469-7688](<https://search.worldcat.org/issn/1469-7688>).*
11. ↑ [Spencer's 15-Point Moving Average — from Wolfram MathWorld](<http://mathworld.wolfram.com/Spencers15-PointMovingAverage.html>)
12. ↑ Rob J Hyndman. "[Moving averages](<https://robjhyndman.com/papers/movingaverage.pdf>)". 2009-11-08. Accessed 2020-08-20.
13. ↑ Aditya Guntuboyina. "[Statistics 153 (Time Series) : Lecture Three](<https://www.stat.berkeley.edu/~aditya/Site/Statistics_153;_Spring_2012_files/Spring2012Statistics153LectureThree.pdf>)". 2012-01-24. Accessed 2024-01-07.
14. ↑ *["Efficient Running Median using an Indexable Skiplist « Python recipes « ActiveState Code"](<http://code.activestate.com/recipes/576930/>).*
15. ↑ G.R. Arce, "Nonlinear Signal Processing: A Statistical Approach", Wiley:New Jersey, US, 2005.

Retrieved from "[https://en.wikipedia.org/w/index.php?title=Moving\_average&oldid=1380989056](<https://en.wikipedia.org/w/index.php?title=Moving_average&oldid=1380989056>)"

[Categories](<https://en.wikipedia.org/wiki/Help:Category>):

- [Statistical charts and diagrams](<https://en.wikipedia.org/wiki/Category:Statistical_charts_and_diagrams>)
- [Time series](<https://en.wikipedia.org/wiki/Category:Time_series>)
- [Chart overlays](<https://en.wikipedia.org/wiki/Category:Chart_overlays>)
- [Technical analysis](<https://en.wikipedia.org/wiki/Category:Technical_analysis>)

Hidden categories:

- [Articles with short description](<https://en.wikipedia.org/wiki/Category:Articles_with_short_description>)
- [Short description matches Wikidata](<https://en.wikipedia.org/wiki/Category:Short_description_matches_Wikidata>)
- [All articles with unsourced statements](<https://en.wikipedia.org/wiki/Category:All_articles_with_unsourced_statements>)
- [Articles with unsourced statements from February 2018](<https://en.wikipedia.org/wiki/Category:Articles_with_unsourced_statements_from_February_2018>)
- [Articles with unsourced statements from August 2025](<https://en.wikipedia.org/wiki/Category:Articles_with_unsourced_statements_from_August_2025>)
- [Commons category link is on Wikidata](<https://en.wikipedia.org/wiki/Category:Commons_category_link_is_on_Wikidata>)


<!-- sllmwiki:original-source -->
## Original source

- URL: <https://en.wikipedia.org/wiki/Moving_average>
- License: [CC-BY-SA-4.0](<https://creativecommons.org/licenses/by-sa/4.0/>)
