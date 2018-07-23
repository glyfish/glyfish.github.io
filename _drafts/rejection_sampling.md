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
distribution to have an invertible CDF. It has a still simple but more complicated implementation.

<!--more-->
