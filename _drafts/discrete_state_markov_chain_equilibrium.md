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

A Markov Chain is a [stochastic process](https://en.wikipedia.org/wiki/Stochastic_process) that
constructed from the set of states {% katex %}\{x_1,\ x_2,\ldots,\ x_N\}{% endkatex %} in time.
The process starts at time {% katex %}t=0{% endkatex %} with state {% katex %}X_0=x_i{% endkatex %}.
At the next step, {% katex %}t=1{% endkatex %}, the process will assume a state
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
P_{ij} = P(X_{t+1}=x_j|X_t=x_i).
{% endkatex %}

{% katex %}P{% endkatex %} will be a square {% katex %}N\times N{% endkatex %} matrix
where the size {% katex %}N{% endkatex %} is determined by the number of possible states. Each
row represents the Markov Chain state at time {% katex %}t{% endkatex %} and the columns the
values at {% katex %}t+1{% endkatex %}. It follows that,
{% katex display %}
\sum_{j=1}^{N}P_{ij} = 1\ \ \ \ \ (1),
{% endkatex %}

where {% katex %}P_{ij}\ \geq\ 0{% endkatex %}.

The end goal of understanding the equilibrium distribution requires the transition probability across
two time steps. Using the [Law of Total Probability](https://en.wikipedia.org/wiki/Law_of_total_probability)
{% katex display %}
\begin{aligned}
P(X_{t+2}=x_j|X_t=x_i) &= \sum_{k=1}^{N} P(X_{t+2}=x_j | X_{t}=x_i, X_{t+1}=x_k)P(X_{t+1}=x_k | X_{t}=x_i) \\
&= \sum_{k=1}^{N} P(X_{t+2}=x_j | X_{t+1}=x_k)P(X_{t+1}=x_k | X_{t}=x_i) \\
&= \sum_{k=1}^{N} P_{kj}P_{ik} \\
&= \sum_{k=1}^{N} P_{ik}P_{kj} \\
&= {(P^2)}_{ij},
\end{aligned}
{% endkatex %}

where the last step follows from the definition of matrix multiplication. It is straight forward but
tedious to use [Mathematical Induction](https://en.wikipedia.org/wiki/Mathematical_induction) to extend the previous result
to the case of an arbitrary time difference, {% katex %}\tau{% endkatex %},
{% katex display %}
P(X_{t+\tau}=x_j|X_t=x_i) = {(P^{\tau})}_{ij}.
{% endkatex %}

It should be noted that since {% katex %}{(P^{\tau})}_{ij}{% endkatex %} is a transition probability it must
satisfy,

{% katex display %}
\begin{aligned}
\sum_{j=1}^{N} {(P^{\tau})}_{ij}\ &=\ 1 \\
{(P^{\tau})}_{ij}\ &\geq\ 0
\end{aligned}
{% endkatex %}

To determine the equilibrium solution of the probability distribution of the Markov Chain states,
{% katex %}\{x_1,\ x_2,\ldots,\ x_N\}{% endkatex %} its time variability must be determined.
Begin by considering the distribution of states at {% katex %}t=0{% endkatex %}.
This distribution is arbitrary and can be written as a column vector,
{% katex display %}
\pi =
\begin{pmatrix}
\pi_1 \\
\pi_2 \\
\vdots \\
\pi_N
\end{pmatrix} =
\begin{pmatrix}
P(X_0=x_1) \\
P(X_0=x_2) \\
\vdots \\
P(X_0=x_N)
\end{pmatrix},
{% endkatex %}
where {% katex %}\sum_{i=1}^{N} \pi_i\ =\ 1{% endkatex %} and {% katex %}\pi_i\ \geq \ 0{% endkatex %}.
The distribution of states after the first step is given by,
{% katex display %}
\begin{aligned}
P(X_1=x_j) &= \sum_{i=1}^{N} P(X_1=x_j|X_0=x_i)P(X_0=x_i) \\
&= \sum_{i=1}^{N} P_{ij}\pi_{i} \\
&= \sum_{i=1}^{N} \pi_{i}P_{ij} \\
&= {(\pi^{T}P)}_{j},
\end{aligned}
{% endkatex %}

where {% katex %}\pi^T{% endkatex %} is the transpose of {% katex %}\pi{% endkatex %}. Similarly, the distribution of
states after the second step is,
