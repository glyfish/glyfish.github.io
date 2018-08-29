---
title: Metropolis Hastings Sampling
key: 20180721
image: /assets/posts/inverse_cdf_sampling/weibull_sampled_distribution.png
author: Troy Stribling
permlink: /metropolis_hastings_sampling.html
comments: false
---

[Metropolis Hastings Sampling](https://en.wikipedia.org/wiki/Metropolis–Hastings_algorithm) is a method for obtaining samples for known target
probability distribution with no sampler using samples from some other proposal distribution. It is similar to
[Rejection Sampling]({{ site.baseurl }}{% link _posts/2018-07-29-rejection_sampling.md %}) in providing of a criteria for acceptance
of a proposal sample as a target sample but instead of discarding the samples that do not meet the acceptance criteria the sample
from the previous time step is replicated. Another difference is that Metropolis Hastings samples are modeled as a [Markov Chain](https://en.wikipedia.org/wiki/Markov_chain) where the target distribution
is the [Markov Chain Equilibrium Distribution]({{ site.baseurl }}{% link _posts/2018-08-16-continuous_state_markov_chain_equilibrium.md %}). As a consequence
the previously accepted sample is used as part of the acceptance criteria when generating the next sample. This has the advantage of
permitting dynamical adjustment of the proposal distribution parameters as each sample is generated, which is an improvement over Rejection Sampling,
that can lead to a more efficient traversal of the distribution. But a downside of the Markov Chain representation is that
[autocorrelation]({{ site.baseurl }}{% link _posts/2018-08-25-discrete_cross_correlation_theorem.md %}) can develop in the samples which is
not the case in Rejection Sampling.

<!--more-->

## Theory
