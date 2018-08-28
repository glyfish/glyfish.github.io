---
title: Inverse CDF Sampling
key: 20180721
image: /assets/posts/inverse_cdf_sampling/weibull_sampled_distribution.png
author: Troy Stribling
permlink: /inverse_cdf_sampling.html
comments: false
---

[Inverse CDF](https://en.wikipedia.org/wiki/Inverse_transform_sampling) sampling is a method
for obtaining samples from both discrete and continuous probability distributions that requires the
CDF to be invertible.
The method assumes values of the CDF are Uniform random variables on {% katex %}[0, 1]{% endkatex %}.
CDF values are generated and used as input into the inverted CDF to obtain samples with the
distribution defined by the CDF.
<!--more-->

## Discrete Distributions

A discrete probability distribution consisting of a finite set of {% katex %}N{% endkatex %}
probability values is defined by,

{% katex display %}
\{p_1,\ p_2,\ \ldots,\ p_N\}\ \ \ \ \ (1),
{% endkatex %}

with {% katex %}p_i\ \geq\ 0, \  \forall i{% endkatex %} and {% katex %}\sum_{i=1}^N{p_i} = 1{% endkatex %}.
The CDF specifies the probability that {% katex %}i \leq n{% endkatex %} and is given by,

{% katex display %}
P(i\ \leq\ n)=P(n)=\sum_{i=1}^n{p_i},
{% endkatex %}

For a given CDF value, {% katex %}U{% endkatex %}, The equation for {% katex %}P(n){% endkatex %} can always
be inverted by evaluating it for each {% katex %}n{% endkatex %} and
searching for the smallest value of {% katex %}n{% endkatex %} that satisfies,
{% katex %}P(n)\ \geq\ U{% endkatex %}.
It follows that generated samples determined from {% katex %}U \sim \textbf{Uniform}(0, 1){% endkatex %}
will have distribution {% katex %}(1){% endkatex %} since the intervals {% katex %}P(n)-P(n-1) = p_n{% endkatex %}
are uniformly sampled.

Consider the distribution,

{% katex display %}
\left\{\frac{1}{12},\ \frac{1}{12},\ \frac{1}{6},\ \frac{1}{6},\ \frac{1}{12},\ \frac{5}{12} \right\} \ \ \ \ \ (2)
{% endkatex %}
It is shown in the following plot with its CDF. Note that the CDF is a monotonically increasing function.

<img class="post-image" src="/assets/posts/inverse_cdf_sampling/discrete_cdf.png">

A sampler using the Inverse CDF method with target distribution specified in {% katex %}(2){% endkatex %} implemented in
Python is shown below.

```python
import numpy

n = 10000
df = numpy.array([1/12, 1/12, 1/6, 1/6, 1/12, 5/12])
cdf = numpy.cumsum(df)

samples = [numpy.flatnonzero(cdf >= u)[0] for u in numpy.random.rand(n)]
```
The program first stores the CDF computed from the partial sums
{% katex %}P(n){% endkatex %} in an array. Next, CDF samples using
{% katex %}U \sim \textbf{Uniform}(0, 1){% endkatex %} are generated. Finally, for each sampled CDF value,
{% katex %}U{% endkatex %}, the array containing {% katex %}P(n){% endkatex %} is scanned for
the smallest value of {% katex %}n{% endkatex %} where {% katex %}P(n)\ \geq\ U{% endkatex %}. The resulting
values of {% katex %}n{% endkatex %} will have the target distribution {% katex %}(2){% endkatex %}. This is
shown in the figure below.

<img class="post-image" src="/assets/posts/inverse_cdf_sampling/discrete_sampled_distribution.png">

A measure of convergence of the samples to the target distribution can be obtained by comparing the cumulative
moments of the distribution computed from the samples with the target value of the moment computed analytically.
In general for a discrete distribution the first and second moments are given by,

{% katex display %}
\begin{aligned}
\mu & =  \sum_{i=1}^N ip_i\\
\sigma^2 & = \sum_{i=1}^N i^2p_i - \mu^2,
\end{aligned}
{% endkatex %}

In the following two plots the cumulative values of {% katex %}\mu{% endkatex %} and
{% katex %}\sigma{% endkatex %} computed from the samples generated are compared with the target values
using the equations above.
The first shows the convergence of {% katex %}\mu{% endkatex %} and the second the convergence of
{% katex %}\sigma{% endkatex %}. Within only {% katex %}10^3{% endkatex %} samples both
{% katex %}\mu{% endkatex %} and {% katex %}\sigma{% endkatex %} computed from samples is comparable to the
target value and by {% katex %}10^4{% endkatex %} the values are indistinguishable.

<img class="post-image" src="/assets/posts/inverse_cdf_sampling/discrete_sampled_mean_convergence.png">

<img class="post-image" src="/assets/posts/inverse_cdf_sampling/discrete_sampled_sigma_convergence.png">

The number of operations required for generating samples using Inverse CDF sampling from a discrete
distribution will scale {% katex %}O(N_{samples}N){% endkatex %} where {% katex %}N_{samples}{% endkatex %}
is the desired number of samples and {% katex %}N{% endkatex %} is the number of terms in the discrete distribution.

It is also possible to directly sample distribution {% katex %}(2){% endkatex %} using the `multinomial` sampler from `numpy`,

```python
import numpy

n = 10000
df = numpy.array([1/12, 1/12, 1/6, 1/6, 1/12, 5/12])
samples = numpy.random.multinomial(n, df, size=1)/n
```

## Continuous Distributions

A continuous probability distribution is defined by the [PDF](https://en.wikipedia.org/wiki/Probability_density_function),
{% katex %}f_X(x){% endkatex %}, where {% katex %}f_X(x) \geq 0,\ \forall x{% endkatex %} and
{% katex %}\int_{-\infty}^{\infty} f_X(x) dx = 1{% endkatex %}. The CDF is a monotonically increasing function
that specifies the probability that {% katex %}X\ \leq\ x{% endkatex %}, namely,

{% katex display %}
P(X \leq x) = F_X(x) = \int_{-\infty}^{x} f_X(w) dw.
{% endkatex %}

### Theory

To prove that Inverse CDF sampling works for continuous distributions it must be shown that,

{% katex display %}
P[F_X^{-1}(U)\ \leq\ x] = F_X(x) \ \ \ \ \ (3),
{% endkatex %}

where {% katex %}F_X^{-1}(x){% endkatex %} is the inverse of {% katex %}F_X(x){% endkatex %}
and {% katex %}U \sim \textbf{Uniform}(0, 1){% endkatex %}.

A more general result needed to complete this proof is obtained using a change of variable on a CDF.
If {% katex %}Y=G(X){% endkatex %} is a monotonically increasing invertible function
of {% katex %}X{% endkatex %} it will be shown that,

{% katex display %}
P(X\ \leq\ x) = P(Y\ \leq\ y) = P[G(X)\ \leq\ G(x)]. \ \ \ \ \ (4)
{% endkatex %}

To prove this note that {% katex %}G(x){% endkatex %} is monotonically increasing so the ordering of values is
preserved for all {% katex %}x{% endkatex %},

{% katex display %}
X\ \leq\ x\ \implies\ G(X)\ \leq\ G(x).
{% endkatex %}

Consequently, the order of the integration limits is maintained by the transformation.
Further, since {% katex %}y=G(x){% endkatex %} is invertible,
{% katex %}x = G^{-1}(y){% endkatex %} and {% katex %}dx = (dG^{-1}/dy) dy{% endkatex %}, so

{% katex display %}
\begin{aligned}
P(X\ \leq\ x) & = \int_{-\infty}^{x} f_X(w) dw \\
& = \int_{-\infty}^{y} f_X(G^{-1}(z)) \frac{dG^{-1}}{dz} dz \\
& = \int_{-\infty}^{y} f_Y(z) dz \\
& = P(Y\ \leq\ y) \\
& = P[G(X)\ \leq\ G(x)],
\end{aligned}
{% endkatex %}

where,

{% katex display %}
f_Y(y) = f_X(G^{-1}(y)) \frac{dG^{-1}}{dy}
{% endkatex %}

For completeness consider the case where {% katex %}Y=G(X){% endkatex %} is a monotonically decreasing invertible function
of {% katex %}X{% endkatex %} then,

{% katex display %}
X\ \leq\ x\ \implies\ G(X)\ \geq\ G(x),
{% endkatex %}

it follows that,

{% katex display %}
\begin{aligned}
P(X\ \leq x) & = \int_{-\infty}^{x} f_X(w) dw \\
& = \int_{y}^{\infty} f_X(G^{-1}(z)) \frac{dG^{-1}}{dz} dz \\
& = \int_{y}^{\infty} f_Y(z) dz \\
& = P(Y\ \geq\ y) \\
& = P[G(X)\ \geq\ G(x)] \\
& = 1 - P[G(X)\ \leq\ G(x)]
\end{aligned}
{% endkatex %}

The desired proof of equation {% katex %}(3){% endkatex %} follows from equation {% katex %}(4){% endkatex %}
by noting that {% katex %}U \sim \textbf{Uniform}(0, 1){% endkatex %} so {% katex %}f_U(u) = 1{% endkatex %},

{% katex display %}
\begin{aligned}
P[F_X^{-1}(U)\ \leq\ x] & = P[F_X(F_X^{-1}(U))\ \leq\ F_X(x)] \\
& = P[U\ \leq\ F_X(x)] \\
& = \int_{0}^{F_X(x)} f_U(w) dw \\
& = \int_{0}^{F_X(x)} dw \\
& = F_X(x).
\end{aligned}
{% endkatex %}

### Example

Consider the [Weibull Distribution](https://en.wikipedia.org/wiki/Weibull_distribution). The PDF is
given by,

{% katex display %}
f_X(x; k, \lambda) =
\begin{cases}
\frac{k}{\lambda}\left(\frac{x}{\lambda} \right)^{k-1} e^{\left(\frac{-x}{\lambda}\right)^k} & x\ \geq\ 0 \\
0 & x\ <\ 0,
\end{cases} \ \ \ \ \ (5)
{% endkatex %}

where {% katex %}k\ >\ 0{% endkatex %} is the shape parameter and {%katex %}\lambda\ >\ 0{% endkatex %} the scale parameter.
The CDF is given by,

{% katex display %}
F_X(x; k, \lambda) =
\begin{cases}
1-e^{\left(\frac{-x}{\lambda}\right)^k} & x\ \geq\ 0 \\
0 & x\ <\ 0.
\end{cases}
{% endkatex %}

The CDF can be inverted to yield,

{% katex display %}
F_X^{-1}(u; k, \lambda) =
\begin{cases}
\lambda\ln\left(\frac{1}{1-u}\right)^{\frac{1}{k}} & 0\ \leq\ u\ \leq 1 \\
0 & u\ <\ 0 \text{ or } u\ >\ 1.
\end{cases}
{% endkatex %}

In the example described here it will be assumed that {% katex %}k=5.0{% endkatex %} and
{% katex %}\lambda=1.0{% endkatex %}. The following plot shows the PDF and CDF using these values.

<img class="post-image" src="/assets/posts/inverse_cdf_sampling/weibull_cdf.png">

The sampler implementation for the continuous case is simpler than for the discrete case.
Just as in the discrete case CDF samples with distribution {% katex %}U \sim \textbf{Uniform}(0, 1){% endkatex %}
are generated. The desired samples with the target Weibull distribution are then computed using the CDF inverse.
Below an implementation of this procedure in Python is given.

```python
import numpy

k = 5.0
λ = 1.0
nsamples = 100000

cdf_inv = lambda u: λ * (numpy.log(1.0/(1.0 - u)))**(1.0/k)
samples = [cdf_inv(u) for u in numpy.random.rand(nsamples)]
```

The following plot compares a histogram of the samples generated by the sampler above and the target
distribution (5). The fit is quite good. The subtle asymmetry of the Weibull distribution is captured.

<img class="post-image" src="/assets/posts/inverse_cdf_sampling/weibull_sampled_distribution.png">

The first and second moments for the Weibull distribution are given by,

{% katex display %}
\begin{aligned}
\mu & = \lambda\Gamma\left(1+\frac{1}{k}\right) \\
\sigma^2 & = \lambda^2\left[\Gamma\left(1+\frac{2}{k}\right)-\left(\Gamma\left(1+\frac{1}{k}\right)\right)^2\right],
\end{aligned}
{% endkatex %}

where {% katex %}\Gamma(x){% endkatex %} is the [Gamma function](https://en.wikipedia.org/wiki/Gamma_function).

The following two plots compare the cumulative values of {% katex %}\mu{% endkatex %} and {% katex %}\sigma{% endkatex %}
computed from sampling of the target distribution {% katex %}(5){% endkatex %} with the values from the equations above.
The first shows the convergence of {% katex %}\mu{% endkatex %} and the second the convergence of {% katex %}\sigma{% endkatex %}.
Within only {% katex %}10^3{% endkatex %} samples both {% katex %}\mu{% endkatex %} and {% katex %}\sigma{% endkatex %} computed from
samples are comparable to the target values and by {% katex %}10^4{% endkatex %} the values are indistinguishable.

<img class="post-image" src="/assets/posts/inverse_cdf_sampling/weibull_sampled_mean_convergence.png">

<img class="post-image" src="/assets/posts/inverse_cdf_sampling/weibull_sampled_std_convergence.png">

### Performance

Any continuous distribution can be sampled by assuming that {% katex %}f_X(x){% endkatex %} can be approximated
by the discrete distribution, {% katex %}\left\{f_X(x_i)\Delta x_i \right\}_N{% endkatex %} for
{% katex %}i=1,2,3,\ldots,N{% endkatex %}, where {% katex %}\Delta x_i=(x_{max}-x_{min})/(N-1){% endkatex %} and
{% katex %}x_i = x_{min}+(i-1)\Delta x_i{% endkatex %}. This method has disadvantages compared to using
Inverse CDF sampling on the continuous distribution. First, a bounded range for the samples must
be assumed when in general the range of the samples can be unbounded while the Inverse CDF method can sample an unbounded range.
Second, the number of operations required for
sampling a discrete distribution scales {% katex %}O(N_{samples}N){% endkatex %} but sampling the
continuous distribution is {% katex %}O(N_{samples}){% endkatex %}.

## Conclusions

Inverse CDF Sampling provides a method for obtaining samples from a known target distribution using
a sampler with a {% katex %}\textbf{Uniform}(0, 1){% endkatex %} distribution that requires the
target distribution be invertible. Algorithms for both the discrete and continuous cases were
analytically proven to produce samples with distributions defined by the CDF.
Example implementations of the algorithms for both distribution cases were developed.
Samples produced by the algorithms for example target distributions were favorably compared. Mean and standard deviations computed from generated samples converged to the target distribution values
in {% katex %}O(10^3){% endkatex %} samples.
The continuous sampling algorithm was shown to be more performant than the discrete version.
The discrete version required {% katex %}O(N_{samples}N){% endkatex %}
operations while the continuous version required {% katex %}O(N_{samples}){% endkatex %} operations.
