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
