---
title: Discrete Cross Correlation Theorem
key: 20180818
image: /assets/posts/discrete_cross_correlation_theorem/ar1_alpha_equilibrium_autocorrelation_comparison.png
author: Troy Stribling
permlink: /discrete_cross_correlation.html
comments: false
---

The [Cross Correlation Theorem](https://en.wikipedia.org/wiki/Cross-correlation) is similar to the more
widely known [Convolution Theorem](https://en.wikipedia.org/wiki/Convolution_theorem). The cross correlation
of two discrete time series is {% katex %}\{x_0,\ x_1,\ x_2,\ldots\,\ x_{N-1}\}{% endkatex %} and
{% katex %}\{y_0,\ y_1,\ y_2,\ldots\,\ y_{N-1}\}{% endkatex %} is defined by,

{% katex display %}
\psi_t = \sum_{n=0}^{N-1} x_{n} y_{n+t}\ \ \ \ \ (1),
{% endkatex %}

where {% katex %}t{% endkatex %} is called the *time lag*. Cross correlation provides a measure of the similarity of two time series when displaced by the time lag. A direct calculation of the cross correlation using the
equation above requires {% katex %}O(N^2){% endkatex %} operations. The Cross Correlation Theorem
provides a method for calculating cross correlation requiring {% katex %}O(NlogN){% endkatex %}
operations by use of the
[Fast Fourier Transform](https://en.wikipedia.org/wiki/Fast_Fourier_transform). Here the theoretical
background required to understand cross correlation calculations using the Cross Correlation Theorem is discussed
and illustrative examples are solved.

<!--more-->

## Properties of Cross Correlation

Cross Correlation can be understood by considering the [Covariance](https://en.wikipedia.org/wiki/Covariance) of
two random variables, {% katex %}X{% endkatex %} and {% katex %}Y{% endkatex %}, which is the
[Expectation](https://en.wikipedia.org/wiki/Expected_value) of the product of the deviations of the random variables
from their respective means, namely,

{% katex display %}
\begin{aligned}
Cov(X,Y) &= E\left[(X-E[X])(Y-E[Y])\right] \\
&= E[XY] - E[X]E[Y].
\end{aligned}\ \ \ \ \ (2)
{% endkatex %}

Note that {% katex %}Cov(X,Y)=Cov(Y,X){% endkatex %} and if {% katex %}X{% endkatex %} and
{% katex %}Y{% endkatex %} are
[Independent Random Variables](https://en.wikipedia.org/wiki/Independence_(probability_theory))
{% katex %}E[XY]=E[X]E[Y]\ \implies\ Cov(X,Y)=0{% endkatex %}.
If {% katex %}X=Y{% endkatex %} the Covariance reduces to the [Variance](https://en.wikipedia.org/wiki/Variance),

{% katex display %}
\begin{aligned}
Var(X) &= E\left[(X-E[X])^2\right] \\
&= E[X^2] - \left(E[X]\right)^2.
\end{aligned}
{% endkatex %}

These two results are combined in the definition of the
[Correlation Coefficient](https://en.wikipedia.org/wiki/Pearson_correlation_coefficient),

{% katex display %}
\rho = \frac{Cov(X,Y)}{\sqrt{Var(X)Var(Y)}}.
{% endkatex %}

The correlation coefficient has a geometric interpretation leading to the conclusion that
{% katex %}-1\ \leq \rho\ \leq 1{% endkatex %}. At the extreme values {% katex %}\rho{% endkatex %}
{% katex %}X{% endkatex %} and {% katex %}Y{% endkatex %} are either the same or differ by a
multiple of {% katex %}-1{% endkatex %} and the at the midpoint value of {% katex %}0{% endkatex %}
{% katex %}X{% endkatex %} and {% katex %}Y{% endkatex %} are independent random variables. If follows that
{% katex %}\rho{% endkatex %} provides a measure of possible *dependence* or *similarity* of two random variables.

Now, consider the first term in equation {% katex %}(2){% endkatex %} when a time series of samples
of {% katex %}X{% endkatex %} and {% katex %}Y{% endkatex %} of equal length are available,

{% katex display %}
\begin{aligned}
E[XY]& \approx \frac{1}{N^2}\sum_{n=0}^{N-1}x_n y_n \\
&\propto \psi_{0},
\end{aligned}
{% endkatex %}

if {% katex %}N{% endkatex %} is sufficiently large. Generalizing this result to arbitrary time shifts leads to
equation {% katex %}(1){% endkatex %}.

An important special case of the Cross Correlation is the [Autocorrelation](https://en.wikipedia.org/wiki/Autocorrelation) which is defined a the Cross
Correlation of a time series with itself, namely,

{% katex display %}
r_t = \sum_{n=0}^{N-1} x_{n} x_{n+t}\ \ \ \ \ (3).
{% endkatex %}

Building on the interpretation of Cross Correlation the Autocorrelation is viewed as a measure of *dependence*
or *similarity* of the past and future of a time series. For a time lag of {% katex %}t=0{% endkatex %},

{% katex display %}
r_0\ \propto\ E\left[X^2\right].
{% endkatex %}

This leads to the normalization,

{% katex display %}
\gamma_t = \frac{r_t}{r_0}\ \ \ \ \ (4),
{% endkatex %}

where {% katex %}-1\ \leq \gamma_t\ \leq\ 1{% endkatex %}.

## Discrete Fourier Transform
