# EWMA chart - Wikipedia

From Wikipedia, the free encyclopedia

| EWMA chart |  |
|---|---|
| Originally proposed by | S. W. Roberts |
| Process observations |  |
| Rational subgroup size | n = 1 |
| Measurement type | Moving average of the quality characteristic |
| Quality characteristic type | [Variables data](<https://en.wikipedia.org/wiki/Variable_and_attribute_(research)>) |
| Underlying distribution | [Normal distribution](<https://en.wikipedia.org/wiki/Normal_distribution>) |
| Performance |  |
| Size of shift to detect | ≤ 1.5σ |
| Process variation chart |  |
| Not applicable |  |
| Process mean chart |  |
| [[image: ](<https://thumb.wikimedia.org/wikipedia/commons/thumb/a/a7/MATLABEWMAChart.png/250px-MATLABEWMAChart.png?utm_source=en.wikipedia.org&utm_campaign=parser&utm_content=thumbnail>)](<https://en.wikipedia.org/wiki/File:MATLABEWMAChart.png>) |  |
| Center line | The target value, T, of the quality characteristic |
| Control limits | $T\pm L{\frac {S}{\sqrt {n}}}{\sqrt {{\frac {\lambda }{2-\lambda }}\lbrack 1-\left(1-\lambda \right)^{2i}\rbrack }}$ |
| Plotted statistic | $z_{i}=\lambda {\bar {x}}_{i}+\left(1-\lambda \right)z_{i-1}$ |

In [statistical quality control](<https://en.wikipedia.org/wiki/Statistical_process_control>), an **EWMA chart** (or **exponentially weighted moving average chart**) is a type of [control chart](<https://en.wikipedia.org/wiki/Control_chart>) used to monitor either variables or attributes-type data using the monitored [business](<https://en.wikipedia.org/wiki/Business_process>) or [industrial process](<https://en.wikipedia.org/wiki/List_of_industrial_processes>)'s entire history of output.\[1\] While other control charts treat rational subgroups of samples individually, the EWMA chart tracks the [exponentially-weighted moving average](<https://en.wikipedia.org/wiki/EWMA>) of all prior sample means. EWMA weights samples in geometrically decreasing order so that the most recent samples are weighted most highly while the most distant samples contribute very little.\[2\]: 406

Although the [normal distribution](<https://en.wikipedia.org/wiki/Normal_distribution>) is the basis of the EWMA chart, the chart is also relatively robust in the face of [non-normally distributed](<https://en.wikipedia.org/wiki/Normal_distribution>) quality characteristics.\[2\]: 412  There is, however, an adaptation of the chart that accounts for quality characteristics that are better modeled by the [Poisson distribution](<https://en.wikipedia.org/wiki/Poisson_distribution>).\[2\]: 415  The chart monitors only the process mean; monitoring the process variability requires the use of some other technique.\[2\]: 414

The EWMA control chart requires a knowledgeable person to select two parameters before setup:

1. The first parameter is λ, the weight given to the most recent rational subgroup mean. λ must satisfy 0 \< λ ≤ 1, but selecting the "right" value is a matter of personal preference and experience. One 2005 textbook recommends 0.05 ≤ λ ≤ 0.25,\[2\]: 411  while a 1986 journal article recommends 0.1 ≤ λ ≤ 0.3.\[3\]
2. The second parameter is L, the multiple of the rational subgroup standard deviation that establishes the control limits. L is typically set at 3 to match other control charts, but it may be necessary to reduce L slightly for small values of λ.\[2\]: 406

Instead of plotting rational subgroup averages directly, the EWMA chart computes successive observations zi by computing the rational subgroup average, ${\bar {x}}_{i}$, and then combining that new subgroup average with the running average of all preceding observations, zi - 1, using the specially–chosen weight, λ, as follows:

$z_{i}=\lambda {\bar {x}}_{i}+\left(1-\lambda \right)z_{i-1}$.

The control limits for this chart type are $T\pm L{\frac {S}{\sqrt {n}}}{\sqrt {{\frac {\lambda }{2-\lambda }}\lbrack 1-\left(1-\lambda \right)^{2i}\rbrack }}$ where T and S are the estimates of the long-term process mean and standard deviation established during control-chart setup and n is the number of samples in the rational subgroup. Note that the limits widen for each successive rational subgroup, approaching $\pm L{\frac {\hat {\sigma }}{\sqrt {n}}}{\sqrt {\frac {\lambda }{2-\lambda }}}$.\[2\]: 407

The EWMA chart is sensitive to small shifts in the process mean, but does not match the ability of Shewhart-style charts (namely the [${\bar {x}}$ and R](<https://en.wikipedia.org/wiki/Xbar_and_R_chart>) and [${\bar {x}}$ and s charts](<https://en.wikipedia.org/wiki/Xbar_and_s_chart>)) to detect larger shifts.\[2\]: 412  One author recommends superimposing the EWMA chart on top of a suitable Shewhart-style chart with widened control limits in order to detect both small and large shifts in the process mean.\[*[citation needed](<https://en.wikipedia.org/wiki/Wikipedia:Citation_needed>)*\]

Exponentially weighted moving variance (EWMVar) can be used to obtain a significance score or limits that automatically adjust to the observed data.\[4\]\[5\]

## References

\[[edit](<https://en.wikipedia.org/w/index.php?title=EWMA_chart&action=edit&section=1>)\]

1. ↑ *["EWMA Control Charts"](<http://www.itl.nist.gov/div898/handbook/pmc/section3/pmc324.htm>). *Engineering Statistics Handbook*. [National Institute of Standards and Technology](<https://en.wikipedia.org/wiki/National_Institute_of_Standards_and_Technology>). Retrieved 2009-08-10.*
2. 1 2 3 4 5 6 7 8  *Montgomery, Douglas (2005). [*Introduction to Statistical Quality Control*](<https://web.archive.org/web/20080620095346/http://www.eas.asu.edu/~masmlab/montgomery/>). [Hoboken, New Jersey](<https://en.wikipedia.org/wiki/Hoboken,_New_Jersey>): [John Wiley & Sons](<https://en.wikipedia.org/wiki/John_Wiley_&_Sons>), Inc. [ISBN](<https://en.wikipedia.org/wiki/ISBN_(identifier)>) [978-0-471-65631-9](<https://en.wikipedia.org/wiki/Special:BookSources/978-0-471-65631-9>). [OCLC](<https://en.wikipedia.org/wiki/OCLC_(identifier)>) [56729567](<https://search.worldcat.org/oclc/56729567>). Archived from [the original](<http://www.eas.asu.edu/~masmlab/montgomery/>) on 2008-06-20. Retrieved 2009-08-10.*
3. ↑ J. S. Hunter, The Exponentially Weighted Moving Average, *Journal of Quality Technology* 18: 203-210, 1986. [doi](<https://en.wikipedia.org/wiki/Doi_(identifier)>):[10.1080/00224065.1986.11979014](<https://doi.org/10.1080%2F00224065.1986.11979014>)
4. ↑ *Schubert, E.; Weiler, M.; [Kriegel, H. P.](<https://en.wikipedia.org/wiki/Hans-Peter_Kriegel>) (2014). *SigniTrend: scalable detection of emerging topics in textual streams by hashed significance thresholds*. Proceedings of the 20th ACM SIGKDD international conference on Knowledge discovery and data mining - KDD '14. pp. 871–880. [doi](<https://en.wikipedia.org/wiki/Doi_(identifier)>):[10.1145/2623330.2623740](<https://doi.org/10.1145%2F2623330.2623740>). [ISBN](<https://en.wikipedia.org/wiki/ISBN_(identifier)>) [9781450329569](<https://en.wikipedia.org/wiki/Special:BookSources/9781450329569>).*
5. ↑ Finch, Tony. (2009). *Incremental calculation of weighted mean and variance.* [PDF](<https://fanf2.user.srcf.net/hermes/doc/antiforgery/stats.pdf>). University of Cambridge Computing Service.

Retrieved from "[https://en.wikipedia.org/w/index.php?title=EWMA\_chart&oldid=1266288624](<https://en.wikipedia.org/w/index.php?title=EWMA_chart&oldid=1266288624>)"

[Categories](<https://en.wikipedia.org/wiki/Help:Category>):

- [Quality control tools](<https://en.wikipedia.org/wiki/Category:Quality_control_tools>)
- [Statistical charts and diagrams](<https://en.wikipedia.org/wiki/Category:Statistical_charts_and_diagrams>)

Hidden categories:

- [Articles with short description](<https://en.wikipedia.org/wiki/Category:Articles_with_short_description>)
- [Short description matches Wikidata](<https://en.wikipedia.org/wiki/Category:Short_description_matches_Wikidata>)
- [All articles with unsourced statements](<https://en.wikipedia.org/wiki/Category:All_articles_with_unsourced_statements>)
- [Articles with unsourced statements from April 2010](<https://en.wikipedia.org/wiki/Category:Articles_with_unsourced_statements_from_April_2010>)


<!-- sllmwiki:original-source -->
## Original source

- URL: <https://en.wikipedia.org/wiki/EWMA_chart>
- License: [CC-BY-SA-4.0](<https://creativecommons.org/licenses/by-sa/4.0/>)
