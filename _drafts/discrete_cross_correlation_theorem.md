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
of two discrete time series is {% katex %}\{f_0,\ f_1,\ f_2,\ldots\,\ f_{N-1}\}{% endkatex %} and
{% katex %}\{g_0,\ g_1,\ g_2,\ldots\,\ g_{N-1}\}{% endkatex %} is defined by,

{% katex display %}
\psi_t = \sum_{n=0}^{N-1} f_{n} g_{n+t}\ \ \ \ \ (1),
{% endkatex %}

where {% katex %}t{% endkatex %} is called the *time lag*. Cross correlation provides a measure of the similarity of two time series when displaced by the time lag. A direct calculation of the cross correlation using the
equation above requires {% katex %}O(N^2){% endkatex %} operations. The Cross Correlation Theorem
provides a method for calculating cross correlation requiring {% katex %}O(NlogN){% endkatex %}
operations by use of the
[Fast Fourier Transform](https://en.wikipedia.org/wiki/Fast_Fourier_transform). Here the theoretical
background required to understand cross correlation calculations using the Cross Correlation Theorem is discussed
and illustrative examples are solved.

<!--more-->

## Cross Correlation

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
\rho_{XY} = \frac{Cov(X,Y)}{\sqrt{Var(X)Var(Y)}}.
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
&\propto\ \psi_{0},
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

## Discrete Fourier Transform

This section will discuss properties of the Discrete Fourier Transform that are used in following discussions.
The [Discrete Fourier Transform](https://en.wikipedia.org/wiki/Discrete_Fourier_transform) for a discrete periodic
time series of length {% katex %}N{% endkatex %}, {% katex %}\{f_0,\ f_1,\ f_2,\ldots,\ f_{N-1}\}{% endkatex %},
is defined by,

{% katex display %}
\begin{gathered}
f_{n} = \frac{1}{N}\sum_{k=0}^{N-1}F_{k}e^{2\pi i (k/N)n} \\
F_{k} = \sum_{n=0}^{N-1}f_{n}e^{-2\pi i (n/N)k},
\end{gathered}\ \ \ \ \ (4)
{% endkatex %}

where the expression for {% katex %}f_{n}{% endkatex %} is referred to the inverse transform and the one for
{% katex %}F_{k}{% endkatex %} the forward transform. Several properties of the transform are discussed in the following sections.

### Linearity

Both the forward and inverse transforms are [Linear Operators](http://mathworld.wolfram.com/LinearOperator.html).
An operator, {% katex %}\mathfrak{F}{% endkatex %}, is linear if the operation on a sum is equal the sum of the operations,

{% katex display %}
\mathfrak{F}(a+b) = \mathfrak{F}(a)+\mathfrak{F}(b)\ \ \ \ \ (5)
{% endkatex %}

To show this for the forward transform consider {% katex %}h_{n}=f_{n}+g_{n}{% endkatex %}, then,

{% katex display %}
\begin{aligned}
H_{k} &= \sum_{n=0}^{N-1}h_{n}e^{-2\pi i (n/N)k} \\
&= \sum_{n=0}^{N-1}\left(f_{n}+g_{n}\right)e^{-2\pi i (n/N)k} \\
&= \sum_{n=0}^{N-1}f_{n} e^{-2\pi i (n/N)k} + \sum_{n=0}^{N-1} g_{n}e^{-2\pi i (n/N)k} \\
&= F_{k} + G_{k}.
\end{aligned}
{% endkatex %}

Similarly it can be shown that the inverse transform is linear.

### Periodicity

Periodicity of the forward transform implies that,
{% katex display %}
F_{k+mN}=F_{k}\ \ \ \ \ (6),
{% endkatex %}
where {% katex %}m=\{\ldots,-2,\ -1,\ 0,\ 1,\ 2,\ldots\}{% endkatex %}.
To show that this is true first consider the case
{% katex %}m=1{% endkatex %},

{% katex display %}
\begin{aligned}
F_{k+N} &= \sum_{n=0}^{N} f_{n}e^{-2\pi i (n/N)(k+N)} \\
&= \sum_{n=0}^{N} f_{n} e^{-2\pi i(n/N)k}e^{-2\pi i n} \\
&= \sum_{n=0}^{N} f_{n} e^{-2\pi i(n/N)k} \\
&= F_{k},
\end{aligned}
{% endkatex %}

where the second step follows from,

{% katex display %}
e^{2\pi i n} = 1,\ \forall\ n
{% endkatex %}

For and arbitrary value of {% katex %}m{% endkatex %},
{% katex display %}
\begin{aligned}
F_{k+mN} &= \sum_{n=0}^{N} f_{n}e^{-2\pi i (n/N)(k+mN)} \\
&= \sum_{n=0}^{N} f_{n} e^{-2\pi i(n/N)k}e^{-2\pi i nm} \\
&= \sum_{n=0}^{N} f_{n} e^{-2\pi i(n/N)k} \\
&= F_{k},
\end{aligned}
{% endkatex %}

since,

{% katex display %}
e^{2\pi i mn} = 1,\ \forall\ m,\ n
{% endkatex %}

### Consequence Real {% katex %}f_{n}{% endkatex %}

If {% katex %}f_{n}{% endkatex %} is real then {% katex %}f_{n}=f_{n}^{\ast}{% endkatex %}, where
{% katex %}\ast{% endkatex %} denotes the [Complex Conjugate](https://en.wikipedia.org/wiki/Complex_conjugate).
It follows that,

{% katex display %}
\begin{aligned}
f_{n} &= f_{n}^{\ast} \\
&= \left\{ \frac{1}{N}\sum_{k=0}^{N-1}F_{k}e^{2\pi i (k/N)n} \right\}^{\ast} \\
&= \frac{1}{N}\sum_{k=0}^{N-1}F_{k}^{\ast}e^{-2\pi i (k/N)n}
\end{aligned}\ \ \ \ \ (7)
{% endkatex %}

Another interesting and related result is that,

{% katex display %}
F_{-k} = F_{k}^{\ast}\ \ \ \ \ (8).
{% endkatex %}

which follows from,

{% katex display %}
\begin{aligned}
F_{-k} &= \sum_{n=0}^{N-1} f_{n}e^{2\pi i (n/N)k} \\
&= \sum_{n=0}^{N-1} f_{n}^{\ast}e^{2\pi i (n/N)k} \\
&= \left\{ \sum_{n=0}^{N-1} f_{n}e^{-2\pi i (n/N)k}\right\}^{\ast} \\
&=F_{k}^{\ast}.
\end{aligned}
{% endkatex %}

## Orthogonality of Fourier Basis

The Discrete Fourier Basis is the collection of functions,

{% katex display %}
e^{2\pi i(k/N)n}.
{% endkatex %}

This collection of functions is an [Orthogonal Basis](https://en.wikipedia.org/wiki/Orthogonal_basis) since,

{% katex display %}
\frac{1}{N} \sum_{n=0}^{N-1} e^{2\pi \left[ (m-k)/N\right] n}\ =\ \delta_{mk} =
\begin{cases}
1 & \text{if}\ m\ =\ k \\
0 & \text{if}\ m\ \ne\  k
\end{cases}\ \ \ \ \ (9),
{% endkatex %}

where {% katex %}\delta_{mk}{% endkatex %} is the [Kronecker Delta](https://en.wikipedia.org/wiki/Kronecker_delta).
This result can be proven by noting that the sum in equation {% katex %}(9){% endkatex %}
is a [Geometric Series](https://en.wikipedia.org/wiki/Geometric_series),

{% katex display %}
\frac{1}{N} \sum_{n=0}^{N-1} e^{2\pi i \left[(m-k)/N \right] n} = \frac{1}{N} \frac{1-e^{2\pi i (m-k)}}{1-e^{2\pi i (m-k)/N}}.
{% endkatex %}

First assume that {% katex %}m\ \ne\ k{% endkatex %}. Since {% katex %}2\pi i(m-k){% endkatex %} is
always a multiple of {% katex %}2\pi{% endkatex %} it follows that the numerator is zero,

{% katex display %}
1-e^{2\pi i (m-k)} = 1-1 = 0.
{% endkatex %}

The denominator is zero only if {% katex %}m-k=lN{% endkatex %} where {% katex %}l{% endkatex %} is
an integer. This cannot happen since {% katex %}-(N-1)\ \leq m-k\ \leq N-1{% endkatex %}, so,

{% katex display %}
\sum_{n=0}^{N-1} e^{2\pi \left[ (m-k)/N\right] n}\ =\ 0
{% endkatex %}

If {% katex %}m=k{% endkatex %} then,

{% katex display %}
\sum_{n=0}^{N-1} e^{2\pi i \left[(m-k)/N \right] n} = \sum_{n=0}^{N-1} 1 = N,
{% endkatex %}

this proves that equation {% katex %}(9){% endkatex %}.

## The Cross Correlation Theorem

The Cross Correlation Theorem is relationship between the Fourier Transform of the cross correlation,
{% katex %}\psi_{t}{% endkatex %} defined by equation {% katex %}(1){% endkatex %} and the
Fourier Transforms of the two time series used in the cross correlation calculation, namely,

{% katex display %}
\Psi_{k} = F_{k}^{\ast}G_{k}\ \ \ \ \ (10),
{% endkatex %}

where,

{% katex display %}
\begin{aligned}
\Psi_{k} &= \sum_{n=0}^{N-1}\psi_{n}e^{-2\pi i (n/N)k} \\
F_{k}^{\ast} &= \sum_{n=0}^{N-1}f_{n}e^{2\pi i (n/N)k} \\
G_{k} &= \sum_{n=0}^{N-1}g_{n}e^{-2\pi i (n/N)k}.
\end{aligned}
{% endkatex %}

Now to derive equation {% katex %}(10){% endkatex %} consider the Inverse Fourier Transform of the time
series {% katex %}f_{n}{% endkatex %} and {% katex %}g_{n+t}{% endkatex %},

{% katex display %}
\begin{gathered}
f_{n} = \frac{1}{N}\sum_{k=0}^{N-1}F_{k}^{\ast}e^{-2\pi i (k/N)n} \\
g_{n+t} = \frac{1}{N}\sum_{k=0}^{N-1}G_{k}e^{2\pi i (k/N)(n+t)},
\end{gathered}
{% endkatex %}

where use was made of equation {% katex %}(7){% endkatex %} when writing the inverse transform for
{% katex %}f_{n}{% endkatex %}.

Substituting these expressions into equation {% katex %}(1){% endkatex %} gives,

{% katex display %}
\begin{aligned}
\psi_t &= \sum_{n=0}^{N-1} f_{n} g_{n+t} \\
&= \sum_{n=0}^{N-1} \left\{   \frac{1}{N}\sum_{k=0}^{N-1}F_{k}^{\ast}e^{-2\pi i (k/N)n} \right\} \left\{ \frac{1}{N}\sum_{m=0}^{N-1}G_{m}e^{2\pi i (m/N)(n+t)} \right\} \\
&= \frac{1}{N}\sum_{k=0}^{N-1}\sum_{m=0}^{N-1} F_{k}^{\ast}G_{m} e^{2\pi i (t/N)m} \frac{1}{N} \sum_{n=0}^{N-1} e^{2\pi i \left[(m-k)/N \right] n} \\
&= \frac{1}{N}\sum_{k=0}^{N-1}\sum_{m=0}^{N-1} F_{k}^{\ast}G_{m} e^{2\pi i (t/N)m} \delta_{mk} \\
&= \frac{1}{N}\sum_{k=0}^{N-1} F_{k}^{\ast}G_{k} e^{2\pi i (t/N)k}.
\end{aligned}
{% endkatex %}

Equation {% katex %}(10){% endkatex %} follows by taking the Fourier Transform of the previous result,

{% katex display %}
\begin{aligned}
\Psi_{k} &= \sum_{t=0}^{N-1}\psi_{t}e^{-2\pi i (t/N)k} \\
&= \sum_{t=0}^{N-1} \left\{ \frac{1}{N}\sum_{k=0}^{N-1} F_{k}^{\ast}G_{k} e^{2\pi i (t/N)m} \right\}e^{-2\pi i (t/N)k} \\
&= \sum_{m=0}^{N-1} F_{m}^{\ast}G_{m} \frac{1}{N} \sum_{t=0}^{N-1} e^{2\pi i \left[(m-k)/N)\right]t} \\
&= \sum_{m=0}^{N-1} F_{m}^{\ast}G_{m} \delta_{mk} \\
&= F_{k}^{\ast}G_{k},
\end{aligned}
{% endkatex %}

which proves the Cross Correlation Theorem defined by equation {% katex %}(10){% endkatex %}.

## Cross Correlation Theorem Example

This section will work through an example calculation of cross correlation which can be worked out by hand.
The manual calculations will be compared with calculations performed using the FFT library from `numpy`.

Discrete Fourier Transform of a time series, represented by the column vector {% katex %}f{% endkatex %}, into
a column vector of Fourier coefficients, {% katex %}F{% endkatex %}, can be represented by the linear equation,

{% katex display %}
F = Tf
{% endkatex %}

Where {% katex %}T{% endkatex %} is the transform matrix computed from the Fourier basis functions.
Each row the matrix will be the value of the basis functions used for calculation a Fourier coefficient.
It is assumed that the time series contains only {% katex %}4{% endkatex %} points so that
{% katex %}T{% endkatex %} will be a {% katex %}4\times 4{% endkatex %} matrix. The transform
matrix only depends on the number of elements in the time series vector and is given by,

{% katex display %}
T=
\begin{pmatrix}
1 & 1 & 1 & 1 \\
1 & e^{-i\pi/2} & e^{-i\pi} & e^{-i3\pi/2} \\
1 & e^{-i\pi} & e^{-i2\pi} & e^{-i3\pi} \\
1 & e^{-i3\pi/2} & e^{-i3\pi} & e^{-i9\pi/2}
\end{pmatrix} =
\begin{pmatrix}
1 & 1 & 1 & 1 \\
1 & -i & -1 & i \\
1 & -1 & 1 & -1 \\
1 & i & -1 & -i
\end{pmatrix}
{% endkatex %}
