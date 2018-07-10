---
title: Inverse CDF Sampling
key: 20180708
---

Inverse [CDF](https://en.wikipedia.org/wiki/Cumulative_distribution_function) sampling is a method for obtaining samples from both discrete and continuous probability distributions that requires the CDF to be invertible. The method assumes values of the CDF are Uniform random variables on {% katex %}[0, 1]{% endkatex %}.
CDF values are generated and used as input into the inverted CDF to obtain samples with the
distribution defined by the CDF.

<!--more-->

## Sampling Discrete Distributions

A discrete probability distribution consisting of a finite set of {% katex %}N{% endkatex %}
probability values is defined by, {% katex %}\{p_i\}_N = \{p_1, p_2,\ldots,p_N\}{% endkatex %} with
{% katex %}p_i \geq 0, \  \forall i{% endkatex %} and {% katex %}\sum_{i=1}^N{p_i} = 1{% endkatex %}.
The CDF specifies the probability that {% katex %}i \leq n{% endkatex %} and is given by,

{% katex display %}
P(i \leq n)=P(n)=\sum_{i=1}^n{p_i},
{% endkatex %}

For a given generated CDF value, {% katex %}U{% endkatex %}, Equation {% katex %}P(n){% endkatex %} can always be inverted by evaluating it for each {% katex %}n{% endkatex %} and
searching for the value of {% katex %}n{% endkatex %} that satisfies, {% katex %}P(n) \geq U{% endkatex %}.
It can be seen that the generated samples will have
distribution {% katex %}\{p_i\}_N{% endkatex %} since the intervals
{% katex %}P(n)-P(n-1) = p_n{% endkatex %} are Uniformly sampled.

Consider the example distribution,

{% katex display %}
\left\{\frac{1}{12}, \frac{1}{12}, \frac{1}{6}, \frac{1}{6}, \frac{1}{12}, \frac{5}{12} \right\} \ \ \ \ \ (1)
{% endkatex %}

It is shown in the following plot with its CDF. Note that the CDF is a monotonically increasing function.

<div style="text-align:center;">
<img src="/assets/posts/inverse_cdf_sampling/discrete_cdf.png">
</div>

## Sampling Continuous Distributions

### Proof that Inverse CD Sampling Works

### Example

Consider the [Weibull Distribution](https://en.wikipedia.org/wiki/Weibull_distribution). The PDF is
given by,

{% katex display %}
f_X(x; k, \lambda) =
\begin{cases}
\frac{k}{\lambda}\left(\frac{x}{\lambda} \right)^{k-1} e^{\left(\frac{-x}{\lambda}\right)^k} & x \geq 0 \\
0 & x < 0,
\end{cases}
{% endkatex %}

where {% katex %}k{% endkatex %} is the shape parameter and {%katex %}\lambda{% endkatex %} the scale parameter.
The CDF is given by,

{% katex display %}
F_X(x; k, \lambda) =
\begin{cases}
1-e^{\left(\frac{-x}{\lambda}\right)^k
} & x \geq 0 \\
0 & x < 0.
\end{cases}
{% endkatex %}

The CDF can be inverted to yield,

{% katex display %}
F_X^{-1}(u; k, \lambda) =
\begin{cases}
\lambda\ln\left(\frac{1}{1-u}\right)^{\frac{1}{k}} & 0 \leq u \leq 1 \\
0 & u < 0 \text{ or } u > 1.
\end{cases}
{% endkatex %}

### Performance
