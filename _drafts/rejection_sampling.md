---
title: Rejection Sampling
key: 20180721
image: /assets/posts/rejection_sampling/weibull_normal_3_efficiency.png
author: Troy Stribling
comments: false
---

Rejection Sampling is a method for obtaining samples for a known probability distribution
with no available sampler using samples from some other distribution with an available sampler.
It is a more general method than
[Inverse CDF Sampling]({{ site.baseurl }}{% link _posts/2018-07-21-inverse_cdf_sampling.md %}) which requires
distribution to have an invertible CDF. Inverse CDF Sampling transforms a
{% katex %}\textbf{Uniform}(0, 1){% endkatex %} random variable into a random variable with
a desired target distribution using the inverted CDF of the target distribution. While, Rejection Sampling
is a method for transformation of random variables from arbitrary proposal distribution into a desired target
distribution.

<!--more-->

The implementation of Rejection Sampling is more complicated than the implementation of Inverse CDF Sampling.
In addition to the target distribution, {% katex %}f(x){% endkatex %}, a proposal distribution,
{% katex %}g(x){% endkatex %}, must also be considered. Also, a criterion is defined for acceptance of
a proposal random variable.
