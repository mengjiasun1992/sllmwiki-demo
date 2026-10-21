## Page 1

Incremental calculation of weighted mean and variance

Tony Finch
fanf2@cam.ac.uk dot@dotat.at
University of Cambridge Computing Service

February 2009

Abstract
In these notes I explain how to derive formulae for numerically stable calculation of the mean
and standard deviation, which are also suitable for incremental on-line calculation. I then generalize
these formulae to weighted means and standard deviations. I unpick the difficulties that arise when
generalizing further to normalized weights. Finally I show that the exponentially weighted moving
average is a special case of the incremental normalized weighted mean formula, and derive a formula
for the exponentially weighted moving standard deviation.

1 Simple mean

Straightforward translation of equation 1 into code can suffer from loss of precision because of the
difference in magnitude between a sample and the sum of all samples. Equation 4 calculates the mean
in a way that is more numerically stable because it avoids accumulating large sums.

μn =

1
n

n

i=1

xi (1)

=

1
n

(xn +

n−1

i=1

xi) (2)

=

1
n

(xn + (n − 1)μn−1) (3)

= μn−1 +

1
n

(xn − μn−1) (4)

This formula also provides us with some useful identities.
xn − μn−1 = n(μn − μn−1) (5)
xn − μn = n(μn − μn−1) − μn + μn−1
= (n − 1)(μn − μn−1) (6)

2 Simple variance

The definition of the standard deviation in equation 7 below requires us to already know the mean,
which implies two passes over the data. This isn’t feasible for online algorithms that need to produce
incremental results after each sample becomes available. Equation 12 solves this problem since it allows
us to calculate the standard deviation from two running sums.

σ2 =

1
n

n

i=1

(xi − μ)2 (7)

=

1
n

n

i=1

(x2i − 2xiμ + μ2) (8)

1

## Page 2

=

1
n

n
i=1

x2i − 2μ

1
n

n
i=1

xi + μ2

1
n

n
i=1

1 (9)

=

1
n

n
i=1

x2i − 2μμ + μ2

n
n

(10)

=

1
n

n
i=1

x2i − μ2 (11)

=

1
n

n
i=1

x2i
−

1
n

n
i=1

xi

2
(12)

3 Incremental variance
Knuth notes [1] that equation 12 is prone to loss of precision because it takes the difference between two
large sums of similar size, and suggests equation 24 as an alternative that avoids this problem. However
he does not say how it is derived. In the following, equation 20 is derived from the previous step using
equation 5.
Let Sn = nσ2n (13)
=

n
i=1(xi − μn)

2 (14)
=

n
i=1 x

2
i − nμ

2
n (15)
Sn − Sn−1 =

n
i=1

x2i − nμ2n −

n−1
i=1

x2i + (n − 1)μ2n−1 (16)
= x2n − nμ2n + (n − 1)μ2n−1 (17)
= x2n − μ2n−1 + n(μ2n−1 − μ2n) (18)
= x2n − μ2n−1 + n(μn−1 − μn)(μn−1 + μn) (19)
= x2n − μ2n−1 + (μn−1 − xn)(μn−1 + μn) (20)
= x2n − μ2n−1 + μ2n−1 − xnμn − xnμn−1 + μnμn−1 (21)
= x2n − xnμn − xnμn−1 + μnμn−1 (22)
= (xn − μn−1)(xn − μn) (23)
Sn = Sn−1 + (xn − μn−1)(xn − μn) (24)
σn =

Sn/n (25)
Mathworld [2] has an alternative derivation of a similar formula, which in our notation is as follows.
Sn =

n
i=1

(xi − μn)2 (26)

=

n
i=1

((xi − μn−1) − (μn − μn−1))2 (27)

=

n
i=1

(xi − μn−1)2 +

n
i=1

(μn − μn−1)2 − 2

n
i=1

(xi − μn−1)(μn − μn−1) (28)

Simplify the first summation:
n
i=1

(xi − μn−1)2 = (xn − μn−1)2 +

