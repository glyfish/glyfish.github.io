---
title: Continuous State Markov Chain Equilibrium
key: 20180804
image: /assets/posts/continuous_state_markov_chain_equilibrium/ar1_relaxation_to_equilibrium_1.png
author: Troy Stribling
permlink: /continuous_state_markov_chain_equilibrium.html
comments: false
---

A [Markov Chain](https://en.wikipedia.org/wiki/Markov_chain) is a sequence of states
where transitions between states occur ordered in time with
the probability of transition depending only on the previous state. Here the
states will be assumed a continuous unbounded set and time a discrete unbounded set. If the
set of states is given by, {% katex %}x\in\mathbb{R}{% endkatex %}, the probability that the process
will be in state {% katex %}x{% endkatex %} at time {% katex %}t{% endkatex %}, denoted by
{% katex %}\pi_t (y){% endkatex %}, is referred to as the distribution. Markov Chain equilibrium is
defined by {% katex %}\lim_{t\to\infty}\pi_t (y)\ <\ \infty{% endkatex %}, that is, as time advances
{% katex %}\pi_t (y){% endkatex %} becomes independent of time. Here a solution
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
{% katex %}X_{t+1}=y{% endkatex %} will occur with probability, {% katex %}P(X_{t+1}=y|X_t=x){% endkatex %}
that is independent of {% katex %}t{% endkatex %}.
The [Transition Kernel](https://en.wikipedia.org/wiki/Markov_kernel),
defined by,

{% katex display %}
p(x,y) = P(X_{t+1}=y|X_t=x),
{% endkatex %}

plays the same role as the transition matrix plays in the theory of
[Discrete State Markov Chains]({{ site.baseurl }}{% link _posts/2018-08-08-discrete_state_markov_chain_equilibrium.md %}). In general {% katex %}p(x,y){% endkatex %} cannot always be represented by a probability density function, but
here it is assumed that it has a density function representation.
This leads to the interpretation that for a known value of {% katex %}x{% endkatex %} {% katex %}p(x, y){% endkatex %} can be interpreted as a conditional probability density for a transition to state {% katex %}y{% endkatex %} given that the process is in state {% katex %}x{% endkatex %}, namely,

{% katex display %}
p(x, y) = f(y|x)
{% endkatex %}

Consequently, {% katex %}p(x,y){% endkatex %} is a family of conditional probability distributions each
representing conditioning on a possible state of the chain at time step {% katex %}t{% endkatex %}.
Since {% katex %}p(x, y){% endkatex %} is a conditional probability density for each value of
{% katex %}x{% endkatex %} it follows that,

{% katex display %}
\begin{gathered}
\int^{\infty}_{-\infty} p(x, y) dy = 1\ \forall\ x \\
p(x,y)\ \geq 0\ \forall\ x,\ y
\end{gathered}\ \ \ \ \ (1).
{% endkatex %}

The transition kernel for a single step in the Markov Process is defined by {% katex %}p(x,y){% endkatex %}. The
transition kernel across two time steps is computed as follows. Begin by denoting the process state at time
{% katex %}t{% endkatex %} by {% katex %}X_t=x{% endkatex %}, the state at {% katex %}t+1{% endkatex %} by
{% katex %}X_{t+1}=y{% endkatex %} and the state at {% katex %}t+2{% endkatex %} by
{% katex %}X_{t+2}=z{% endkatex %}. Then the probability of transitioning to a state
{% katex %}X_{t+3}=z{% endkatex %} from {% katex %}X_{t}=x{% endkatex %} in two steps,
{% katex %}p^2(x, z){% endkatex %}, is given by,
{% katex display %}
\begin{aligned}
f(z|x) &= \int_{-\infty}^{\infty} f(z|x, y)f(y|x) dy \\
&= \int_{-\infty}^{\infty} f(z|y)f(y|x) dy \\
&= \int_{-\infty}^{\infty} p(y,z)p(x,y) dy \\
&= \int_{-\infty}^{\infty} p(x,y)p(y,z) dy
\end{aligned}
{% endkatex %}

where use was made of the [Law of Total Probability](https://en.wikipedia.org/wiki/Law_of_total_probability), in the
first step and second step used the Markov Chain property {% katex %}f(z|x, y)=f(z|y){% endkatex %}.
Now, the result obtained for the two step transition kernel is the continuous version of
matrix multiplication of the single step transitions probabilities.
A operator inspired by matrix multiplication would be helpful. Make the definition,
{% katex display %}
P^2(x,z) = \int_{-\infty}^{\infty} p(x, y)p(y, z) dy.
{% endkatex %}

Using this operator, with {% katex %}X_{t+3}=w{% endkatex %}, the three step transition kernel is given by,
{% katex display %}
\begin{aligned}
f(w|x) &= \int_{-\infty}^{\infty} f(w|x,z)f(z|x) dz \\
&= \int_{-\infty}^{\infty} f(w|z) \left[ \int_{-\infty}^{\infty} f(z|y)f(y|x) dy \right] dz \\
&= \int_{-\infty}^{\infty} \int_{-\infty}^{\infty} f(w|z)f(z|y)f(y|x) dy dz \\
&= \int_{-\infty}^{\infty} \int_{-\infty}^{\infty} p(z,w)p(y,z)p(x,y) dy dz \\
&= \int_{-\infty}^{\infty} \int_{-\infty}^{\infty} p(x,y)p(y,z)p(z,w) dy dz \\
&= P^3(x,w),
\end{aligned}
{% endkatex %}

where the second step substitutes the two step transition kernel and the remaining steps perform the decomposition to the single step transition kernel, {% katex %}p(x,y){% endkatex %}. Use of
[Mathematical Induction](https://en.wikipedia.org/wiki/Mathematical_induction) is used to show that the transition
kernel between two states in an arbitrary number of steps, {% katex %}t{% endkatex %}, is given by,

{% katex display %}
P^t(x,y) = \int_{-\infty}^{\infty}\int_{-\infty}^{\infty}\cdots\int_{-\infty}^{\infty} p(x,z_1)p(z_1,z_2)\ldots p(z_{t-1},y) dz_1 dz_2\ldots dz_{t-1}.
{% endkatex %}

The distribution of Markov Chain states {% katex %}\pi (x){% endkatex %} is defined by,

{% katex display %}
\begin{gathered}
\int_{-\infty}^{\infty} \pi (x) dx = 1 \\
\pi (x) \geq 0\ \forall\ x
\end{gathered}
{% endkatex %}

To determine the equilibrium distribution, {% katex %}\pi_E (x){% endkatex %}, the time
variability of {% katex %}\pi_t (x){% endkatex %} must be determined. Begin by considering an arbitrary
distribution at {% katex %}t=0{% endkatex %}, {% katex %}\pi_0 (x){% endkatex %}. The distribution
after the first step will be,

{% katex display %}
\pi_1 (y) = \int_{-\infty}^{\infty} p(x, y)\pi_0 (x) dx.
{% endkatex %}

The distribution after two steps is,

{% katex display %}
\begin{aligned}
\pi_2 (y) &= \int_{-\infty}^{\infty} p(x, y)\pi_1 (x) dx \\
&= \int_{-\infty}^{\infty} p(x, y)\left[ \int_{-\infty}^{\infty} p(z, x)\pi_0 (z) dz \right] dx \\
&= \int_{-\infty}^{\infty} \int_{-\infty}^{\infty} p(z,x) p(x,y) \pi_0 (z) dx dz
\end{aligned}
{% endkatex %}

The pattern becomes more apparent after the third step,

{% katex display %}
\begin{aligned}
\pi_3 (y) &= \int_{-\infty}^{\infty} p(x, y)\pi_2 (x) dx \\
&= \int_{-\infty}^{\infty} p(x, y)\left[ \int_{-\infty}^{\infty} \int_{-\infty}^{\infty} p(w,x) p(z,w)\pi_0 (z) dz dw \right] dx \\
&= \int_{-\infty}^{\infty}\int_{-\infty}^{\infty} \int_{-\infty}^{\infty} p(x,y) p(w,x) p(z,w)\pi_0 (z) dz dw dx \\
&= \int_{-\infty}^{\infty}\int_{-\infty}^{\infty} \int_{-\infty}^{\infty} p(z,w) p(w,x) p(x,y) \pi_0 (z) dw dx dz.
\end{aligned}
{% endkatex %}

This looks similar to the previous result obtained for the time dependence of the transition kernel. Making use
of the operator {% katex %}P{% endkatex %} gives,

{% katex display %}
\begin{aligned}
\pi_1 (y) &= P\pi_0(y) = \int_{-\infty}^{\infty} p(x, y)\pi_0 (x) dx \\
\pi_2 (y) &= P^2\pi_0(y) = \int_{-\infty}^{\infty} \int_{-\infty}^{\infty} p(z,x) p(x,y) \pi_0 (z) dx dz \\
\pi_3 (y) &= P^3\pi_0(y) = \int_{-\infty}^{\infty}\int_{-\infty}^{\infty} \int_{-\infty}^{\infty} p(z,w) p(w,x) p(x,y) \pi_0 (z) dw dx dz.
\end{aligned}
{% endkatex %}

Mathematical Induction can be used to prove the distribution at an arbitrary time {% katex %}t{% endkatex %}
is given by,

{% katex display %}
\pi_t (y) = P^t\pi_0(y)\ \ \ \ \ (2)
{% endkatex %}

## Equilibrium Distribution

The equilibrium distribution is defined as the invariant solution to equation {% katex %}(2){% endkatex %}
for arbitrary {% katex %}t{% endkatex %}, namely,

{% katex display %}
\pi_E (y) = P^t\pi_E(y)\ \ \ \ \ (3).
{% endkatex %}

Consider,
{% katex display %}
\pi_E (y) = P\pi_E(y)\ \ \ \ \ (4),
{% endkatex %}

substitution into equation {% katex %}(3){% endkatex %} gives,

{% katex display %}
\begin{aligned}
\pi_E (y) &= P^t\pi_E(y) \\
&= P^{t-1}(P\pi_E)(y) \\
&= P^{t-1}\pi_E(y) \\
&\vdots \\
&= P\pi_E(y) \\
&= \pi_E(y).
\end{aligned}
{% endkatex %}

Thus equation {% katex %}(4){% endkatex %} defines the time invariant solution to equation
{% katex %}(3){% endkatex %}.

To go further a particular form for the transition kernel must be specified. Unlike the
[discrete state model]({{ site.baseurl }}{% link _posts/2018-08-08-discrete_state_markov_chain_equilibrium.md %})
a general solution cannot be obtained since convergence of the limit {% katex %}t\to\infty{% endkatex %} will
depend on the assumed transition kernel. The following section will describe a solution to equation
{% katex %}(4){% endkatex %} arising from a simple stochastic processes.

## Example

To evaluate the equilibrium distribution a form for the transition kernel must be assumed. Here the
[AR(1)](https://en.wikipedia.org/wiki/Autoregressive_model) stochastic process is considered.
AR(1) is a simple first order autoregressive model providing an example of a continuous state Markov Chain.
In following sections its equilibrium distribution is determined and the results of simulations are discussed.

AR(1) is defined by the difference equation,

{% katex display %}
X_{t} = \alpha X_{t-1} + \varepsilon_{t}\ \ \ \ \ (5),
{% endkatex %}

where {% katex %}t=0,\ 1,\ 2,\ldots{% endkatex %} and the {% katex %}\varepsilon{t}{% endkatex %} are identically
distributed independent {% katex %}\textbf{Normal}{% endkatex %}
random variables with zero mean and variance, {% katex %}\sigma^2{% endkatex %}. The {% katex %}t{% endkatex %}
subscript on {% katex %}\varepsilon{% endkatex %} indicates that it is generated at time step
{% katex %}t{% endkatex %}.
The transition kernel for AR(1) can be derived from equation {% katex %}(5){% endkatex %} by using,
{% katex %}\varepsilon_{t} \sim \textbf{Normal}(0,\ \sigma^2){% endkatex %} and letting
{% katex %}x=x_{t-1}{% endkatex %} and {% katex %}y=x_{t}{% endkatex %} so that
{% katex %}\varepsilon_{t} = y - \alpha x{% endkatex %}. The result is,

{% katex display %}
p(x,y) = \frac{1}{\sqrt{2\pi\sigma^2}} e^{(y-\alpha x)^2/2\sigma^2}\ \ \ \ \ \ (6)
{% endkatex %}

Now, consider a few steps,

{% katex display %}
\begin{aligned}
X_1 &= \alpha X_0 + \varepsilon_1 \\
X_2 &= \alpha X_1 + \varepsilon_2 \\
X_3 &= \alpha X_2 + \varepsilon_3,
\end{aligned}
{% endkatex %}

substituting the first equation into the second equation and that result into the third equation gives,

{% katex display %}
X_3 = \alpha^3 X_0 + \alpha^2\varepsilon_1 + \alpha\varepsilon_2 + \varepsilon_3
{% endkatex %}

If this process is continued for {% katex %}t{% endkatex %} steps the following result is
obtained,

{% katex display %}
X_t = \alpha^t X_0 + \sum_{i=1}^{t} \alpha^{t-i} \varepsilon_{i}\ \ \ \ \ (7).
{% endkatex %}

## Equilibrium Solution

In this section equation {% katex %}(7){% endkatex %} is used to evaluate the the mean and variance of
the AR(1) process in the equilibrium limit {% katex %}t\to\infty{% endkatex %}. The mean and variance
obtained are then used to construct {% katex %}\pi_E(x){% endkatex %} that is shown to be a solution
to equation {% katex %}(4){% endkatex %}.

The equilibrium mean is given by,

{% katex display %}
  \mu_{E} = \lim_{t\to\infty} E(X_t).
{% endkatex %}

From equation {% katex %}(7){% endkatex %} it is seen that,

{% katex display %}
\begin{aligned}
E(X_t) &= E\left[ \alpha^t X_0 + \sum_{i=1}^{t} \alpha^{t-i} \varepsilon_{i} \right] \\
&= \alpha^t x_0 + \sum_{i=1}^t E(\varepsilon_i) \\
&= \alpha^t x_0,
\end{aligned}
{% endkatex %}

where the last step follows from {% katex %}E(\varepsilon_i)=0{% endkatex %}. Now,

{% katex display %}
\begin{aligned}
\mu_E &= \lim_{t\to\infty} E(X_t) \\
&= \lim_{t\to\infty} \alpha^t X_0,
\end{aligned}
{% endkatex %}

this limit exists for {% katex %}\mid\alpha\mid\ \leq\ 1{% endkatex %},

{% katex display %}
\mu_E =
\begin{cases}
X_0 & \mid\alpha\mid=1 \\
0 & \mid\alpha\mid\ \leq\ 1
\end{cases}\ \ \ \ \ (8).
{% endkatex %}

The equilibrium variance is given by,

{% katex display %}
\sigma^2_E = \lim_{t\to\infty} E(X^2_t) - \left[E(X_t)\right]^2.
{% endkatex %}

To evaluate this expression and equation for {% katex %}X^2_t{% endkatex %} is needed. From equation
{% katex %}(7){% endkatex %} it follows that,

{% katex display %}
\begin{aligned}
X_t^2 &= \left[ \alpha^t X_0 + \sum_{i=1}^{t} \alpha^{t-i} \varepsilon_i \right]^2 \\
&= \alpha^{2t}X_0^2 + 2\alpha^t X_0\sum_{i=1}^t \alpha^{t-1} \varepsilon_i + \sum_{i=1}^t \sum_{j=1}^t \alpha^{t-i}\alpha^{t-j}\varepsilon_i \varepsilon_j.
\end{aligned}
{% endkatex %}

It follows that,

{% katex display %}
\begin{aligned}
E(X_t^2) &= E\left[\alpha^{2t}X_0^2 + 2\alpha^t X_0\sum_{i=1}^t \alpha^{t-1} \varepsilon_i + \sum_{i=1}^t \sum_{j=1}^t \alpha^{t-i}\alpha^{t-j}\varepsilon_i \varepsilon_j \right] \\
&= \alpha^{2t} X_0^2 +  2\alpha^t X_0\sum_{i=1}^t \alpha^{t-1} E(\varepsilon_i) + \sum_{i=1}^t \sum_{j=1}^t \alpha^{t-i}\alpha^{t-j}E(\varepsilon_i \varepsilon_j) \\
&= \alpha^{2t} X_0^2 + \sum_{i=1}^t \sum_{j=1}^t \alpha^{t-i}\alpha^{t-j}E(\varepsilon_i \varepsilon_j),
\end{aligned}
{% endkatex %}

where the last step follows from {% katex %}E(\varepsilon_i)=0{% endkatex %}. Since the
{% katex %}\varepsilon_t{% endkatex %} are independent,

{% katex display %}
E(\varepsilon_i \varepsilon_j) = \sigma^2 \delta_{ij},
{% endkatex %}

where {% katex %}\delta_{ij}{% endkatex %} is the [Kronecker Delta](https://en.wikipedia.org/wiki/Kronecker_delta),
{% katex display %}
\delta_{ij} =
\begin{cases}
0 & i \neq j \\
1 & i=j
\end{cases}.
{% endkatex %}

Using these results gives,

{% katex display %}
\begin{aligned}
E(X_t^2) &= \alpha^{2t} X_0^2 + \sigma^2\sum_{i=1}^t \sum_{j=1}^t \alpha^{2t-i-j}\delta_{ij} \\
&= \alpha^{2t} X_0^2 + \sigma^2\sum_{i=1}^t \alpha^{2(t-i)} \\
&= \alpha^{2t} X_0^2 + \sigma^2\sum_{i=1}^t \left(\alpha^2\right)^{t-i} \\
&= \alpha^{2t} X_0^2 + \frac{\sigma^2\left[1 - (\alpha^2)^{t-1}\right]}{1 - \alpha^2},
\end{aligned}
{% endkatex %}

The last step follows from summation of a geometric series,

{% katex display %}
\begin{aligned}
\sum_{i=1}^{t} (\alpha^2)^{t-1} &= \sum_{k=0}^{t-1}(\alpha^2)^k \\
&= \frac{1-(\alpha^2)^{t-1}}{1-\alpha^2}.
\end{aligned}
{% endkatex %}

In the limit {% katex %}t\to\infty {% endkatex %} {% katex %}E(X_t^2){% endkatex %} only converges for {% katex %}\mid\alpha\mid\ <\ 1{% endkatex %}. If {% katex %}\alpha=1{% endkatex %} the denominator of the second term in
of {% katex %}E(X_t^2){% endkatex %} is {% katex %}0{% endkatex %} {% katex %}E(X_t^2){% endkatex %} is
undefined. {% katex %}\sigma^2_E{% endkatex %}
is evaluated assuming {% katex %}\mid\alpha\mid\ <\ 1{% endkatex %} if this is the case
{% katex %}(8){% endkatex %} gives {% katex %}\mu_E=0{% endkatex %}, so,

{% katex display %}
\begin{aligned}
\sigma^2_E &= \lim_{t\to\infty} E(X_t^2) - \left(\mu_E\right)^2 \\
&= \lim_{t\to\infty} \alpha^{2t}X_0^2 + \frac{\sigma^2\left[1 - (\alpha^2)^{t-1}\right]}{1 - \alpha^2} \\
&= \frac{\sigma^2}{1 - \alpha^2}.
\end{aligned}\ \ \ \ \ (9)
{% endkatex %}

The equilibrium distribution, {% katex %}\pi_E{% endkatex %}, is found by substituting the results
from equation {% katex %}(8){% endkatex %} and {% katex %}(9){% endkatex %} into a
{% katex %}\textbf{Normal}(\mu_E,\ \sigma^2_E){% endkatex %} distribution to obtain,

{% katex display %}
\pi_E(y) = \frac{1}{\sqrt{2\pi\sigma_E^2}}e^{-y^2/2\sigma_E^2}\ \ \ \ \ (10)
{% endkatex %}

It can be shown that equation {% katex %}(10){% endkatex %} is the equilibrium distribution by verifying that it is a solution to
equation {% katex %}(4){% endkatex %}. Inserting equations {% katex %}(6){% endkatex %} and
{% katex %}(10){% endkatex %} into equation {% katex %}(4){% endkatex %} yields,

{% katex display %}
\begin{aligned}
P\pi_E(y) &= \int_{-\infty}^{\infty} p(x, y) \pi_E(x) dx \\
&= \int_{-\infty}^{\infty} \left[\frac{1}{\sqrt{2\pi\sigma^2}} e^{(y-\alpha x)^2/2\sigma^2}\right]\left[\frac{1}{\sqrt{2\pi\sigma_E^2}}e^{-y^2/2\sigma_E^2}\right] dx \\
&= \frac{1}{\sqrt{2\pi\sigma^2}}\frac{1}{\sqrt{2\pi\sigma_E^2}} \int_{-\infty}^{\infty} e^{-\frac{1}{2}\left[(y-\alpha x)^2/\sigma^2+x^2/\sigma_E^2 \right]} dx \\
&= \frac{1}{\sqrt{2\pi\sigma^2}}\frac{1}{\sqrt{2\pi\sigma_E^2}} \int_{-\infty}^{\infty} e^{-\frac{1}{2}\left[y^2/\sigma_E^2+(x-\alpha y)^2/\sigma^2 \right]} dx \\
&= \frac{1}{\sqrt{2\pi\sigma_E^2}} e^{-y^2/2\sigma_E^2}\frac{1}{\sqrt{2\pi\sigma^2}}\int_{-\infty}^{\infty} e^{-(x-\alpha y)^2/2\sigma^2} dx \\
&= \frac{1}{\sqrt{2\pi\sigma_E^2}} e^{-y^2/2\sigma_E^2} \\
&= \pi_E(y)
\end{aligned}
{% endkatex %}

## Simulation

An AR(1) simulator can be implemented using either the difference equation definition, equation
{% katex %}(5){% endkatex %} or the stochastic kernel, equation {% katex %}(6){% endkatex %}.

```python
def ar_1_difference_series(α, σ, x0, nsamples=100):
    samples = numpy.zeros(nsamples)
    ε = numpy.random.normal(0.0, σ, nsamples)
    samples[0] = x0
    for i in range(1, nsamples):
        samples[i] = α * samples[i-1] + ε[i]
    return samples
```

```python
def ar1_kernel_series(α, σ, x0, nsamples=100):
    samples = numpy.zeros(nsamples)
    samples[0] = x0
    for i in range(1, nsamples):
        samples[i] = numpy.random.normal(α * samples[i-1], σ)
    return samples
```

<div style="text-align:center;">
  <img src="/assets/posts/continuous_state_markov_chain_equilibrium/ar1_alpha_sample_comparison.png">
</div>

<div style="text-align:center;">
  <img src="/assets/posts/continuous_state_markov_chain_equilibrium/ar1_alpha_larger_than_1.png">
</div>

## Relaxation to Equilibrium

<div style="text-align:center;">
  <img src="/assets/posts/continuous_state_markov_chain_equilibrium/mean_convergence.png">
</div>

<div style="text-align:center;">
  <img src="/assets/posts/continuous_state_markov_chain_equilibrium/sigma_convergence.png">
</div>

<div style="text-align:center;">
  <img src="/assets/posts/continuous_state_markov_chain_equilibrium/equilibrium_pdf_comparison_samples.png">
</div>

```python
def ar_1_kernel(α, σ, x, y):
    p = numpy.zeros(len(y))
    for i in range(0, len(y)):
        ε  = ((y[i] -  α * x)**2) / (2.0 * σ**2)
        p[i] = numpy.exp(-ε) / numpy.sqrt(2 * numpy.pi * σ**2)
    return p

def ar_1_equilibrium_distributions(α, σ, x0, y, nsample=100):
    py = [ar_1_kernel(α, σ, x, y) for x in ar_1_difference_series(α, σ, x0, nsample)]
    pavg = [py[0]]
    for i in range(1, len(py)):
        pavg_next = (py[i] + i * pavg[i-1]) / (i + 1)
        pavg.append(pavg_next)
    return pavg
```

<div style="text-align:center;">
  <img src="/assets/posts/continuous_state_markov_chain_equilibrium/equilibrium_pdf_comparison.png">
</div>

<div style="text-align:center;">
  <img src="/assets/posts/continuous_state_markov_chain_equilibrium/ar1_relaxation_to_equilibrium_1.png">
</div>
