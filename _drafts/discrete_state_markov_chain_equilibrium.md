---
title: Discrete State Markov Chain Equilibrium
key: 20180721
image: /assets/posts/discrete_state_markov_chain_equilibrium/distribution_comparison.png
author: Troy Stribling
permlink: /discrete_state_markov_chain_equilibrium.html
comments: false
---

A [Markov Chain](https://en.wikipedia.org/wiki/Markov_chain) is a stochastic process that transitions
between a set of states in time where
the probability of a transition depends only on the previous state. Here the
states will be assumed a discrete finite set and time a discrete unbounded set. If the
set of states is given {% katex %}\{x_1,\ x_2,\ldots,\ x_N\}{% endkatex %} the probability
that process will be in state {% katex %}x_i{% endkatex %} at time {% katex %}t{% endkatex %}
is denoted by {% katex %}P(X_t=x_i){% endkatex %}. Markov Chain equilibrium is defined by
{% katex %}\lim_{t\to\infty}P(X_t=x_i)<\infty{% endkatex %}, that is, as time advances  
{% katex %}P(X_t=x_i){% endkatex %} becomes independent of time. Here a general solution
for this limit is discussed and illustrated with examples.

<!--more-->

## Model