n−1
i=1

(xi − μn−1)2 (29)
= (xn − μn−1)2 + Sn−1 (30)
= Sn−1 + n2(μn − μn−1)2 (31)

2

## Page 3

Simplify the second summation:
n
i=1

(μn − μn−1)2 = n(μn − μn−1)2 (32)

Simplify the third summation:
n
i=1

(xi − μn−1)(μn − μn−1) = (μn − μn−1)

n
i=1

(xi − μn−1) (33)

= (μn − μn−1)
xn − μn−1 +

n−1
i=1

(xi − μn−1)
(34)

= (μn − μn−1)

xn − μn−1 − (n − 1)μn−1 +

n−1
i=1

xi
(35)
= (μn − μn−1)(xn − μn−1 − (n − 1)μn−1 + (n − 1)μn−1) (36)
= (μn − μn−1)(xn − μn−1) (37)
= n(μn − μn−1)2 (38)
Back to the complete formula:
Sn = Sn−1 + n2(μn − μn−1)2 + n(μn − μn−1)2 − 2n(μn − μn−1)2 (39)
= Sn−1 + n2(μn − μn−1)2 − n(μn − μn−1)2 (40)
= Sn−1 + n(n − 1)(μn − μn−1)2 (41)
We can use equations 6 and 5 to show this is equivalent to equation 24.
Sn = Sn−1 + n(n − 1)(μn − μn−1)2 (42)
= Sn−1 + n(μn − μn−1)(xn − μn) (43)
= Sn−1 + (xn − μn−1)(xn − μn) (44)

4 Weighted mean
The weighted mean is defined as follows.
μ =

n
i=1 wixi
n
i=1 wi

(45)
It is equivalent to the simple mean when all the weights are equal, since
μ =

n
i=1 wxi
n
i=1 w

=

w

n
i=1 xi
nw

=

1
n

n
i=1

xi (46)

If the samples are all different, then weights can be thought of as sample frequencies, or they can be
used to calculate probabilities where pi = wi/

wi. The following derivation of the incremental formula
(equation 53) follows the same pattern as the derivation of equation 4. For brevity we also define Wn as
the sum of the weights.
Wn =

n
i=1

wi (47)

μn =

1
Wn

n
i=1

wixi (48)

=

1
Wn

wnxn +

n−1
i=1

wixi
(49)

3

## Page 4

=

1
Wn

(wnxn + Wn−1μn−1) (50)
=

1
Wn

(wnxn + (Wn − wn)μn−1) (51)
=

1
Wn

(Wnμn−1 + wnxn − wnμn−1) (52)
= μn−1 +

wn
Wn

(xn − μn−1) (53)
Useful identities derived from this formula are:
Wn(μn−1 − μn) = wn(μn−1 − xn) (54)
Wn
wn

(μn − μn−1) = xn − μn−1 (55)
xn − μn =

Wn
wn

(μn − μn−1) − μn + μn−1
=

Wn − wn
wn

(μn − μn−1) (56)
=

Wn − wn
Wn

(xn − μn−1) (57)

5 Weighted variance
Similarly, we derive a numerically stable formula for calculating the weighted variance (equation 68)
using the same pattern as the derivation of the unweighed wersion (equation 24).
σ2 =

1
Wn

n
i=1

wi(xi − μ)2 =

1
Wn

n
i=1

wix2i − μ2 (58)
Let Sn = Wnσ2n (59)
=

n
i=1

wix2i − Wnμ2n (60)

Sn − Sn−1 =

n
i=1

wix2i − Wnμ2n −

n−1
i=1

wix2i + Wn−1μ2n−1 (61)
= wnx2n − Wnμ2n + Wn−1μ2n−1 (62)
= wnx2n − Wnμ2n + (Wn − wn)μ2n−1 (63)
= wn
x2n − μ2n−1
+ Wn(μ2n−1 − μ2n) (64)
= wn
x2n − μ2n−1
+ Wn(μn−1 − μn)(μn−1 + μn) (65)
= wn
x2n − μ2n−1 + (μn−1 − xn)(μn−1 + μn)
(66)
= wn(xn − μn−1)(xn − μn) (67)
Sn = Sn−1 + wn(xn − μn−1)(xn − μn) (68)
σn =
Sn/Wn (69)

