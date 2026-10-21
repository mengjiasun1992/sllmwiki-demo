# Exponential smoothing - Wikipedia

From Wikipedia, the free encyclopedia

**Exponential smoothing** or **exponential moving average (EMA)** is a technique for [smoothing](<https://en.wikipedia.org/wiki/Smoothing>) [time series](<https://en.wikipedia.org/wiki/Time_series>) data using the exponential [window function](<https://en.wikipedia.org/wiki/Window_function>). Whereas in the [simple moving average](<https://en.wikipedia.org/wiki/Simple_moving_average>) the past observations are [weighted](<https://en.wikipedia.org/wiki/Weight_function>) equally, exponential functions are used to assign exponentially decreasing weights over time. It is an easily learned and easily applied procedure for making some determination based on prior assumptions by the user, such as seasonality. Exponential smoothing is often used for analysis of time-series data.

Exponential smoothing is one of many [window functions](<https://en.wikipedia.org/wiki/Window_functions>) commonly applied to smooth data in [signal processing](<https://en.wikipedia.org/wiki/Signal_processing>), acting as [low-pass filters](<https://en.wikipedia.org/wiki/Low-pass_filter>) to remove high-frequency [noise](<https://en.wikipedia.org/wiki/Noise>). This method is preceded by [Poisson](<https://en.wikipedia.org/wiki/Siméon_Denis_Poisson>)'s use of recursive exponential window functions in convolutions from the 19th century, as well as [Kolmogorov and Zurbenko's use of recursive moving averages](<https://en.wikipedia.org/wiki/Kolmogorov–Zurbenko_filter>) from their studies of turbulence in the 1940s.

The raw data sequence is often represented by  beginning at time , and the output of the exponential smoothing algorithm is commonly written as , which may be regarded as a best estimate of what the next value of  will be. When the sequence of observations begins at time , the simplest form of exponential smoothing is given by the following formulas:\[1\]

where  is the *smoothing factor*, and . If  is substituted into  continuously so that the formula of  is fully expressed in terms of , then exponentially decaying weighting factors on each raw data  is revealed, showing how exponential smoothing is named.

The simple exponential smoothing is not able to predict what would be observed at  based on the raw data up to , while the double exponential smoothing and triple exponential smoothing can be used for the prediction due to the presence of  as the sequence of best estimates of the linear trend.

## Basic (simple) exponential smoothing

\[[edit](<https://en.wikipedia.org/w/index.php?title=Exponential_smoothing&action=edit&section=1>)\]

The use of the exponential window function is first attributed to [Poisson](<https://en.wikipedia.org/wiki/Siméon_Denis_Poisson>)\[2\] as an extension of a numerical analysis technique from the 17th century, and later adopted by the [signal processing](<https://en.wikipedia.org/wiki/Signal_processing>) community in the 1940s. Here, exponential smoothing is the application of the exponential, or Poisson, [window function](<https://en.wikipedia.org/wiki/Window_function>). Exponential smoothing was suggested in the statistical literature without citation to previous work by [Robert Goodell Brown](<https://en.wikipedia.org/wiki/Robert_Goodell_Brown>) in 1956,\[3\] and expanded by [Charles C. Holt](<https://en.wikipedia.org/wiki/Charles_C._Holt>) in 1957.\[4\] The formulation below, which is the one commonly used, is attributed to Brown and is known as "Brown’s simple exponential smoothing".\[5\] All the methods of Holt, Winters, and Brown may be seen as a simple application of [recursive filtering](<https://en.wikipedia.org/wiki/Recursive_filter>), first found in the 1940s\[2\] to convert [finite impulse response](<https://en.wikipedia.org/wiki/Finite_impulse_response>) (FIR) filters to [infinite impulse response](<https://en.wikipedia.org/wiki/Infinite_impulse_response>) filters.

The simplest form of exponential smoothing is given by the formula:

where  is the *smoothing factor*, with . In other words, the smoothed statistic  is a simple weighted average of the current [observation](<https://en.wikipedia.org/wiki/Random_variate>)  and the previous smoothed statistic . Simple exponential smoothing is easily applied, and it produces a smoothed statistic as soon as two observations are available. The term *smoothing factor* applied to  here is something of a misnomer, as larger values of  actually reduce the level of smoothing, and in the limiting case with  = 1 the smoothing output series is just the current observation. Values of  close to 1 have less of a smoothing effect and give greater weight to recent changes in the data, while values of  closer to 0 have a greater smoothing effect and are less responsive to recent changes. In the limiting case with  = 0, the output series is just flat or a constant as the observation  at the beginning of the smoothening process .

The method for choosing  must be decided by the modeler. Sometimes the statistician's judgment is used to choose an appropriate factor. Alternatively, a statistical technique may be used to *optimize* the value of . For example, the [method of least squares](<https://en.wikipedia.org/wiki/Least_squares>) might be used to determine the value of  for which the sum of the quantities  is minimized.\[6\]

Unlike some other smoothing methods, such as the simple moving average, this technique does not require any minimum number of observations to be made before it begins to produce results. In practice, however, a "good average" will not be achieved until several samples have been averaged together; for example, a constant signal will take approximately  stages to reach 95% of the actual value. To accurately reconstruct the original signal without information loss, all stages of the exponential moving average must also be available, because older samples decay in weight exponentially. This is in contrast to a simple moving average, in which some samples can be skipped without as much loss of information due to the constant weighting of samples within the average. If a known number of samples will be missed, one can adjust a weighted average for this as well, by giving equal weight to the new sample and all those to be skipped.

This simple form of exponential smoothing is also known as an [exponentially weighted moving average](<https://en.wikipedia.org/wiki/Moving_average#Exponential_moving_average>) (EWMA). Technically it can also be classified as an [autoregressive integrated moving average](<https://en.wikipedia.org/wiki/Autoregressive_integrated_moving_average>) (ARIMA) (0,1,1) model with no constant term.\[7\]

### Time constant

\[[edit](<https://en.wikipedia.org/w/index.php?title=Exponential_smoothing&action=edit&section=2>)\]

The [time constant](<https://en.wikipedia.org/wiki/Time_constant>) of an exponential moving average is the amount of time for the smoothed response of a [unit step function](<https://en.wikipedia.org/wiki/Unit_step_function>) to reach  of the final value. The relationship between this time constant, , and the smoothing factor, , is given by the following formula:

, thus

where  is the sampling time interval of the discrete time implementation.

Some math libraries provide the function `expm1(x) = exp(x) - 1` to provide more accurate results when `x` is close to 0 and this may be used directly to compute

Similarly the inverse function `log1p(x) = log(1+x)` may be used to compute the time constant from the smoothing factor:

But if the sampling time is very small compared to the time constant () then, by using [the Taylor expansion of the exponential function](<https://en.wikipedia.org/wiki/Taylor_series#Examples>),

, thus

### Choosing the initial smoothed value

\[[edit](<https://en.wikipedia.org/w/index.php?title=Exponential_smoothing&action=edit&section=3>)\]

Note that in the definition above,  (the initial output of the exponential smoothing algorithm) is being initialized to  (the initial raw data or observation). Because exponential smoothing requires that, at each stage, we have the previous forecast , it is not obvious how to get the method started. We could assume that the initial forecast is equal to the initial value of demand; however, this approach has a serious drawback. Exponential smoothing puts substantial weight on past observations, so the initial value of demand will have an unreasonably large effect on early forecasts. This problem can be overcome by allowing the process to evolve for a reasonable number of periods (10 or more) and using the average of the demand during those periods as the initial forecast. There are many other ways of setting this initial value, but the smaller the value of , the more sensitive the forecast will be on the selection of this initial smoother value .\[8\]\[9\]

### Optimization

\[[edit](<https://en.wikipedia.org/w/index.php?title=Exponential_smoothing&action=edit&section=4>)\]

For every exponential smoothing method, we also need to choose the value for the smoothing parameters. For simple exponential smoothing, there is only one smoothing parameter (*α*), but for the methods that follow there are usually more than one smoothing parameter.

There are cases where the smoothing parameters may be chosen in a subjective manner – the forecaster specifies the value of the smoothing parameters based on previous experience. However, a more robust and objective way to obtain values of the unknown parameters included in any exponential smoothing method is to estimate them from the observed data.

The unknown parameters and the initial values for any exponential smoothing method can be estimated by minimizing the [sum of squared errors](<https://en.wikipedia.org/wiki/Sum_of_squared_errors_of_prediction>) (SSE). The errors are specified as  for  (the one-step-ahead within-sample forecast errors) where  and  are a variable to be predicted at  and a variable as the prediction result at  (based on the previous data or prediction), respectively. Hence, we find the values of the unknown parameters and the initial values that minimize

\[10\]

Unlike the regression case (where we have formulae to directly compute the regression coefficients which minimize the SSE) this involves a non-linear minimization problem, and we need to use an [optimization](<https://en.wikipedia.org/wiki/Mathematical_optimization>) tool to perform this.

### "Exponential" naming

\[[edit](<https://en.wikipedia.org/w/index.php?title=Exponential_smoothing&action=edit&section=5>)\]

The name *exponential smoothing* is attributed to the use of the exponential function as the filter [impulse response](<https://en.wikipedia.org/wiki/Impulse_response>) in the [convolution](<https://en.wikipedia.org/wiki/Convolution>).

By direct substitution of the defining equation for simple exponential smoothing back into itself we find that

In other words, as time passes the smoothed statistic  becomes the weighted average of a greater and greater number of the past observations , and the weights assigned to previous observations are proportional to the terms of the geometric progression

A [geometric progression](<https://en.wikipedia.org/wiki/Geometric_progression>) is the discrete version of an [exponential function](<https://en.wikipedia.org/wiki/Exponential_function>), so this is where the name for this smoothing method originated according to [Statistics](<https://en.wikipedia.org/wiki/Statistics>) lore.

### Comparison with moving average

\[[edit](<https://en.wikipedia.org/w/index.php?title=Exponential_smoothing&action=edit&section=6>)\]

Exponential smoothing and moving average have similar defects of introducing a lag relative to the input data. While this can be corrected by shifting the result by half the window length for a symmetrical kernel, such as a moving average or gaussian, this approach is not possible for exponential smoothing since it is an [IIR filter](<https://en.wikipedia.org/wiki/IIR_filter>) and therefore has an asymmetric kernel and frequency-dependent [group delay](<https://en.wikipedia.org/wiki/Group_delay>). This means each constituent frequency is shifted by a different amount and therefore, there is no single number of samples that can be used to shift the output signal to account for the lag.

Both filters also both have roughly the same distribution of forecast error when *α* = 2/(*k* + 1) where *k* is the number of past data points in consideration of moving average. They differ in that exponential smoothing takes into account all past data, whereas moving average only takes into account *k* past data points. Computationally speaking, they also differ in that moving average requires that the past *k* data points, or the data point at lag *k* + 1 plus the most recent forecast value, to be kept, whereas exponential smoothing only needs the most recent forecast value to be kept.\[11\]

In the [signal processing](<https://en.wikipedia.org/wiki/Signal_processing>) literature, the use of non-causal (symmetric) filters is commonplace, and the exponential [window function](<https://en.wikipedia.org/wiki/Window_function>) is broadly used in this fashion, but a different terminology is used: exponential smoothing is equivalent to a first-order [infinite-impulse response](<https://en.wikipedia.org/wiki/Infinite-impulse_response>) (IIR) filter and moving average is equivalent to a [finite impulse response filter](<https://en.wikipedia.org/wiki/Finite_impulse_response_filter>) with equal weighting factors.

## Double exponential smoothing (Holt linear)

\[[edit](<https://en.wikipedia.org/w/index.php?title=Exponential_smoothing&action=edit&section=7>)\]

Simple exponential smoothing does not do well when there is a [trend](<https://en.wikipedia.org/wiki/Trend_estimation>) in the data.\[1\] In such situations, several methods were devised under the name "double exponential smoothing" or "second-order exponential smoothing," which is the recursive application of an exponential filter twice, thus being termed "double exponential smoothing". The basic idea behind double exponential smoothing is to introduce a term to take into account the possibility of a series exhibiting some form of trend. This slope component is itself updated via exponential smoothing.

One method works as follows:\[12\]

Again, the raw data sequence of observations is represented by , beginning at time . We use  to represent the smoothed value for time , and  is our best estimate of the trend at time . The output of the algorithm is now written as , an estimate of the value of  at time  based on the raw data up to time . Double exponential smoothing is given by the formulas

and for  by

where  () is the *data smoothing factor*, and  () is the *trend smoothing factor*.

To forecast beyond  is given by the following approximation:

.

Setting the initial value  is a matter of preference. An option other than the one listed above is  for some .

Note that *F*0 is undefined (there is no estimation for time 0), and according to the definition *F*1=*s*0+*b*0, which is well defined, thus further values can be evaluated.

A second method, referred to as either Brown's linear exponential smoothing (LES) or Brown's double exponential smoothing, has only one smoothing factor, :\[13\]

where *a**t*, the estimated level at time *t*, and *b**t*, the estimated trend at time *t*, are given by

## Triple exponential smoothing (Holt–Winters)

\[[edit](<https://en.wikipedia.org/w/index.php?title=Exponential_smoothing&action=edit&section=8>)\]

Triple exponential smoothing applies exponential smoothing three times, which is commonly used when there are three high frequency signals to be removed from a [time series](<https://en.wikipedia.org/wiki/Time_series>) under study. There are different types of seasonality: 'multiplicative' and 'additive' in nature. Multiplicative signifies that the seasonal effect size depends on the current level, while additive seasonal effects do not.

If in *every* summer we sell 10,000 more balls of ice cream than we do in winter, then seasonality is *additive*. However, if we sell 6 times more ice cream in summer than in winter, that means that the difference in sales amounts moves together with the level itself: in years with overall larger consumption, the difference between summer and winter is larger and vice versa. The effect is thus *multiplicative*. Multiplicative seasonality can be represented as a constant factor, additive seasonality as an absolute amount.\[14\]

Triple exponential smoothing was first suggested by Holt's student, Peter Winters, in 1960 after reading a signal processing book from the 1940s on exponential smoothing.\[15\] Holt's novel idea was to repeat filtering an odd number of times greater than 1 and less than 5, which was popular with scholars of previous eras.\[15\] While recursive filtering had been used previously, it was applied twice and four times to coincide with the [Hadamard conjecture](<https://en.wikipedia.org/wiki/Hadamard_conjecture>), while triple application required more than double the operations of singular convolution. The use of a triple application is considered a [rule of thumb](<https://en.wikipedia.org/wiki/Rule_of_thumb>) technique, rather than one based on theoretical foundations and has often been overemphasized by practitioners.\[*[citation needed](<https://en.wikipedia.org/wiki/Wikipedia:Citation_needed>)*\]

Suppose we have a sequence of observations  beginning at time  with a cycle of seasonal change of length . The method calculates a trend line for the data as well as seasonal indices that weight the values in the trend line based on where that time point falls in the cycle of length .

Let  represent the smoothed value of the constant part for time ,  is the sequence of best estimates of the linear trend that are superimposed on the seasonal changes, and  is the sequence of seasonal correction factors. We wish to estimate  at every time mod  in the cycle that the observations take on. As a rule of thumb, a minimum of two full seasons (or  periods) of historical data is needed to initialize a set of seasonal factors.

The output of the algorithm is again written as , an estimate of the value of  at time  based on the raw data up to time . Triple exponential smoothing with multiplicative seasonality is given by the formulas\[1\]

where  () is the *data smoothing factor*,  () is the *trend smoothing factor*, and  () is the *seasonal change smoothing factor*.

The general formula for the initial trend estimate  is

.

Setting the initial estimates for the seasonal indices  for  is a bit more involved. If  is the number of complete cycles present in your data, then

where

.

Note that  is the average value of  in the  cycle of your data.

This results in

Triple exponential smoothing with additive seasonality is given by \[*[citation needed](<https://en.wikipedia.org/wiki/Wikipedia:Citation_needed>)*\]

## Implementations in statistics packages

\[[edit](<https://en.wikipedia.org/w/index.php?title=Exponential_smoothing&action=edit&section=9>)\]

- [R](<https://en.wikipedia.org/wiki/R_(programming_language)>): the HoltWinters function in the stats package\[16\] and ets function in the forecast package\[17\] (a more complete implementation, generally resulting in a better performance\[18\]).
- [Python](<https://en.wikipedia.org/wiki/Python_(programming_language)>): the holtwinters module of the statsmodels package allow for simple, double and triple exponential smoothing.
- IBM [SPSS](<https://en.wikipedia.org/wiki/SPSS>) includes Simple, Simple Seasonal, Holt's Linear Trend, Brown's Linear Trend, Damped Trend, Winters' Additive, and Winters' Multiplicative in the Time-Series modeling procedure within its Statistics and Modeler statistical packages. The default Expert Modeler feature evaluates all seven exponential smoothing models and ARIMA models with a range of nonseasonal and seasonal *p*, *d*, and *q* values, and selects the model with the lowest [Bayesian Information Criterion](<https://en.wikipedia.org/wiki/Bayesian_Information_Criterion>) statistic.
- [Stata](<https://en.wikipedia.org/wiki/Stata>): tssmooth command\[19\]
- [LibreOffice](<https://en.wikipedia.org/wiki/LibreOffice>) 5.2\[20\]
- [Microsoft Excel](<https://en.wikipedia.org/wiki/Microsoft_Excel>) 2016\[21\]
- [Julia](<https://en.wikipedia.org/wiki/Julia_(programming_language)>): TrendDecomposition.jl package\[22\] implements simple and double exponential smoothing and Holts-Winters forecasting procedure.

## See also

\[[edit](<https://en.wikipedia.org/w/index.php?title=Exponential_smoothing&action=edit&section=10>)\]

- [Autoregressive moving average model](<https://en.wikipedia.org/wiki/Autoregressive_moving_average_model>) (ARMA)
- [Errors and residuals in statistics](<https://en.wikipedia.org/wiki/Errors_and_residuals_in_statistics>)
- [Moving average](<https://en.wikipedia.org/wiki/Moving_average>)

## Notes

\[[edit](<https://en.wikipedia.org/w/index.php?title=Exponential_smoothing&action=edit&section=11>)\]

1. 1 2 3 *["NIST/SEMATECH e-Handbook of Statistical Methods"](<http://www.itl.nist.gov/div898/handbook/>). NIST. Retrieved 23 May 2010.*
2. 1 2 *Oppenheim, Alan V.; Schafer, Ronald W. (1975). *Digital Signal Processing*. [Prentice Hall](<https://en.wikipedia.org/wiki/Prentice_Hall>). p. 5. [ISBN](<https://en.wikipedia.org/wiki/ISBN_(identifier)>) [0-13-214635-5](<https://en.wikipedia.org/wiki/Special:BookSources/0-13-214635-5>).*
3. ↑ *Brown, Robert G. (1956). [*Exponential Smoothing for Predicting Demand*](<http://legacy.library.ucsf.edu/tid/dae94e00;jsessionid=104A0CEFFA31ADC2FA5E0558F69B3E1D.tobacco03>). Cambridge, Massachusetts: Arthur D. Little Inc. p. 15.*
4. ↑ *[Holt, Charles C.](<https://en.wikipedia.org/wiki/Charles_C._Holt>) (1957). "Forecasting Trends and Seasonal by Exponentially Weighted Averages". *Office of Naval Research Memorandum*. **52**.* reprinted in *[Holt, Charles C.](<https://en.wikipedia.org/wiki/Charles_C._Holt>) (January–March 2004). "Forecasting Trends and Seasonal by Exponentially Weighted Averages". *[International Journal of Forecasting](<https://en.wikipedia.org/wiki/International_Journal_of_Forecasting>)*. **20** (1): 5–10. [doi](<https://en.wikipedia.org/wiki/Doi_(identifier)>):[10.1016/j.ijforecast.2003.09.015](<https://doi.org/10.1016%2Fj.ijforecast.2003.09.015>).*
5. ↑ *Brown, Robert Goodell (1963). [*Smoothing Forecasting and Prediction of Discrete Time Series*](<http://babel.hathitrust.org/cgi/pt?id=mdp.39015004514728;view=1up;seq=1>). Englewood Cliffs, NJ: Prentice-Hall.*
6. ↑ *["NIST/SEMATECH e-Handbook of Statistical Methods, 6.4.3.1. Single Exponential Smoothing"](<http://www.itl.nist.gov/div898/handbook/pmc/section4/pmc431.htm>). NIST. Retrieved 5 July 2017.*
7. ↑ *Nau, Robert. ["Averaging and Exponential Smoothing Models"](<http://www.duke.edu/~rnau/411avg.htm>). Retrieved 26 July 2010.*
8. ↑ "Production and Operations Analysis" Nahmias. 2009.
9. ↑ Čisar, P., & Čisar, S. M. (2011). "Optimization methods of EWMA statistics." *Acta Polytechnica Hungarica*, 8(5), 73–87. Page 78.
10. ↑ *[*7.1 Simple exponential smoothing | Forecasting: Principles and Practice*](<https://www.otexts.org/fpp/7/1>).*
11. ↑ *Nahmias, Steven; Olsen, Tava Lennon. *Production and Operations Analysis* (7th ed.). Waveland Press. p. [53](<https://books.google.com/books?id=SIsoBgAAQBAJ&pg=PA53>). [ISBN](<https://en.wikipedia.org/wiki/ISBN_(identifier)>) [9781478628248](<https://en.wikipedia.org/wiki/Special:BookSources/9781478628248>).*
12. ↑ *["6.4.3.3. Double Exponential Smoothing"](<http://www.itl.nist.gov/div898/handbook/pmc/section4/pmc433.htm>). *itl.nist.gov*. Retrieved 25 September 2011.*
13. ↑ *["Averaging and Exponential Smoothing Models"](<http://www.duke.edu/~rnau/411avg.htm>). *duke.edu*. Retrieved 25 September 2011.*
14. ↑ *Kalehar, Prajakta S. ["Time series Forecasting using Holt–Winters Exponential Smoothing"](<http://www.it.iitb.ac.in/~praj/acads/seminar/04329008_ExponentialSmoothing.pdf>) (PDF). Retrieved 23 June 2014.*
15. 1 2 *Winters, P. R. (April 1960). "Forecasting Sales by Exponentially Weighted Moving Averages". *[Management Science](<https://en.wikipedia.org/wiki/Management_Science:_A_Journal_of_the_Institute_for_Operations_Research_and_the_Management_Sciences>)*. **6** (3): 324–342. [doi](<https://en.wikipedia.org/wiki/Doi_(identifier)>):[10.1287/mnsc.6.3.324](<https://doi.org/10.1287%2Fmnsc.6.3.324>).*
16. ↑ *["R: Holt–Winters Filtering"](<https://stat.ethz.ch/R-manual/R-patched/library/stats/html/HoltWinters.html>). *stat.ethz.ch*. Retrieved 5 June 2016.*
17. ↑ *["ets {forecast} | inside-R | A Community Site for R"](<https://web.archive.org/web/20160716153135/http://www.inside-r.org/packages/cran/forecast/docs/ets>). *inside-r.org*. Archived from [the original](<http://www.inside-r.org/packages/cran/forecast/docs/ets>) on 16 July 2016. Retrieved 5 June 2016.*
18. ↑ *["Comparing HoltWinters() and ets()"](<http://robjhyndman.com/hyndsight/estimation2/>). *Hyndsight*. 29 May 2011. Retrieved 5 June 2016.*
19. ↑ [tssmooth](<https://www.stata.com/help.cgi?tssmooth>) in Stata manual
20. ↑ *["LibreOffice 5.2: Release Notes – the Document Foundation Wiki"](<https://wiki.documentfoundation.org/ReleaseNotes/5.2#New_spreadsheet_functions>).*
21. ↑ *["Excel 2016 Forecasting Functions | Real Statistics Using Excel"](<http://www.real-statistics.com/time-series-analysis/basic-time-series-forecasting/excel-2016-forecasting-functions/>).*
22. ↑ [TrendDecomposition.jl](<https://sdbrinkmann.github.io/TrendDecomposition.jl/stable/>) Julia implementation of exponential smoothing and Holt-Winters forecasting procedure

## External links

\[[edit](<https://en.wikipedia.org/w/index.php?title=Exponential_smoothing&action=edit&section=12>)\]

- [Lecture notes on exponential smoothing (Robert Nau, Duke University)](<http://people.duke.edu/~rnau/411avg.htm>)
- [Data Smoothing](<http://demonstrations.wolfram.com/DataSmoothing/>) by Jon McLoone, [The Wolfram Demonstrations Project](<https://en.wikipedia.org/wiki/The_Wolfram_Demonstrations_Project>)
- [The Holt–Winters Approach to Exponential Smoothing: 50 Years Old and Going Strong](<https://foresight.forecasters.org/product/foresight-issue-19/>) by Paul Goodwin (2010) [Foresight: The International Journal of Applied Forecasting](<https://en.wikipedia.org/wiki/Foresight:_The_International_Journal_of_Applied_Forecasting>)
- [Algorithms for Unevenly Spaced Time Series: Moving Averages and Other Rolling Operators](<https://web.archive.org/web/20150623211648/http://www.eckner.com/papers/ts_alg.pdf>) by Andreas Eckner

Retrieved from "[https://en.wikipedia.org/w/index.php?title=Exponential\_smoothing&oldid=1372755600](<https://en.wikipedia.org/w/index.php?title=Exponential_smoothing&oldid=1372755600>)"

[Category](<https://en.wikipedia.org/wiki/Help:Category>):

- [Time series](<https://en.wikipedia.org/wiki/Category:Time_series>)

Hidden categories:

- [Articles with short description](<https://en.wikipedia.org/wiki/Category:Articles_with_short_description>)
- [Short description matches Wikidata](<https://en.wikipedia.org/wiki/Category:Short_description_matches_Wikidata>)
- [Use dmy dates from December 2020](<https://en.wikipedia.org/wiki/Category:Use_dmy_dates_from_December_2020>)
- [All articles with unsourced statements](<https://en.wikipedia.org/wiki/Category:All_articles_with_unsourced_statements>)
- [Articles with unsourced statements from April 2026](<https://en.wikipedia.org/wiki/Category:Articles_with_unsourced_statements_from_April_2026>)
- [Articles with unsourced statements from February 2025](<https://en.wikipedia.org/wiki/Category:Articles_with_unsourced_statements_from_February_2025>)


<!-- sllmwiki:original-source -->
## Original source

- URL: <https://en.wikipedia.org/wiki/Exponential_smoothing>
- License: [CC-BY-SA-4.0](<https://creativecommons.org/licenses/by-sa/4.0/>)
