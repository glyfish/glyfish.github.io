---
title: Discrete State Markov Chain Equilibrium
key: 20180721
image: /assets/posts/discrete_state_markov_chain_equilibrium/distribution_comparison.png
author: Troy Stribling
permlink: /discrete_state_markov_chain_equilibrium.html
comments: false
---
A [Markov Chain](https://en.wikipedia.org/wiki/Markov_chain) is a sequence of states
where transitions between states occur in time with
the probability of a transition depending only on the previous state. Here the
states will be assumed a discrete finite set and time a discrete unbounded set. If the
set of states is given {% katex %}\{x_1,\ x_2,\ldots,\ x_N\}{% endkatex %} the probability
that process will be in state {% katex %}x_i{% endkatex %} at time {% katex %}t{% endkatex %}
is denoted by {% katex %}P(X_t=x_i){% endkatex %}. Markov Chain equilibrium is defined by
{% katex %}\lim_{t\to\infty}P(X_t=x_i)<\infty{% endkatex %}, that is, as time advances  
{% katex %}P(X_t=x_i){% endkatex %} becomes independent of time. Here a solution
for this limit is discussed and illustrated with examples.

<!--more-->

## Model

A Markov Chain is a [stochastic process](https://en.wikipedia.org/wiki/Stochastic_process) that constructed from the set of states  {% katex %}\{x_1,\ x_2,\ldots,\ x_N\}{% endkatex %} in time. The process starts at
time {% katex %}t=0{% endkatex %} with state {% katex %}X_0=x_i{% endkatex %}. At the
next step, {% katex %}t=1{% endkatex %}, the process will assume a state
{% katex %}X_1=x_j{% endkatex %} with probability {% katex %}P(X_1=x_j|X_0=x_i){% endkatex %}.
At the next time step {% katex %}t=2{% endkatex %} the process state will be
{% katex %}X_2=x_k{% endkatex %} with probability,
{% katex display %}
P(X_2=x_k|X_0=x_i, X_1=x_j)=P(X_2=x_k|X_1=x_j),
{% endkatex %}
since the probability of state transition depends only upon the state at the previous time step.
For an arbitrary time the transition to a state at the next time will occur with probability,
{% katex %}P(X_{t+1}=x_j|X_t=x_i){% endkatex %}. These transition probabilities have the form of
a matrix,

{% katex display %}
P_{i,j} = P(X_{t+1}=x_i|X_t=x_j).
{% endkatex %}

{% katex %}P_{i,j}{% endkatex %} will be a square {% katex %}N\times N{% endkatex %} matrix
where the size {% katex %}N{% endkatex %} is determined by the number of possible states. Each
row represents the Markov Chain state at time {% katex %}t{% endkatex %} and the columns the
values at {% katex %}t+1{% endkatex %}. It follows that,
{% katex display %}
\sum_{j=1}^{N}P_{i,j} = 1\ \ \ \ \ (1)
{% endkatex %}

To go further the transition probability across two time steps is needed, namely,
{% katex %}P(X_{t+2}=x_i|X_t=x_j){% endkatex %}. Using the
(Law of Total Probability)[https://en.wikipedia.org/wiki/Law_of_total_probability]
{% katex display %}
\begin{aligned}
P(X_{t+2}=x_j|X_t=x_i) &= \sum_{k=1}^{N} P(X_{t+2}=x_j | X_{t}=x_i, X_{t+1}=x_k)P(X_{t+1}=x_k | X_{t}=x_i) \\
&= \sum_{k=1}^{N} P(X_{t+2}=x_j | X_{t+1}=x_k)P(X_{t+1}=x_k | X_{t}=x_i) \\
&= \sum_{k=1}^{N} P_{k,j}P_{i,k} \\
&= \sum_{k=1}^{N} P_{i,k}P_{k,j} \\
&= {(P^2)}_{i,j}.
\end{aligned}
{% endkatex %}