6 Variable weights
In the previous three sections, I have assumed that weights are constant once assigned. However a
common requirement is to normalize the weights, such that
W =

n
i=1

wi = 1 (70)
If we are repeatedly adding new data to our working set, then we can’t have both constant weights
and normalized weights. To allow us to keep weights normalized, we need to allow the weight of each
4

## Page 5

sample to change as the set of samples changes. To indicate this we will give weights two indices, the
first identifying the set of samples using the sample count (as we have been doing for μn etc.) and the
second being the index of the sample in that set. We will not make any assumptions about the sum of
the weights, that is we will not require them to be normalized. For example,

Wn =

n

i=1

wn,i, μn =

1
Wn

n

i=1

wn,ixi (71)

Having done this we need to re-examine some of the logical steps in the previous sections to ensure they
are still valid. In equations 49–51, we used the fact that in the fixed-weight setting,
n−1

i=1

wixi = Wn−1μn−1 = (Wn − wn)μn−1 (72)

In the new setting, this equality is fairly obviously no longer true. (For example, if we are keeping
weights normalized then Wn = Wn−1 = 1.) Fortunately there is a different middle step which justifies
equation 72 when weights vary, so the results of section 4 remain valid.
n−1

i=1

wn,ixi = (Wn − wn,n)μn−1 (73)

n−1

i=1

wn,ixi =

n−1

i=1

wn,i

n−1
i=1 wn−1,ixi
n−1
i=1 wn−1,i

(74)
n−1
i=1 wn,ixi
n−1
i=1 wn,i

=

n−1
i=1 wn−1,ixi
n−1
i=1 wn−1,i

(75)

n−1

i=1

wn,i
n−1
i=1 wn,i

xi =

n−1

i=1

wn−1,i
n−1
i=1 wn−1,i

xi (76)

wn,j
n−1
i=1 wn,i

=

wn−1,j
n−1
i=1 wn−1,i

where 1 ≤ j ≤ n − 1 (77)

This says that for the weighted mean formulae to remain valid the new and old weights should be
consistent. Equation 75 says that we get the same result when we calculate the mean of the previous
working set whether we use the old weights or the new weights. Equation 77 says that when we normalize
the weights across the previous set (up to n − 1) we get the same set of weights whether we start from
the old weights or the new ones. This requirement isn’t enough by itself to make the weighted variance
formulae work, so we will examine it again below.

7 The expectation function

At this point it is worth defining some better notation to reduce the number of summations we need to
write. The expectation function is a generalized version of the mean, whose argument is some arbitrary
function of each sample.

En(f(x)) =

1
Wn

n

i=1

wn,if(xi) (78)

En(k) = k (79)
En(af(x)) = aEn(f(x)) (80)
En(f(x) + g(x)) = En(f(x)) + En(g(x)) (81)
μn = En(x) (82)
σ2n = En((x − μn)2) (83)
= En(x2 + μ2n − 2μnx) (84)
= En(x2) + μ2n − 2μnEn(x) (85)

5

## Page 6

= En(x2) − μ2n (86)
= En(x2) − En(x)2 (87)
The incremental formula is derived in the usual way. Equation 92 is particularly useful.
WnEn(f(x)) =

n
i=1

wn,if(xi) (88)

= wn,nf(xn) +

n−1
i=1

wn,if(xi) (89)

= wn,nf(xn) + (Wn − wn,n)

n−1
i=1 wn,if(xi)
n−1
i=1 wn,i

(90)

= wn,nf(xn) + (Wn − wn,n)

n−1
i=1 wn−1,if(xi)
n−1
i=1 wn−1,i

