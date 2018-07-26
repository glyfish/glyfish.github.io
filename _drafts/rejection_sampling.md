---
title: Rejection Sampling
key: 20180721
image: /assets/posts/rejection_sampling/weibull_normal_3_efficiency.png
author: Troy Stribling
comments: false
---

Rejection Sampling is a method for obtaining samples for a known target probability distribution
with no sampler using samples from some other proposal distribution with a sampler.
It is a more general method than
[Inverse CDF Sampling]({{ site.baseurl }}{% link _posts/2018-07-21-inverse_cdf_sampling.md %}) which requires
distribution to have an invertible CDF. Inverse CDF Sampling transforms a
{% katex %}\textbf{Uniform}(0, 1){% endkatex %} random variable into a random variable with
a desired target distribution using the inverted CDF of the target distribution. While, Rejection Sampling
is a method for transformation of random variables from arbitrary proposal distributions into a desired target
distribution.

<!--more-->

The implementation of Rejection Sampling requires the consideration of a target distribution, {% katex %}f_X(X){% endkatex %}, a proposal distribution, {% katex %}f_Y(Y){% endkatex %} and a {% katex %}\textbf{Uniform}(0, 1){% endkatex %} acceptance probability {% katex %}U{% endkatex %} with distribution {% katex %}f_U(U)=1{% endkatex %}.
A criterion is defined for acceptance
of a sample, {% katex %}Y{% endkatex %}, generated from the proposal distribution, {% katex %}f_Y(Y){% endkatex %}, independent of the acceptance probability, {% katex %}U{% endkatex %}, namely,

{% katex display %}
U\ \leq\ h(Y) = \frac{f_X(Y)}{cf_Y(Y)}\ \ \ \ \ (1),
{% endkatex %}

where, {% katex %}c{% endkatex %} is chosen to satisfy
{% katex %}0\ \leq\ h(Y)\ \leq\ 1, \ \ \forall\ Y{% endkatex %}. If equation
{% katex %}(1){% endkatex %} is satisfied the proposed sample {% katex %}Y{% endkatex %} is accepted as a sample
of {% katex %}f_X(X){% endkatex %} where {% katex %}X=Y{% endkatex %}. If equation {% katex %}(1){% endkatex %}
is not satisfied {% katex %}Y{% endkatex %} is discarded.

This procedure is summarized in the following steps that are repeated for the generation of each sample,

1. Generate a sample {% katex %}Y \sim f_Y(Y){% endkatex %}.
2. Generate a sample {% katex %}U\sim\textbf{Uniform}(0, 1){% endkatex %} independent of {% katex %}Y{% endkatex %}.
3. If equation {% katex %}(1){% endkatex %} is satisfied then {% katex %}X=Y{% endkatex %} is accepted as a sample
of {% katex %}f_X(X){% endkatex %}. If equation {% katex %}(1){% endkatex %} is not satisfied then {% katex %}Y{% endkatex %}
is discarded.

## Proof

To prove that Rejection Sampling works it must be shown that,
{% katex display %}
P\left[Y\ \leq\ y\ |\ U\ \leq\ \frac{f_X(Y)}{cf_Y(Y)}\right]=F_X(y)\ \ \ \ \ (2).
{% endkatex %}

Where {% katex %}F_X(y){% endkatex %} is the CDF for {% katex %}f_X(y){% endkatex %},
{% katex display %}
F_X(y) = \int_{-\infty}^{y} f_X(w)dw.
{% endkatex %}

To prove equation {% katex %}(2){% endkatex %} a couple of intermediate steps are required. First,
The joint distribution of {% katex %}U{% endkatex %} and {% katex %}Y{% endkatex %} containing the
acceptance constraint will be evaluated,

{% katex display %}
P\left[U\ \leq\ \frac{f_X(Y)}{cf_Y(Y)},\ Y\ \leq\ y\right] = \frac{F_X(y)}{c}\ \ \ \ \ (3).
{% endkatex %}

Since the Rejection Sampling algorithm as described in the previous section assumes
that {% katex %}Y{% endkatex %} and {% katex %}U{% endkatex %} are independent random variables,
{% katex%}
f_{YU}(Y, U)\ =\ f_Y(Y)f_U(U)\ = f_Y(Y).
{% endkatex %} It follows that,

{% katex display %}
\begin{aligned}
P\left[U\ \leq\ \frac{f_X(Y)}{cf_Y(Y)},\ Y\ \leq y\right] &= \int_{-\infty}^{y}\int_{0}^{f_X(w)/cf_Y(w)} f_{YU}(w, u) du dw\\
&= \int_{-\infty}^{y}\int_{0}^{f_X(w)/cf_Y(w)} f_Y(w) du dw \\
&= \int_{-\infty}^{y} f_Y(w) \int_{0}^{f_X(w)/cf_Y(w)} du dw \\
&= \int_{-\infty}^{y} f_Y(w) \frac{f_X(w)}{cf_Y(w)} dw \\
&= \frac{1}{c}\int_{-\infty}^y f_X(w) dw \\
&= \frac{F_X(y)}{c}, \\
\end{aligned}
{% endkatex %}

Next, it will be shown that the probability of accepting a sample is given by,
{% katex display %}
P\left[U\ \leq\ \frac{f_X(Y)}{cf_Y(Y)}\right] = \frac{1}{c}\ \ \ \ \ (4).
{% endkatex %}

This result follows from equation {% katex %}(3){% endkatex %} in the limit {% katex %}y\to\infty{% endkatex %}
and noting that, {% katex %}\int_{-\infty}^{\infty} f_X(y) dy = 1{% endkatex %}.

{% katex display %}
\begin{aligned}
P\left[U\ \leq\ \frac{f_X(Y)}{cf_Y(Y)}\right] &= \int_{-\infty}^{\infty}\int_{0}^{f_X(w)/cf_Y(w)} f_{YU}(w, u) du dw\\
&= \frac{1}{c}\int_{-\infty}^{\infty} f_X(w) dw \\
&= \frac{1}{c}
\end{aligned}
{% endkatex %}

Finally, equation {% katex %}(2){% endkatex %} can be proven, using the definition of [Conditional Probability](https://en.wikipedia.org/wiki/Conditional_probability),
equation {% katex %}(3){% endkatex %} and equation {% katex %}(4){% endkatex %},

{% katex display %}
\begin{aligned}
P\left[Y\ \leq\ y\ |\ U\ \leq\ \frac{f_X(Y)}{cf_Y(Y)}\right] &= \frac{P\left[U\ \leq\ \frac{f_X(Y)}{cf_Y(Y)},\ Y \leq y\right]}{P\left[U\ \leq\ \frac{f_X(Y)}{cf_Y(Y)}\right]}\\
&=\frac{F_X(y)}{c}\frac{1}{1/c}\\
&=F_X(y)
\end{aligned}
{% endkatex %}

## Implementation

## Examples
