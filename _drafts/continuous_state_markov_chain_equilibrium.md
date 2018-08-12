---
title: Continuous State Markov Chain Equilibrium
key: 20180804
image: /assets/posts/continuous_state_markov_chain_equilibrium/ar1_relaxation_to_equilibrium_2.png
author: Troy Stribling
permlink: /continuous_state_markov_chain_equilibrium.html
comments: false
---

A [Markov Chain](https://en.wikipedia.org/wiki/Markov_chain) is a sequence of states
where transitions between states occur ordered in time with
the probability of transition depending only on the previous state. Here the
states will be assumed a continuous unbounded set and time a discrete unbounded set. If the
set of states is given by, {% katex %}x\in\mathbb{R}{% endkatex %}, the probability that the process
will be in state {% katex %}x{% endkatex %} at time {% katex %}t{% endkatex %} is denoted by
{% katex %}P(X_t=x){% endkatex %}, is referred to as the distribution. Markov Chain equilibrium is
defined by {% katex %}\lim_{t\to\infty}P(X_t=x)\ <\ \infty{% endkatex %}, that is, as time advances
{% katex %}P(X_t=x_i){% endkatex %} becomes independent of time. Here a solution
for this limit is discussed and illustrated with examples.

<!--more-->

## Model

The Markov Chain is constructed from the set of states {% katex %}\{x\}{% endkatex %},
ordered in time, where {% katex %}x\in\mathbb{R}{% endkatex %}.
The process starts at time {% katex %}t=0{% endkatex %} with state {% katex %}X_0=x{% endkatex %}.
At the next step, {% katex %}t=1{% endkatex %}, the process will assume a state
{% katex %}X_1=y{% endkatex %}, {% katex %}y\in\{x\}{% endkatex %},
with probability {% katex %}P(X_1=y|X_0=x){% endkatex %}
since it will depend on the state at {% katex %}t=0{% endkatex %} by the definition of a Markov Process.
At the next time step {% katex %}t=2{% endkatex %} the process state will be
{% katex %}X_2=z, \text{where}\ z\in\{x\}{% endkatex %} with probability,
{% katex display %}
P(X_2=z|X_0=x, X_1=y) = P(X_2=z|X_1=y),
{% endkatex %}
since by definition the probability of state transition depends only upon the state at the previous time step.
For an arbitrary time the transition from a state {% katex %}X_{t}=x{% endkatex %} to a state
{% katex %}X_{t+1}=y{% endkatex %} at the next step will occur with probability,
{% katex %}P(X_{t+1}=y|X_t=x){% endkatex %}. The [Transition Kernel](https://en.wikipedia.org/wiki/Markov_kernel),
defined by,
{% katex display %}
p(x,y) = P(X_{t+1}=y|X_t=x),
{% endkatex %}
plays the same role as the transition matrix plays in the theory of
[Discrete State Markov Chains](({{ site.baseurl }}{% link _posts/2018-08-08-discrete_state_markov_chain_equilibrium.md %})). In general {% katex %}p(x,y){% endkatex %} cannot always be represented by a probability density function, but
here it is assumed that it has a density function representation.
This leads to the interpretation that for a known value of {% katex %}x{% endkatex %} {% katex %}p(x, y){% endkatex %} can be interpreted as a conditional probability density for a transition to state {% katex %}y{% endkatex %} given that the process is in state {% katex %}x{% endkatex %}, namely,
{% katex display %}
p(x, y) = f(y|x)
{% endkatex %}
Consequently, {% katex %}p(x,y){% endkatex %} is a family of conditional probability distributions each
representing conditioning on a possible state of the chain at time step {% katex %}t{% endkatex %}.
Since
{% katex %}p(x, y){% endkatex %} is a conditional probability density for each value of {% katex %}x{% endkatex %}
it follows that,
{% katex display %}
\begin{gathered}
\int^{\infty}_{-\infty} p(x, y) dy = 1\ \forall\ x \\
p(x,y)\ \geq 0\ \forall\ x,\ y
\end{gathered}
{% endkatex %}

The transition probability for a single step in the Markov Process is defined by {% katex %}p(x,y){% endkatex %}. The
transition probability across two time steps is computed as follows. Begin by denoting the process state at time
{% katex %}t{% endkatex %} by {% katex %}X_t=x_{t}{% endkatex %}, then the probability of transitioning to a state
{% katex %}X_{t+2} = x_{t+2}{% endkatex %} from {% katex %}X_{t}=x_{t}{% endkatex %} in two time steps is,
{% katex display %}
\begin{aligned}
p(x_{t}, x_{t+2}) &= f(x_{t+2}|x_{t}) \\
&= \int_{-\infty}^{\infty} f(x_{t+2}|x_{t}, x_{t+1})f(x_{t+1}|x_{t}) dx_{t+1} \\
&= \int_{-\infty}^{\infty} f(x_{t+2}|x_{t+1})f(x_{t+1}|x_{t}) dx_{t+1} \\
&= \int_{-\infty}^{\infty} p(x_{t+1}, x_{t+2})p(x_{t}, x_{t+1}) dx_{t+1} \\
&= \int_{-\infty}^{\infty} p(x_{t}, x_{t+1})p(x_{t+1}, x_{t+2}) dx_{t+1}
\end{aligned}
{% endkatex %}

where use was made of the [Law of Total Probability](https://en.wikipedia.org/wiki/Law_of_total_probability), in the
first step and second step used the Markov Chain property
{% katex %}f(x_{t+2}|x_{t}, x_{t+1})=f(x_{t+2}|x_{t+1}){% endkatex %}. Now, the result obtained for the two
step transition probability is the continuous version of matrix multiplication of the single step transitions
probabilities. A notation inspired by matrix multiplication would be helpful. Make the definition,
{% katex display %}
\begin{aligned}
p^2(x, y) &= p(x,y) \star p(x,y) \\
&= \int_{-\infty}^{\infty} p(x, z)p(z, y) dz
\end{aligned}
{% endkatex %}