(91)
= wn,nf(xn) + (Wn − wn,n)En−1(f(x)) (92)
En(f(x)) = En−1(f(x)) +

wn,n
Wn

(f(xn) − En−1(f(x))) (93)

8 Variable-weight variance
In equations 61–63 we made the following assumptions which are not true when weights can vary.
n
i=1

wn,ix2i − Wnμ2n −

n−1
i=1

wn−1,ix2i + Wn−1μ2n−1
= wn,nx2n − Wnμ2n + Wn−1μ2n−1
= wn,nx2n − Wnμ2n + (Wn − wn,n)μ2n−1
If we try to re-do the short derivation of the incremental standard deviation formula starting from
Sn − Sn−1 then we soon get stuck. Fortunately the longer derivation shows how to made it work.
Sn = Wnσ2n (94)
= WnEn
(x − μn)2
(95)
= WnEn
([x − μn−1] − [μn − μn−1])2
(96)
= WnEn
[x − μn−1]2 + [μn − μn−1]2 − 2[x − μn−1][μn − μn−1]
(97)
= WnEn
[x − μn−1]2
+ WnEn
[μn − μn−1]2
− 2WnEn ([x − μn−1][μn − μn−1]) (98)
Simplify the first term:
WnEn
[x − μn−1]2
= wn,n[xn − μn−1]2 + (Wn − wn,n)En−1
[x − μn−1]2
(99)
= wn,n[xn − μn−1]2 + (Wn − wn,n)

Sn−1
Wn−1

(100)
=

Wn − wn,n
Wn−1

Sn−1 +

W 2n
wn,n

[μn − μn−1]2 (101)
Simplify the second term:
WnEn
[μn − μn−1]2
= Wn[μn − μn−1]2 (102)
Simplify the third term:
WnEn([x − μn−1][μn − μn−1])
= [μn − μn−1]WnEn[x − μn−1] (103)

6

## Page 7

= [μn − μn−1] (wn,n[xn − μn−1] + (Wn − wn,n)En−1[x − μn−1]) (104)
= [μn − μn−1] (wn,n[xn − μn−1] + (Wn − wn,n)[En−1(x) − En−1(μn−1)]) (105)
= [μn − μn−1] (wn,n[xn − μn−1] + (Wn − wn,n)[μn−1 − μn−1]) (106)
= [μn − μn−1]wn,n[xn − μn−1] (107)
= Wn[μn − μn−1]2 (108)

Back to the complete formula:

Sn =

Wn − wn,n
Wn−1

Sn−1 +

W 2n
wn,n

[μn − μn−1]2 + Wn[μn − μn−1]2 − 2Wn[μn − μn−1]2 (109)

=

Wn − wn,n
Wn−1

Sn−1 +

W 2n
wn,n

[μn − μn−1]2 −

Wnwn,n
wn,n

[μn − μn−1]2 (110)

=

Wn − wn,n
Wn−1

Sn−1 + (Wn − wn,n)

Wn
wn,n

[μn − μn−1]2 (111)

=

Wn − wn,n
Wn−1

Sn−1 + (Wn − wn,n)[μn − μn−1](xn − μn−1) (112)

Sn =

Wn − wn,n
Wn−1

Sn−1 + wn,n(xn − μn)(xn − μn−1) (113)

This is the same as equation 68, except for the multiplier

Wn−wn,n
Wn−1 which captures the change in weights
between the old and new sets.
Wn − wn,n
Wn−1

=

n−1
i=1 wn,i
n−1
i=1 wn−1,i

=

wn,j
wn−1,j

where 1 ≤ j ≤ n − 1 (114)

Now that we know the rescaling trick which makes it work, we can write down the short version.

Sn −

Wn − wn,n
Wn−1

Sn−1

