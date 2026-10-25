# Low-pass filter - Wikipedia

From Wikipedia, the free encyclopedia

| [[image: icon](<https://thumb.wikimedia.org/wikipedia/en/thumb/9/99/Question_book-new.svg/60px-Question_book-new.svg.png?utm_source=en.wikipedia.org&utm_campaign=parser&utm_content=thumbnail>)](<https://en.wikipedia.org/wiki/File:Question_book-new.svg>) | This article **needs [more citations](<https://en.wikipedia.org/wiki/Wikipedia:Verifiability>)**. Please help [improve this article](<https://en.wikipedia.org/wiki/Special:EditPage/Low-pass_filter>) by [adding citations to reliable sources](<https://en.wikipedia.org/wiki/Help:Referencing_for_beginners>). Unsourced material may be challenged and [removed](<https://en.wikipedia.org/wiki/Wikipedia:Verifiability#Burden_of_evidence>).\ *Find sources:* ["Low-pass filter"](<https://www.google.com/search?as_eq=wikipedia&q=%22Low-pass+filter%22>) – [news](<https://www.google.com/search?tbm=nws&q=%22Low-pass+filter%22+-wikipedia&tbs=ar:1>) **·** [newspapers](<https://www.google.com/search?&q=%22Low-pass+filter%22&tbs=bkt:s&tbm=bks>) **·** [books](<https://www.google.com/search?tbs=bks:1&q=%22Low-pass+filter%22+-wikipedia>) **·** [scholar](<https://scholar.google.com/scholar?q=%22Low-pass+filter%22>) **·** [JSTOR](<https://www.jstor.org/action/doBasicSearch?Query=%22Low-pass+filter%22&acc=on&wc=on>) *(May 2023)**([Learn how and when to remove this message](<https://en.wikipedia.org/wiki/Help:Maintenance_template_removal>))* |
|---|---|

A **low-pass filter** is a [filter](<https://en.wikipedia.org/wiki/Filter_(signal_processing)>) that passes [signals](<https://en.wikipedia.org/wiki/Signal_(electrical_engineering)>) with a [frequency](<https://en.wikipedia.org/wiki/Frequency>) lower than a selected [cutoff frequency](<https://en.wikipedia.org/wiki/Cutoff_frequency>) and [attenuates](<https://en.wikipedia.org/wiki/Attenuate>) signals with frequencies higher than the cutoff frequency. The exact [frequency response](<https://en.wikipedia.org/wiki/Frequency_response>) of the filter depends on the [filter design](<https://en.wikipedia.org/wiki/Filter_design>). The filter is sometimes called a **high-cut filter**, or **treble-cut filter** in audio applications. A low-pass filter is the complement of a [high-pass filter](<https://en.wikipedia.org/wiki/High-pass_filter>).

In optics, **high-pass** and **low-pass** may have different meanings, depending on whether referring to the frequency or wavelength of light, since these variables are inversely related. High-pass frequency filters would act as low-pass wavelength filters, and vice versa. For this reason, it is a good practice to refer to wavelength filters as *short-pass* and *long-pass* to avoid confusion, which would correspond to *high-pass* and *low-pass* frequencies.\[1\]

Low-pass filters exist in many different forms, including electronic circuits such as a *[hiss filter](<https://en.wikipedia.org/wiki/Hiss_filter>)* used in [audio](<https://en.wikipedia.org/wiki/Sound_recording>), [anti-aliasing filters](<https://en.wikipedia.org/wiki/Anti-aliasing_filter>) for conditioning signals before [analog-to-digital conversion](<https://en.wikipedia.org/wiki/Analog-to-digital_conversion>), [digital filters](<https://en.wikipedia.org/wiki/Digital_filter>) for smoothing sets of data, acoustic barriers, [blurring](<https://en.wikipedia.org/wiki/Gaussian_blur>) of images, and so on. The [moving average](<https://en.wikipedia.org/wiki/Moving_average_(finance)>) operation used in fields such as finance is a particular kind of low-pass filter and can be analyzed with the same [signal processing](<https://en.wikipedia.org/wiki/Signal_processing>) techniques as are used for other low-pass filters. Low-pass filters provide a smoother form of a signal, removing the short-term fluctuations and leaving the longer-term trend.

Filter designers will often use the low-pass form as a [prototype filter](<https://en.wikipedia.org/wiki/Prototype_filter>). That is a filter with unity bandwidth and impedance. The desired filter is obtained from the prototype by scaling for the desired bandwidth and impedance and transforming into the desired bandform (that is, low-pass, high-pass, [band-pass](<https://en.wikipedia.org/wiki/Band-pass_filter>) or [band-stop](<https://en.wikipedia.org/wiki/Band-stop_filter>)).

## Examples

\[[edit](<https://en.wikipedia.org/w/index.php?title=Low-pass_filter&action=edit&section=1>)\]

Examples of low-pass filters occur in [acoustics](<https://en.wikipedia.org/wiki/Acoustics>), [optics](<https://en.wikipedia.org/wiki/Optics>) and [electronics](<https://en.wikipedia.org/wiki/Electronics>).

A stiff physical barrier tends to reflect higher sound frequencies, acting as an acoustic low-pass filter for transmitting sound. When music is playing in another room, the low notes are easily heard, while the high notes are attenuated.

An [optical filter](<https://en.wikipedia.org/wiki/Optical_filter>) with the same function can correctly be called a low-pass filter, but conventionally is called a *longpass* filter (low frequency is long wavelength), to avoid confusion.\[1\]

In an electronic low-pass [RC filter](<https://en.wikipedia.org/wiki/RC_filter>) for voltage signals, high frequencies in the input signal are attenuated, but the filter has little attenuation below the [cutoff frequency](<https://en.wikipedia.org/wiki/Cutoff_frequency>) determined by its [RC time constant](<https://en.wikipedia.org/wiki/RC_time_constant>). For current signals, a similar circuit, using a resistor and capacitor in [parallel](<https://en.wikipedia.org/wiki/Parallel_circuits#Parallel_circuits>), works in a similar manner. (See [current divider](<https://en.wikipedia.org/wiki/Current_divider>) discussed in more detail below.)

Electronic low-pass filters are used on inputs to [subwoofers](<https://en.wikipedia.org/wiki/Subwoofer>) and other types of [loudspeakers](<https://en.wikipedia.org/wiki/Loudspeaker>), to block high pitches that they cannot efficiently reproduce. Radio transmitters use low-pass filters to block [harmonic](<https://en.wikipedia.org/wiki/Harmonic>) emissions that might interfere with other communications. The tone knob on many [electric guitars](<https://en.wikipedia.org/wiki/Electric_guitar>) is a low-pass filter used to reduce the amount of treble in the sound. An [integrator](<https://en.wikipedia.org/wiki/Integrator>) is another [time constant](<https://en.wikipedia.org/wiki/Time_constant#Time_constants_in_electrical_circuits>) low-pass filter.\[2\]

Telephone lines fitted with [DSL splitters](<https://en.wikipedia.org/wiki/DSL_splitter>) use low-pass filters to separate [DSL](<https://en.wikipedia.org/wiki/Digital_subscriber_line>) from [POTS](<https://en.wikipedia.org/wiki/Plain_old_telephone_service>) signals (and [high-pass](<https://en.wikipedia.org/wiki/High-pass_filter>) vice versa), which share the same [pair](<https://en.wikipedia.org/wiki/Twisted_pair>) of wires (*transmission channel*).\[3\]\[4\]

Low-pass filters also play a significant role in the sculpting of sound created by analogue and virtual analogue [synthesisers](<https://en.wikipedia.org/wiki/Synthesiser>). *See [subtractive synthesis](<https://en.wikipedia.org/wiki/Subtractive_synthesis>).*

A low-pass filter is used as an [anti-aliasing filter](<https://en.wikipedia.org/wiki/Anti-aliasing_filter>) before [sampling](<https://en.wikipedia.org/wiki/Sampling_(signal_processing)>) and for [reconstruction](<https://en.wikipedia.org/wiki/Reconstruction_filter>) in [digital-to-analog conversion](<https://en.wikipedia.org/wiki/Digital-to-analog_conversion>).

## Ideal and real filters

\[[edit](<https://en.wikipedia.org/w/index.php?title=Low-pass_filter&action=edit&section=2>)\]

[[image: ](<https://thumb.wikimedia.org/wikipedia/commons/thumb/d/d4/Sinc_function_%28normalized%29.svg/250px-Sinc_function_%28normalized%29.svg.png?utm_source=en.wikipedia.org&utm_campaign=parser&utm_content=thumbnail>)](<https://en.wikipedia.org/wiki/File:Sinc_function_(normalized).svg>)

The [sinc function](<https://en.wikipedia.org/wiki/Sinc_function>), the time-domain [impulse response](<https://en.wikipedia.org/wiki/Impulse_response>) of an ideal low-pass filter. The ripples of a true sinc extend infinitely to the left and right while getting smaller and smaller, but this particular graph is truncated.

[[image: ](<https://thumb.wikimedia.org/wikipedia/commons/thumb/6/60/Butterworth_response.svg/500px-Butterworth_response.svg.png?utm_source=en.wikipedia.org&utm_campaign=parser&utm_content=thumbnail>)](<https://en.wikipedia.org/wiki/File:Butterworth_response.svg>)

The gain-magnitude frequency response of a first-order (one-pole) low-pass filter. *Power gain* is shown in decibels (i.e., a 3 [dB](<https://en.wikipedia.org/wiki/Decibel>) decline reflects an additional $1/{\sqrt {2}}$ attenuation). [Angular frequency](<https://en.wikipedia.org/wiki/Angular_frequency>) is shown on a logarithmic scale in units of radians per second.

An [ideal low-pass filter](<https://en.wikipedia.org/wiki/Sinc_filter>) completely eliminates all frequencies above the [cutoff frequency](<https://en.wikipedia.org/wiki/Cutoff_frequency>) while passing those below unchanged; its [frequency response](<https://en.wikipedia.org/wiki/Frequency_response>) is a [rectangular function](<https://en.wikipedia.org/wiki/Rectangular_function>) and is a [brick-wall filter](<https://en.wikipedia.org/wiki/Brick-wall_filter>). The transition region present in practical filters does not exist in an ideal filter. An ideal low-pass filter can be realized mathematically (theoretically) by multiplying a signal by the rectangular function in the frequency domain or, equivalently, [convolution](<https://en.wikipedia.org/wiki/Convolution>) with its [impulse response](<https://en.wikipedia.org/wiki/Impulse_response>), a [sinc function](<https://en.wikipedia.org/wiki/Sinc_function>), in the time domain.

However, the ideal filter is impossible to realize without also having signals of infinite extent in time, and so generally needs to be approximated for real ongoing signals, because the sinc function's support region extends to all past and future times. The filter would therefore need to have infinite delay, or knowledge of the infinite future and past, to perform the convolution. It is effectively realizable for pre-recorded digital signals by assuming extensions of zero into the past and future, or, more typically, by making the signal repetitive and using Fourier analysis.

Real filters for [real-time](<https://en.wikipedia.org/wiki/Real-time_computing>) applications approximate the ideal filter by truncating and [windowing](<https://en.wikipedia.org/wiki/Window_function>) the infinite impulse response to make a [finite impulse response](<https://en.wikipedia.org/wiki/Finite_impulse_response>); applying that filter requires delaying the signal for a moderate period of time, allowing the computation to "see" a little bit into the future. This delay is manifested as [phase shift](<https://en.wikipedia.org/wiki/Phase_(waves)>). Greater accuracy in approximation requires a longer delay.

Truncating an ideal low-pass filter result in [ringing artifacts](<https://en.wikipedia.org/wiki/Ringing_artifacts>) via the [Gibbs phenomenon](<https://en.wikipedia.org/wiki/Gibbs_phenomenon>), which can be reduced or worsened by the choice of windowing function. [Design and choice of real filters](<https://en.wikipedia.org/wiki/Window_function#Filter_design>) involves understanding and minimizing these artifacts. For example, simple truncation of the sinc function will create severe ringing artifacts, which can be reduced using window functions that drop off more smoothly at the edges.\[5\]

## Time response

\[[edit](<https://en.wikipedia.org/w/index.php?title=Low-pass_filter&action=edit&section=3>)\]

[[image: ](<https://thumb.wikimedia.org/wikipedia/commons/thumb/e/e0/1st_Order_Lowpass_Filter_RC.svg/250px-1st_Order_Lowpass_Filter_RC.svg.png?utm_source=en.wikipedia.org&utm_campaign=parser&utm_content=thumbnail>)](<https://en.wikipedia.org/wiki/File:1st_Order_Lowpass_Filter_RC.svg>)

A simple low-pass [RC filter](<https://en.wikipedia.org/wiki/RC_circuit>)

The time response of a low-pass filter is found by solving the response to the simple low-pass RC filter.

Using [Kirchhoff's laws](<https://en.wikipedia.org/wiki/Kirchhoff's_circuit_laws>) we arrive at the differential equation\[6\] $$v_{\text{out}}(t)=v_{\text{in}}(t)-RC{\frac {dv_{\text{out}}}{dt}}.$$ This first-order differential equation is also the standard form of a [leaky integrator](<https://en.wikipedia.org/wiki/Leaky_integrator>), with the time constant τ = *RC*.

### Step input response example

\[[edit](<https://en.wikipedia.org/w/index.php?title=Low-pass_filter&action=edit&section=4>)\]

If we let $v_{\text{in}}(t)$ be a step function of magnitude $V_{\text{i}},$ then the differential equation has the solution\[7\] $$v_{\text{out}}(t)=V_{\text{i}}(1-e^{-\omega _{0}t}),$$ where $\omega _{0}={\frac {1}{RC}}$ is the cutoff frequency of the filter.

## Frequency response

\[[edit](<https://en.wikipedia.org/w/index.php?title=Low-pass_filter&action=edit&section=5>)\]

The most common way to characterize the frequency response of a circuit is to find its Laplace transform\[6\] transfer function, $H(s)=V_{\text{out}}(s)/V_{\text{in}}(s)$. Taking the Laplace transform of our differential equation and solving for $H(s),$ we get $$H(s)={\frac {V_{\text{out}}(s)}{V_{\text{in}}(s)}}={\frac {\omega _{0}}{s+\omega _{0}}}.$$

## Difference equation through discrete time sampling

\[[edit](<https://en.wikipedia.org/w/index.php?title=Low-pass_filter&action=edit&section=6>)\]

A discrete [difference equation](<https://en.wikipedia.org/wiki/Linear_difference_equation>) is easily obtained by sampling the step input response above at regular intervals of $nT,$ where $n=0,1,\dots ,$ and $T$ is the time between samples. Taking the difference between two consecutive samples, we have $$v_{\text{out}}(nT)-v_{\text{out}}{\big (}(n-1)T{\big )}=V_{\text{i}}(1-e^{-\omega _{0}nT})-V_{\text{i}}(1-e^{-\omega _{0}[(n-1)T]}).$$

Solving for $v_{\text{out}}(nT)$ gives $$v_{\text{out}}(nT)=\beta v_{\text{out}}{\big (}(n-1)T{\big )}+(1-\beta )V_{\text{i}},$$ where $\beta =e^{-\omega _{0}T}.$

Using the notation $V_{n}=v_{\text{out}}(nT)$ and $v_{n}=v_{\text{in}}(nT),$ and substituting our sampled value, $v_{n}=V_{i},$ we get the difference equation $$V_{n}=\beta V_{n-1}+(1-\beta )v_{n}.$$

### Error analysis

\[[edit](<https://en.wikipedia.org/w/index.php?title=Low-pass_filter&action=edit&section=7>)\]

Comparing the reconstructed output signal from the difference equation, $V_{n}=\beta V_{n-1}+(1-\beta )v_{n},$ to the step input response, $v_{\text{out}}(t)=V_{i}(1-e^{-\omega _{0}t}),$ we find that there is an exact reconstruction (0% error). This is the reconstructed output for a time-invariant input. However, if the input is *time-variant*, such as $v_{\text{in}}(t)=V_{i}\sin(\omega t),$ this model approximates the input signal as a series of step functions with duration $T,$ producing an error in the reconstructed output signal. The error produced from *time-variant* inputs is difficult to quantify\[*[citation needed](<https://en.wikipedia.org/wiki/Wikipedia:Citation_needed>)*\] but decreases as $T\to 0.$

## Discrete-time realization

\[[edit](<https://en.wikipedia.org/w/index.php?title=Low-pass_filter&action=edit&section=8>)\]

For another method of conversion from continuous- to discrete-time, see [Bilinear transform](<https://en.wikipedia.org/wiki/Bilinear_transform>).

Many [digital filters](<https://en.wikipedia.org/wiki/Digital_filter>) are designed to give low-pass characteristics. Both [infinite impulse response](<https://en.wikipedia.org/wiki/Infinite_impulse_response>) and [finite impulse response](<https://en.wikipedia.org/wiki/Finite_impulse_response>) low pass filters, as well as filters using [Fourier transforms](<https://en.wikipedia.org/wiki/Fourier_transform>), are widely used.

### Simple infinite impulse response filter

\[[edit](<https://en.wikipedia.org/w/index.php?title=Low-pass_filter&action=edit&section=9>)\]

The effect of an infinite impulse response low-pass filter can be simulated on a computer by analyzing an RC filter's behavior in the time domain, and then [discretizing](<https://en.wikipedia.org/wiki/Discrete_signal>) the model.

From the circuit diagram to the right\[*[where?](<https://en.wikipedia.org/wiki/Wikipedia:Naming_conventions_(geographic_names)>)*\], according to [Kirchhoff's laws](<https://en.wikipedia.org/wiki/Kirchhoff's_circuit_laws>) and the definition of [capacitance](<https://en.wikipedia.org/wiki/Capacitance>),

| $v_{\text{in}}(t)-v_{\text{out}}(t)=R\,i(t),$ |  | V |
|---|---|---|

| $Q_{\text{c}}(t)=C\,v_{\text{out}}(t),$ |  | Q |
|---|---|---|

| $i(t)={\frac {dQ_{\text{c}}}{dt}},$ |  | I |
|---|---|---|

where $Q_{\text{c}}(t)$ is the charge stored in the capacitor at time t. Substituting equation **Q** into equation **I** gives $i(t)=C\,dv_{\text{out}}/dt,$ which can be substituted into equation **V**, so that $$v_{\text{in}}(t)-v_{\text{out}}(t)=RC{\frac {dv_{\text{out}}}{dt}}.$$

This equation can be discretized. For simplicity, assume that samples of the input and output are taken at evenly spaced points in time separated by $\Delta _{T}$ time. Let the samples of $v_{\text{in}}$ be represented by the sequence $(x_{1},x_{2},\ldots ,x_{n}),$ and let $v_{\text{out}}$ be represented by the sequence $(y_{1},y_{2},\ldots ,y_{n}),$ which correspond to the same points in time.  Making these substitutions, $$x_{i}-y_{i}=RC\,{\frac {y_{i}-y_{i-1}}{\Delta _{T}}}.$$ Rearranging terms gives the [recurrence relation](<https://en.wikipedia.org/wiki/Recurrence_relation>) $$y_{i}=\underbrace {x_{i}\left({\frac {\Delta _{T}}{RC+\Delta _{T}}}\right)} _{\text{input contribution}}+\underbrace {y_{i-1}\left({\frac {RC}{RC+\Delta _{T}}}\right)} _{\text{inertia from previous output}}.$$ That is, this discrete-time implementation of a simple *RC* low-pass filter is the [exponentially weighted moving average](<https://en.wikipedia.org/wiki/Exponential_smoothing>): $$y_{i}=\alpha x_{i}+(1-\alpha )y_{i-1},\qquad \alpha :={\frac {\Delta _{T}}{RC+\Delta _{T}}}.$$

By definition, the *smoothing factor* is within the range $0\leq \alpha \leq 1$. The expression for α yields the equivalent [time constant](<https://en.wikipedia.org/wiki/Time_constant>) *RC* in terms of the sampling period $\Delta _{T}$ and smoothing factor α: $$RC=\Delta _{T}{\frac {1-\alpha }{\alpha }}.$$ Recalling that $$f_{\text{c}}={\frac {1}{2\pi RC}},\qquad RC={\frac {1}{2\pi f_{\text{c}}}},$$ note that α and $f_{\text{c}}$ are related as $$\alpha ={\frac {2\pi \Delta _{T}f_{\text{c}}}{2\pi \Delta _{T}f_{\text{c}}+1}},\qquad f_{\text{c}}={\frac {\alpha }{(1-\alpha )2\pi \Delta _{T}}}.$$

If α = 0.5, then the *RC* time constant equals the sampling period. If $\alpha \ll 0.5$, then *RC* is significantly larger than the sampling interval, and $\Delta _{T}\approx \alpha RC.$

The filter recurrence relation provides a way to determine the output samples in terms of the input samples and the preceding output. The following [pseudocode](<https://en.wikipedia.org/wiki/Pseudocode>) algorithm simulates the effect of a low-pass filter on a series of digital samples:

```
// Return RC low-pass filter output samples, given input samples,
// time interval dt, and time constant RC
function lowpass(real[1..n] x, real dt, real RC)
    var real[1..n] y
    var real α := dt / (RC + dt)
    y[1] := α * x[1]
    for i from 2 to n
        y[i] := α * x[i] + (1 - α) * y[i - 1]
    return y
```

The [loop](<https://en.wikipedia.org/wiki/Programming_loop>) that calculates each of the *n* outputs can be [refactored](<https://en.wikipedia.org/wiki/Code_refactoring>) into the equivalent:

```
    for i from 2 to n
        y[i] := y[i - 1] + α * (x[i] - y[i - 1])
```

That is, the change from one filter output to the next is [proportional](<https://en.wikipedia.org/wiki/Proportionality_(mathematics)>) to the difference between the previous output and the next input. This [exponential smoothing](<https://en.wikipedia.org/wiki/Exponential_smoothing>) property matches the [exponential decay](<https://en.wikipedia.org/wiki/Exponential_decay>) seen in the continuous-time system. As expected, as the [time constant](<https://en.wikipedia.org/wiki/Time_constant>) *RC* increases, the discrete-time smoothing parameter $\alpha$ decreases, and the output samples $(y_{1},y_{2},\ldots ,y_{n})$ respond more slowly to a change in the input samples $(x_{1},x_{2},\ldots ,x_{n})$; the system has more *[inertia](<https://en.wikipedia.org/wiki/Inertia>)*. This filter is an [infinite-impulse-response](<https://en.wikipedia.org/wiki/Infinite-impulse-response>) (IIR) single-pole low-pass filter.

### Finite impulse response

\[[edit](<https://en.wikipedia.org/w/index.php?title=Low-pass_filter&action=edit&section=10>)\]

Finite-impulse-response filters can be built that approximate the [sinc function](<https://en.wikipedia.org/wiki/Sinc_function>) time-domain response of an ideal sharp-cutoff low-pass filter. For minimum distortion, the finite impulse response filter has an unbounded number of coefficients operating on an unbounded signal. In practice, the time-domain response must be time truncated and is often of a simplified shape; in the simplest case, a [running average](<https://en.wikipedia.org/wiki/Running_average>) can be used, giving a square time response.\[8\]

### Fourier transform

\[[edit](<https://en.wikipedia.org/w/index.php?title=Low-pass_filter&action=edit&section=11>)\]

| [[image: icon](<https://thumb.wikimedia.org/wikipedia/en/thumb/9/99/Question_book-new.svg/60px-Question_book-new.svg.png?utm_source=en.wikipedia.org&utm_campaign=parser&utm_content=thumbnail>)](<https://en.wikipedia.org/wiki/File:Question_book-new.svg>) | This section **does not [cite](<https://en.wikipedia.org/wiki/Wikipedia:Citing_sources>) any [sources](<https://en.wikipedia.org/wiki/Wikipedia:Verifiability>)**. Please help [improve this section](<https://en.wikipedia.org/wiki/Special:EditPage/Low-pass_filter>) by [adding citations to reliable sources](<https://en.wikipedia.org/wiki/Help:Referencing_for_beginners>). Unsourced material may be challenged and [removed](<https://en.wikipedia.org/wiki/Wikipedia:Verifiability#Burden_of_evidence>). *(March 2015)**([Learn how and when to remove this message](<https://en.wikipedia.org/wiki/Help:Maintenance_template_removal>))* |
|---|---|

For non-realtime filtering, to achieve a low pass filter, the entire signal is usually taken as a looped signal, the Fourier transform is taken, filtered in the frequency domain, followed by an inverse Fourier transform. Only O(n log(n)) operations are required compared to O(n2) for the time domain filtering algorithm.

This can also sometimes be done in real time, where the signal is delayed long enough to perform the Fourier transformation on shorter, overlapping blocks.

## Continuous-time realization

\[[edit](<https://en.wikipedia.org/w/index.php?title=Low-pass_filter&action=edit&section=12>)\]

[[image: ](<https://thumb.wikimedia.org/wikipedia/commons/thumb/c/cd/Butterworth_Filter_Orders.svg/500px-Butterworth_Filter_Orders.svg.png?utm_source=en.wikipedia.org&utm_campaign=parser&utm_content=thumbnail>)](<https://en.wikipedia.org/wiki/File:Butterworth_Filter_Orders.svg>)

Plot of the gain of Butterworth low-pass filters of orders 1 through 5, with [cutoff frequency](<https://en.wikipedia.org/wiki/Cutoff_frequency>) $\omega _{0}=1$. Note that the slope is 20*n* dB/decade where *n* is the filter order.

There are many different types of filter circuits, with different responses to changing frequency. The frequency response of a filter is generally represented using a [Bode plot](<https://en.wikipedia.org/wiki/Bode_plot>), and the filter is characterized by its [cutoff frequency](<https://en.wikipedia.org/wiki/Cutoff_frequency>) and rate of frequency [rolloff](<https://en.wikipedia.org/wiki/Roll-off>). In all cases, at the *cutoff frequency,* the filter [attenuates](<https://en.wikipedia.org/wiki/Attenuate>) the input power by half or 3 dB. So the **order** of the filter determines the amount of additional attenuation for frequencies higher than the cutoff frequency.

- A **first-order filter**, for example, reduces the signal amplitude by half (so power reduces by a factor of 4, or 6 dB), every time the frequency doubles (goes up one [octave](<https://en.wikipedia.org/wiki/Octave>)); more precisely, the power rolloff approaches 20 dB per [decade](<https://en.wikipedia.org/wiki/Decade_(log_scale)>) in the limit of high frequency. The magnitude Bode plot for a first-order filter looks like a horizontal line below the [cutoff frequency](<https://en.wikipedia.org/wiki/Cutoff_frequency>), and a diagonal line above the cutoff frequency. There is also a "knee curve" at the boundary between the two, smoothly transitioning between the two straight-line regions. If the [transfer function](<https://en.wikipedia.org/wiki/Transfer_function>) of a first-order low-pass filter has a [zero](<https://en.wikipedia.org/wiki/Zero_(complex_analysis)>) as well as a [pole](<https://en.wikipedia.org/wiki/Pole_(complex_analysis)>), the Bode plot flattens out again, at some maximum attenuation of high frequencies; such an effect is caused for example by a little bit of the input leaking around the one-pole filter; this one-pole–one-zero filter is still a first-order low-pass. *See [Pole–zero plot](<https://en.wikipedia.org/wiki/Pole–zero_plot>) and [RC circuit](<https://en.wikipedia.org/wiki/RC_circuit>).*
- A **second-order filter** attenuates high frequencies more steeply. The Bode plot for this type of filter resembles that of a first-order filter, except that it falls off more quickly. For example, a second-order [Butterworth filter](<https://en.wikipedia.org/wiki/Butterworth_filter>) reduces the signal amplitude to one-fourth of its original level every time the frequency doubles (so power decreases by 12 dB per octave, or 40 dB per decade). Other all-pole second-order filters may roll off at different rates initially depending on their [Q factor](<https://en.wikipedia.org/wiki/Q_factor>), but approach the same final rate of 12 dB per octave; as with the first-order filters, zeroes in the transfer function can change the high-frequency asymptote. See [RLC circuit](<https://en.wikipedia.org/wiki/RLC_circuit>).
- Third- and higher-order filters are defined similarly. In general, the final rate of power rolloff for an order- n all-pole filter is 6n dB per octave (20n dB per decade).

On any Butterworth filter, if one extends the horizontal line to the right and the diagonal line to the upper-left (the [asymptotes](<https://en.wikipedia.org/wiki/Asymptote>) of the function), they intersect at exactly the *cutoff frequency*, 3 dB below the horizontal line. The various types of filters ([Butterworth filter](<https://en.wikipedia.org/wiki/Butterworth_filter>), [Chebyshev filter](<https://en.wikipedia.org/wiki/Chebyshev_filter>), [Bessel filter](<https://en.wikipedia.org/wiki/Bessel_filter>), etc.) all have different-looking *knee curves*. Many second-order filters have "peaking" or [resonance](<https://en.wikipedia.org/wiki/Electrical_resonance>) that puts their frequency response *above* the horizontal line at this peak.

The meanings of 'low' and 'high'—that is, the [cutoff frequency](<https://en.wikipedia.org/wiki/Cutoff_frequency>)—depend on the characteristics of the filter. The term "low-pass filter" merely refers to the shape of the filter's response; a high-pass filter could be built that cuts off at a lower frequency than any low-pass filter—it is their responses that set them apart. Electronic circuits can be devised for any desired frequency range, right up through microwave frequencies (above 1 GHz) and higher.

### Laplace notation

\[[edit](<https://en.wikipedia.org/w/index.php?title=Low-pass_filter&action=edit&section=13>)\]

Continuous-time filters can also be described in terms of the [Laplace transform](<https://en.wikipedia.org/wiki/Laplace_transform>) of their [impulse response](<https://en.wikipedia.org/wiki/Impulse_response>), in a way that makes it easy to analyze all characteristics of the filter by considering the pattern of poles and zeros of the Laplace transform in the complex plane. (In discrete time, one can similarly consider the [Z-transform](<https://en.wikipedia.org/wiki/Z-transform>) of the impulse response.)

For example, a first-order low-pass filter can be described by the [continuous time](<https://en.wikipedia.org/wiki/Continuous_time>) [transfer function](<https://en.wikipedia.org/wiki/Transfer_function>), in the [Laplace domain](<https://en.wikipedia.org/wiki/Laplace_transform>), as $$H(s)={\frac {\text{output}}{\text{input}}}=K{\frac {1}{\tau s+1}}=K{\frac {\alpha }{s+\alpha }},$$ where *H* is the transfer function, *s* is the Laplace-transform variable (complex angular frequency), *τ* is the filter [time constant](<https://en.wikipedia.org/wiki/Time_constant>), $\alpha$ is the cutoff frequency, and *K* is the [gain](<https://en.wikipedia.org/wiki/Gain_(electronics)>) of the filter in the [passband](<https://en.wikipedia.org/wiki/Passband>). The cutoff frequency is related to the time constant as $$\alpha ={\frac {1}{\tau }}.$$

## Electronic low-pass filters

\[[edit](<https://en.wikipedia.org/w/index.php?title=Low-pass_filter&action=edit&section=14>)\]

Further information: [Electronic filter](<https://en.wikipedia.org/wiki/Electronic_filter>)

### First-order passive

\[[edit](<https://en.wikipedia.org/w/index.php?title=Low-pass_filter&action=edit&section=15>)\]

#### RC filter

\[[edit](<https://en.wikipedia.org/w/index.php?title=Low-pass_filter&action=edit&section=16>)\]

Main article: [RC circuit § Series circuit](<https://en.wikipedia.org/wiki/RC_circuit#Series_circuit>)

[[image: ](<https://thumb.wikimedia.org/wikipedia/commons/thumb/3/3b/RC_Divider.svg/250px-RC_Divider.svg.png?utm_source=en.wikipedia.org&utm_campaign=parser&utm_content=thumbnail>)](<https://en.wikipedia.org/wiki/File:RC_Divider.svg>)

Passive first-order low-pass RC filter

One simple low-pass filter [circuit](<https://en.wikipedia.org/wiki/Electrical_circuit>) consists of a [resistor](<https://en.wikipedia.org/wiki/Resistor>) in series with a [load](<https://en.wikipedia.org/wiki/External_electric_load>), and a [capacitor](<https://en.wikipedia.org/wiki/Capacitor>) in parallel with the load. The capacitor exhibits [reactance](<https://en.wikipedia.org/wiki/Reactance_(electronics)>) and blocks low-frequency signals, forcing them through the load instead. At higher frequencies, the reactance drops, and the capacitor effectively functions as a short circuit. The combination of resistance and capacitance gives the [time constant](<https://en.wikipedia.org/wiki/Time_constant>) of the filter $\tau =RC$ (represented by the Greek letter [tau](<https://en.wikipedia.org/wiki/Tau>)). The break frequency, also called the turnover frequency, corner frequency, or [cutoff frequency](<https://en.wikipedia.org/wiki/Cutoff_frequency>) (in hertz), is determined by the time constant: $$f_{\text{c}}={\frac {1}{2\pi \tau }}={\frac {1}{2\pi RC}},$$ or equivalently (in [radians](<https://en.wikipedia.org/wiki/Radian>) per second), $$\omega _{\text{c}}={\frac {1}{\tau }}={\frac {1}{RC}}.$$

This circuit may be understood by considering the time the capacitor needs to charge or discharge through the resistor:

- At low frequencies, there is plenty of time for the capacitor to charge up to practically the same voltage as the input voltage.
- At high frequencies, the capacitor only has time to charge up a small amount before the input switches direction. The output goes up and down only a small fraction of the amount the input goes up and down. At double the frequency, there's only time for it to charge up half the amount.

Another way to understand this circuit is through the concept of [reactance](<https://en.wikipedia.org/wiki/Reactance_(electronics)>) at a particular frequency:

- Since [direct current](<https://en.wikipedia.org/wiki/Direct_current>) (DC) cannot flow through the capacitor, DC input must flow out the path marked $V_{\text{out}}$ (analogous to removing the capacitor).
- Since [alternating current](<https://en.wikipedia.org/wiki/Alternating_current>) (AC) flows very well through the capacitor, almost as well as it flows through a solid wire, AC input flows out through the capacitor, effectively [short-circuiting](<https://en.wikipedia.org/wiki/Short-circuit>) to the ground (analogous to replacing the capacitor with just a wire).

The capacitor is not an "on/off" object (like the block or pass fluidic explanation above) but acts variably between these two extremes. The [Bode plot](<https://en.wikipedia.org/wiki/Bode_plot>) and [frequency response](<https://en.wikipedia.org/wiki/Frequency_response>) show this variability.

#### RL filter

\[[edit](<https://en.wikipedia.org/w/index.php?title=Low-pass_filter&action=edit&section=17>)\]

Main article: [RL circuit § Series circuit](<https://en.wikipedia.org/wiki/RL_circuit#Series_circuit>)

A resistor–inductor circuit or [RL filter](<https://en.wikipedia.org/wiki/RL_filter>) is an [electric circuit](<https://en.wikipedia.org/wiki/Electric_circuit>) composed of [resistors](<https://en.wikipedia.org/wiki/Resistor>) and [inductors](<https://en.wikipedia.org/wiki/Inductor>) driven by a [voltage](<https://en.wikipedia.org/wiki/Voltage_source>) or [current source](<https://en.wikipedia.org/wiki/Current_source>). A first-order RL circuit is composed of one resistor and one inductor and is the simplest type of RL circuit.

A first-order RL circuit is one of the simplest [analogue](<https://en.wikipedia.org/wiki/Analog_filter>) [infinite impulse response](<https://en.wikipedia.org/wiki/Infinite_impulse_response>) [electronic filters](<https://en.wikipedia.org/wiki/Electronic_filter>). It consists of a [resistor](<https://en.wikipedia.org/wiki/Resistor>) and an [inductor](<https://en.wikipedia.org/wiki/Inductor>), either in [series](<https://en.wikipedia.org/wiki/Series_and_parallel_circuits#Series_circuits>) driven by a [voltage source](<https://en.wikipedia.org/wiki/Voltage_source>) or in [parallel](<https://en.wikipedia.org/wiki/Series_and_parallel_circuits#Parallel_circuits>) driven by a current source.

### Second-order passive

\[[edit](<https://en.wikipedia.org/w/index.php?title=Low-pass_filter&action=edit&section=18>)\]

#### RLC filter

\[[edit](<https://en.wikipedia.org/w/index.php?title=Low-pass_filter&action=edit&section=19>)\]

[[image: ](<https://thumb.wikimedia.org/wikipedia/commons/thumb/7/7d/RLC_low-pass.svg/250px-RLC_low-pass.svg.png?utm_source=en.wikipedia.org&utm_campaign=parser&utm_content=thumbnail>)](<https://en.wikipedia.org/wiki/File:RLC_low-pass.svg>)

RLC circuit as a low-pass filter

An [RLC circuit](<https://en.wikipedia.org/wiki/RLC_circuit>) (the letters R, L, and C can be in a different sequence) is an [electrical circuit](<https://en.wikipedia.org/wiki/Electrical_circuit>) consisting of a [resistor](<https://en.wikipedia.org/wiki/Resistor>), an [inductor](<https://en.wikipedia.org/wiki/Inductor>), and a [capacitor](<https://en.wikipedia.org/wiki/Capacitor>), connected in series or in parallel. The RLC part of the name is due to those letters being the usual electrical symbols for [resistance](<https://en.wikipedia.org/wiki/Electrical_resistance>), [inductance](<https://en.wikipedia.org/wiki/Inductance>), and [capacitance](<https://en.wikipedia.org/wiki/Capacitance>), respectively. The circuit forms a [harmonic oscillator](<https://en.wikipedia.org/wiki/Harmonic_oscillator>) for current and will [resonate](<https://en.wikipedia.org/wiki/Resonance>) in a similar way as an [LC circuit](<https://en.wikipedia.org/wiki/LC_circuit>) will. The main difference that the presence of the resistor makes is that any oscillation induced in the circuit will die away over time if it is not kept going by a source. This effect of the resistor is called [damping](<https://en.wikipedia.org/wiki/Damping>). The presence of the resistance also reduces the peak resonant frequency somewhat. Some resistance is unavoidable in real circuits, even if a resistor is not specifically included as a component. An ideal, pure LC circuit is an abstraction for the purpose of theory.

There are many applications for this circuit. They are used in many different types of [oscillator circuits](<https://en.wikipedia.org/wiki/Electronic_oscillator>). Another important application is for [tuning](<https://en.wikipedia.org/wiki/Tuner_(electronics)>), such as in [radio receivers](<https://en.wikipedia.org/wiki/Receiver_(radio)>) or [television sets](<https://en.wikipedia.org/wiki/Television_set>), where they are used to select a narrow range of frequencies from the ambient radio waves. In this role, the circuit is often called a tuned circuit. An RLC circuit can be used as a [band-pass filter](<https://en.wikipedia.org/wiki/Band-pass_filter>), [band-stop filter](<https://en.wikipedia.org/wiki/Band-stop_filter>), low-pass filter, or [high-pass filter](<https://en.wikipedia.org/wiki/High-pass_filter>). The RLC filter is described as a *second-order* circuit, meaning that any voltage or current in the circuit can be described by a second-order [differential equation](<https://en.wikipedia.org/wiki/Differential_equation>) in circuit analysis.

#### Second-order low-pass filter in standard form

\[[edit](<https://en.wikipedia.org/w/index.php?title=Low-pass_filter&action=edit&section=20>)\]

The transfer function $H_{\text{LP}}(f)$ of a second-order low-pass filter can be expressed as a function of frequency $f$ as the second-order low-pass filter standard form: $$H_{\text{LP}}(f)=-{\frac {K}{f_{\text{FSF}}\cdot f_{\text{c}}^{2}+{\frac {1}{Q}}\cdot jf_{\text{FSF}}\cdot f_{\text{c}}+1}}.$$ In this equation, $f$ is the frequency variable, $f_{\text{c}}$ is the cutoff frequency, $f_{\text{FSF}}$ is the frequency scaling factor, and $Q$ is the quality factor. The equation describes three regions of operation: below cutoff, in the area of cutoff, and above cutoff. For each area, it reduces to:

- $f\ll f_{\text{c}}$: $H_{\text{LP}}(f)\approx K$ –  the circuit passes signals multiplied by the gain factor $K$.
- ${\frac {f}{f_{\text{c}}}}=f_{\text{FSF}}$: $H_{\text{LP}}(f)=jKQ$ –  signals are phase-shifted 90° and modified by the quality factor $Q$.
- $f\gg f_{\text{c}}$: $H_{\text{LP}}(f)\approx -{\frac {K}{f_{\text{FSF}}\cdot f^{2}}}$ –  signals are phase-shifted 180° and attenuated by the square of the frequency ratio. This behavior is detailed by Jim Karki in "Active Low-Pass Filter Design".\[9\]

With attenuation at frequencies above $f_{\text{c}}$ increasing by a power of two, the last formula describes a second-order low-pass filter. The frequency scaling factor $f_{\text{FSF}}$ is used to scale the cutoff frequency of the filter so that it follows the definitions given before.

### Higher-order passive filters

\[[edit](<https://en.wikipedia.org/w/index.php?title=Low-pass_filter&action=edit&section=21>)\]

Higher-order passive filters can also be constructed (see diagram for a third-order example).

[[image: ](<https://thumb.wikimedia.org/wikipedia/commons/thumb/d/dc/LowPass3poleICauer.svg/500px-LowPass3poleICauer.svg.png?utm_source=en.wikipedia.org&utm_campaign=parser&utm_content=thumbnail>)](<https://en.wikipedia.org/wiki/File:LowPass3poleICauer.svg>)

A third-order low-pass filter ([Cauer topology](<https://en.wikipedia.org/wiki/Cauer_topology>)). The filter becomes a Butterworth filter with [cutoff frequency](<https://en.wikipedia.org/wiki/Cutoff_frequency>) ωc = 1 when (for example) *C*2 = 4/3 farad, *R*4 = 1 ohm, *L*1 = 3/2 henry and *L*3 = 1/2 henry.

### First-order active

\[[edit](<https://en.wikipedia.org/w/index.php?title=Low-pass_filter&action=edit&section=22>)\]

See also: [operational amplifier applications § Inverting integrator](<https://en.wikipedia.org/wiki/Operational_amplifier_applications#Inverting_integrator>)

[[image: ](<https://thumb.wikimedia.org/wikipedia/commons/thumb/5/59/Active_Lowpass_Filter_RC.svg/250px-Active_Lowpass_Filter_RC.svg.png?utm_source=en.wikipedia.org&utm_campaign=parser&utm_content=thumbnail>)](<https://en.wikipedia.org/wiki/File:Active_Lowpass_Filter_RC.svg>)

An active low-pass filter

An *active* low-pass filter adds an [active device](<https://en.wikipedia.org/wiki/Active_device>) to create an [active filter](<https://en.wikipedia.org/wiki/Active_filter>) that allows for [gain](<https://en.wikipedia.org/wiki/Gain_(electronics)>) in the passband.

In the [operational amplifier](<https://en.wikipedia.org/wiki/Operational_amplifier>) circuit shown in the figure, the cutoff frequency (in [hertz](<https://en.wikipedia.org/wiki/Hertz>)) is defined as $$f_{\text{c}}={\frac {1}{2\pi R_{2}C}},$$ or equivalently (in radians per second), $$\omega _{\text{c}}={\frac {1}{R_{2}C}}.$$

The gain in the passband is −*R*2/*R*1, and the [stopband](<https://en.wikipedia.org/wiki/Stopband>) drops off at −6 dB per octave (that is, −20 dB per decade) as it is a first-order filter.

## See also

\[[edit](<https://en.wikipedia.org/w/index.php?title=Low-pass_filter&action=edit&section=23>)\]

- [Baseband](<https://en.wikipedia.org/wiki/Baseband>)
- [Smoother (statistics)](<https://en.wikipedia.org/wiki/Smoother_(statistics)>)

## References

\[[edit](<https://en.wikipedia.org/w/index.php?title=Low-pass_filter&action=edit&section=24>)\]

1. 1 2 *[*Long Pass Filters and Short Pass Filters Information*](<http://www.globalspec.com/learnmore/optics_optical_components/optical_components/long_short_pass_filters>), retrieved 2017-10-04*
2. ↑ *[Sedra, Adel](<https://en.wikipedia.org/wiki/Adel_Sedra>); Smith, Kenneth C. (1991). [*Microelectronic Circuits, 3 ed*](<https://archive.org/details/microelectronicc00sedr_0/page/60>). Saunders College Publishing. p. [60](<https://archive.org/details/microelectronicc00sedr_0/page/60>). [ISBN](<https://en.wikipedia.org/wiki/ISBN_(identifier)>) [0-03-051648-X](<https://en.wikipedia.org/wiki/Special:BookSources/0-03-051648-X>).*
3. ↑ *["ADSL filters explained"](<http://www.epanorama.net/documents/telecom/adsl_filter.html>). Epanorama.net. Retrieved 2013-09-24.*
4. ↑ *["Home Networking – Local Area Network"](<https://web.archive.org/web/20130927135123/http://www.pcweenie.com/hni/broadband/broad6.shtml>). Pcweenie.com. 2009-04-12. Archived from [the original](<http://www.pcweenie.com/hni/broadband/broad6.shtml>) on 2013-09-27. Retrieved 2013-09-24.*
5. ↑ [Mastering Windows: Improving Reconstruction](<http://www.cg.tuwien.ac.at/research/vis/vismed/Windows/MasteringWindows.pdf>)
6. 1 2 *Hayt, William H. Jr.; Kemmerly, Jack E. (1978). *Engineering Circuit Analysis*. New York: McGraw Hill. pp. 211–224, 684–729.*
7. ↑ *Boyce, William; DiPrima, Richard (1965). *Elementary Differential Equations and Boundary Value Problems*. New York: Wiley. pp. 11–24.*
8. ↑ Whilmshurst, T. H. (1990) *Signal recovery from noise in electronic instrumentation*. [ISBN](<https://en.wikipedia.org/wiki/ISBN_(identifier)>) [9780750300582](<https://en.wikipedia.org/wiki/Special:BookSources/9780750300582>).
9. ↑ ["Active Low-Pass Filter Design"](<https://www.ti.com/lit/an/sloa049d/sloa049d.pdf>), Texas Instruments, 2023.

## External links

\[[edit](<https://en.wikipedia.org/w/index.php?title=Low-pass_filter&action=edit&section=25>)\]

[[image: Wikimedia Commons logo](<https://thumb.wikimedia.org/wikipedia/en/thumb/4/4a/Commons-logo.svg/40px-Commons-logo.svg.png?utm_source=en.wikipedia.org&utm_campaign=parser&utm_content=thumbnail>)](<https://en.wikipedia.org/wiki/File:Commons-logo.svg>)

Wikimedia Commons has media related to [Lowpass filters](<https://commons.wikimedia.org/wiki/Category:Lowpass%20filters>).

- [Low Pass Filter java simulator](<http://www.st-andrews.ac.uk/~www_pa/Scots_Guide/experiment/lowpass/lpf.html>)
- [ECE 209: Review of Circuits as LTI Systems](<http://www.tedpavlic.com/teaching/osu/ece209/support/circuits_sys_review.pdf>), a short primer on the mathematical analysis of (electrical) LTI systems.
- [ECE 209: Sources of Phase Shift](<http://www.tedpavlic.com/teaching/osu/ece209/lab3_opamp_FO/lab3_opamp_FO_phase_shift.pdf>), an intuitive explanation of the source of phase shift in a low-pass filter. Also verifies simple passive LPF [transfer function](<https://en.wikipedia.org/wiki/Transfer_function>) by means of trigonometric identity.
- [C code generator](<https://relayman.org/fisher/trad.html>) for digital implementation of Butterworth, Bessel, and Chebyshev filters created by the late Dr. Tony Fisher of the University of York (York, England).

Retrieved from "[https://en.wikipedia.org/w/index.php?title=Low-pass\_filter&oldid=1380674115](<https://en.wikipedia.org/w/index.php?title=Low-pass_filter&oldid=1380674115>)"

[Categories](<https://en.wikipedia.org/wiki/Help:Category>):

- [Signal processing](<https://en.wikipedia.org/wiki/Category:Signal_processing>)
- [Linear filters](<https://en.wikipedia.org/wiki/Category:Linear_filters>)
- [Synthesiser modules](<https://en.wikipedia.org/wiki/Category:Synthesiser_modules>)
- [Filter frequency response](<https://en.wikipedia.org/wiki/Category:Filter_frequency_response>)
- [Acoustics](<https://en.wikipedia.org/wiki/Category:Acoustics>)
- [Sound](<https://en.wikipedia.org/wiki/Category:Sound>)

Hidden categories:

- [Articles with short description](<https://en.wikipedia.org/wiki/Category:Articles_with_short_description>)
- [Short description is different from Wikidata](<https://en.wikipedia.org/wiki/Category:Short_description_is_different_from_Wikidata>)
- [Articles needing additional references from May 2023](<https://en.wikipedia.org/wiki/Category:Articles_needing_additional_references_from_May_2023>)
- [All articles needing additional references](<https://en.wikipedia.org/wiki/Category:All_articles_needing_additional_references>)
- [All articles with unsourced statements](<https://en.wikipedia.org/wiki/Category:All_articles_with_unsourced_statements>)
- [Articles with unsourced statements from August 2020](<https://en.wikipedia.org/wiki/Category:Articles_with_unsourced_statements_from_August_2020>)
- [Vague or ambiguous geographic scope from October 2026](<https://en.wikipedia.org/wiki/Category:Vague_or_ambiguous_geographic_scope_from_October_2026>)
- [Articles needing additional references from March 2015](<https://en.wikipedia.org/wiki/Category:Articles_needing_additional_references_from_March_2015>)
- [Commons category link is on Wikidata](<https://en.wikipedia.org/wiki/Category:Commons_category_link_is_on_Wikidata>)


<!-- sllmwiki:original-source -->
## Original source

- URL: <https://en.wikipedia.org/wiki/Low-pass_filter>
- License: [CC-BY-SA-4.0](<https://creativecommons.org/licenses/by-sa/4.0/>)