= Wn
En(x2) − μ2n
− (Wn − wn,n)
En−1(x2) − μ2n−1
(115)
= Wn
En(x2) − μ2n
− WnEn(x2) + wn,nx2n + (Wn − wn,n)μ2n−1 (116)
= wn,nx2n − Wnμ2n + (Wn − wn,n)μ2n−1 (117)
= wn,n
x2n − μ2n−1
+ Wn(μ2n−1 − μ2n) (118)
= wn,n
x2n − μ2n−1
+ Wn(μn−1 − μn)(μn−1 + μn) (119)
= wn,n
x2n − μ2n−1 + (μn−1 − xn)(μn−1 + μn)
(120)
= wn,n(xn − μn)(xn − μn−1) (121)

9 Exponentially-weighted mean and variance

Starting from equation 53, let’s set wn,n/Wn to a constant 0 < α < 1 and let a = 1 − α. This produces
the standard formula for the exponentially weighted moving average.

μn = μn−1 + α(xn − μn−1) (122)
= (1 − α)μn−1 + αxn (123)
= aμn−1 + (1 − a)xn (124)

In the following it’s more convenient to use a lower bound of 0 instead of 1, i.e. 0 ≤ i ≤ n. We are
going to show that the weights are renormalized each time a datum is added. First, we expand out the
inductive definition of the mean.

μn = aμn−1 + (1 − a)xn (125)
= a2μn−2 + a(1 − a)xn−1 + (1 − a)xn (126)

7

## Page 8

= a3μn−3 + a2(1 − a)xn−2 + a(1 − a)xn−1 + (1 − a)xn (127)
μn = anx0 +

n
i=1

an−i(1 − a)xi (128)

This allows us to write down the weights directly. Note that wn,n is independent of n.
wn,0 = an (129)
wn,i = an−i(1 − a), for 1 ≤ i ≤ n (130)
wn,n = 1 − a = α (131)
Since wn,n = α = wn,n/Wn we can see that n. Wn = 1, that is, the weights are always normalized. We
can get the same result by summing the geometric series.
n
i=1

an−i =

n−1
j=0

aj =

1 − an
1 − a

(132)
n
i=1

wn,i =

n
i=1

an−i(1 − a) = 1 − an (133)

Wn = wn,0 +

n
i=1

wn,i = an + (1 − an) = 1 (134)

These weights satisfy the consistency requirement because
wn,j
n−1
i=1 wn,i

=

awn−1,j
n−1
i=1 awn−1,i

=

wn−1,j
n−1
i=1 wn−1,i

(135)
We can use the expectation function to write down the naıve formula for the variance.
En(f(x)) = En−1(f(x)) +

wn,n
Wn

(f(xn) − En−1(f(x))) (136)
= En−1(f(x)) + α(f(xn) − En−1(f(x))) (137)
En(x2) = En−1(x2) + α(x2n − En−1(x2)) (138)
σ2n = En(x2) − μ2 (139)
So using the formula from the previous section we can write the incremental version:
Sn =

Wn − wn,n
Wn−1

Sn−1 + wn,n(xn − μn)(xn − μn−1) (140)

Sn =

1 − α
1

Sn−1 + α(xn − μn)(xn − μn−1) (141)
σ2n =

Sn
Wn

= Sn = aSn−1 + (1 − a)(xn − μn)(xn − μn−1) (142)
= (1 − α)(Sn−1 + α(xn − μn−1)2) (143)
This latter form is slightly more convenient for code:
diff := x - mean
incr := alpha * diff
mean := mean + incr
variance := (1 - alpha) * (variance + diff * incr)

References
[1] Donald E. Knuth. Seminumerical Algorithms, volume 2 of The Art of Computer Programming,
chapter 4.2.2, page 232. Addison-Wesley, Boston, third edition, 1998.
[2] Eric W. Weisstein. Sample variance computation. From Mathworld, a Wolfram web resource,
http://mathworld.wolfram.com/SampleVarianceComputation.html.

8


<!-- sllmwiki:original-source -->
## Original source

- URL: <https://fanf2.user.srcf.net/hermes/doc/antiforgery/stats.pdf>
- License: unknown
