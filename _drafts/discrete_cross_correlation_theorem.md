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

## Discrete Fourier Transform Example

This section will work through an example calculation of a discrete Fourier Transform which can be worked
out by hand. The manual calculations will be compared with calculations performed using the FFT library
from `scipy`.

The Discrete Fourier Transform of a time series, given by the column vector {% katex %}f{% endkatex %},
into a column vector of Fourier coefficients, {% katex %}\overline{f}{% endkatex %}, can be represented by the linear
equation,

{% katex display %}
\overline{f} = Tf
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
\end{pmatrix} \ \ \ \ \ (11)
{% endkatex %}

Assume that example time series is given buy,

{% katex display %}
f =
\begin{pmatrix}
8 \\
4 \\
8 \\
0
\end{pmatrix}.
{% endkatex %}

It follows that,

{% katex display %}
\begin{aligned}
\begin{pmatrix}
\overline{f_1} \\
\overline{f_2} \\
\overline{f_3} \\
\overline{f_4}
\end{pmatrix}
&=
\begin{pmatrix}
1 & 1 & 1 & 1 \\
1 & -i & -1 & i \\
1 & -1 & 1 & -1 \\
1 & i & -1 & -i
\end{pmatrix}
\begin{pmatrix}
8 \\
4 \\
8 \\
0
\end{pmatrix} \\
&=
\begin{pmatrix}
20 \\
-4i \\
12 \\
4i
\end{pmatrix}
\end{aligned}\ \ \ \ \ (12)
{% endkatex %}

The Python code listing below uses the FFT implementation from `numpy` to confirm the calculation of
equation {% katex %}(12){% endkatex %}. It first defines the time series example data
{% katex %}f{% endkatex %}. The Fourier Transform provided by `fftpack` is then used to compute
{% katex %}\overline{f}{% endkatex %} followed by calculation of the inverse Fourier Transform
using {% katex %}\overline{f}{% endkatex %}

```python
In [1]: import numpy
In [2]: f = numpy.array([8, 4, 8, 0])
In [3]: numpy.fft.fft(f)
Out[3]: array([20.+0.j,  0.-4.j, 12.+0.j,  0.+4.j])
```
## Cross Correlation Theorem Example

This section will work through an example calculation of cross correlation using the Cross Correlation Theorem
with the goal of verifying an implementation of the algorithm in Python. Here use will be made of the
time series vector {% katex %}f{% endkatex %} and the transform matrix {% katex %}T{% endkatex %}
discussed in the previous section, but time series vector also needs to be considered, let,

{% katex display %}
g =
\begin{pmatrix}
6 \\
3 \\
9 \\
3
\end{pmatrix}.
{% endkatex %}

First, consider a direct calculation of the cross correlation defined by equation {% katex %}(1){% endkatex %}.
namely, {% katex %}\psi_t = \sum_{n=0}^{N-1} f_{n} g_{n+t}{% endkatex %}.
A convenient method of organizing the the calculation is shown in table below. The rows are constructed
from the all elements of {% katex %}f_{n}{% endkatex %} and the time lagged elements of
{% katex %}g_{n+t}{% endkatex %} for each value {% katex %}t{% endkatex %}. The column is
indexed by the element number {% katex %}n{% endkatex %}.
The time lag shift performed on the vector {% katex %}g_{n+t}{% endkatex %} results in the shown translation of
the components to the right that increases for each row as the time lag increases. Since the number of elements in
{% katex %}f{% endkatex %} and {% katex %}g{% endkatex %} is finite the time lag shift will lead to some
elements not participating in {% katex %}\psi_{t}{% endkatex %} for some time lag values. If there is no element
in the table at a position the value of {% katex %}f{% endkatex %} or {% katex %}g{% endkatex %} at that position
is assumed to be {% katex %}0{% endkatex %}.

| {% katex %}n{% endkatex %}      |     |     |     |  0  |  1  |  2  |  3 |
| :---: | :---:  | :---:  | :---: | :---:  | :---:  | :---: | :---: |
| {% katex %}f_{n}{% endkatex %}  |     |     |     |  8  |  4  |  8  |  0  |
| {% katex %}g_{n}{% endkatex %}  |     |     |     |  6  |  3  |  9  |  3  |
| {% katex %}g_{n+1}{% endkatex %}|     |     |  6  |  3  |  9  |  3  |     |
| {% katex %}g_{n+2}{% endkatex %}|     |  6  |  3  |  9  |  3  |     |     |
| {% katex %}g_{n+3}{% endkatex %}|  6  |  3  |  9  |  3  |     |     |     |

To compute the cross correlation, {% katex %}\psi_t{% endkatex %}, for a value of the time lag,
{% katex %}t{% endkatex %}, for each value of {% katex %}n{% endkatex %} multiply the value of
{% katex %}f_{n}{% endkatex %} and {% katex %}g_{n+t}{% endkatex %} summing the results.
The outcome of performing this calculation is shown as the column vector
{% katex %}\psi{% endkatex %} where each row corresponds to a different time lag value.

{% katex display %}
\psi =
\begin{pmatrix}
8\cdot 6 + 4\cdot 3 + 8\cdot 9 + 0\cdot 3 \\
8\cdot 3 + 4\cdot 9 + 8\cdot 3 \\
8\cdot 9 + 4\cdot 3 \\
8\cdot 3
\end{pmatrix}
=
\begin{pmatrix}
132 \\
84 \\
84 \\
24
\end{pmatrix}\ \ \ \ \ \ (13)
{% endkatex %}

| {% katex %}n{% endkatex %}      |  0  |  1  |  2  |  3  |
| :---: | :---:  | :---:  | :---: | :---: |
| {% katex %}f_{n}{% endkatex %}  |  8  |  4  |  8  |  0  |
| {% katex %}g_{n}{% endkatex %}  |  6  |  3  |  9  |  3  |
| {% katex %}g_{n+1}{% endkatex %}|  3  |  9  |  3  |  6  |
| {% katex %}g_{n+2}{% endkatex %}|  9  |  3  |  6  |  3  |
| {% katex %}g_{n+3}{% endkatex %}|  3  |  6  |  3  |  9  |

{% katex display %}
\psi =
\begin{pmatrix}
8\cdot 6 + 4\cdot 3 + 8\cdot 9 + 0\cdot 3 \\
8\cdot 3 + 4\cdot 9 + 8\cdot 3 + 0\cdot 6 \\
8\cdot 9 + 4\cdot 3 + 8\cdot 6 + 0\cdot 3 \\
8\cdot 3 + 4\cdot 6 + 8\cdot 3 + 0\cdot 9
\end{pmatrix}
=
\begin{pmatrix}
132 \\
84 \\
132 \\
72
\end{pmatrix}
{% endkatex %}

```python
In [1]: import numpy
In [2]: f = numpy.array([8, 4, 8, 0])
In [3]: g = numpy.array([6, 3, 9, 3])
In [4]: f_bar = numpy.fft.fft(f)
In [5]: g_bar = numpy.fft.fft(g)
In [6]: numpy.fft.ifft(f_bar * g_bar)
Out[6]: array([132.+0.j,  72.+0.j, 132.+0.j,  84.+0.j])
```

| {% katex %}n{% endkatex %}      |     |     |     |  0  |  1  |  2  |  3  |
| :---: | :---:  | :---:  | :---: | :---:  | :---:  | :---: | :---: |
| {% katex %}f_{n}{% endkatex %}  |  0  |  0  |  0  |  8  |  4  |  8  |  0  |
| {% katex %}g_{n}{% endkatex %}  |  0  |  0  |  0  |  6  |  3  |  9  |  3  |
| {% katex %}g_{n+1}{% endkatex %}|  0  |  0  |  6  |  3  |  9  |  3  |  0  |
| {% katex %}g_{n+2}{% endkatex %}|  0  |  6  |  3  |  9  |  3  |  0  |  0  |
| {% katex %}g_{n+3}{% endkatex %}|  6  |  3  |  9  |  3  |  0  |  0  |  0  |
| {% katex %}g_{n+4}{% endkatex %}|  3  |  9  |  3  |  0  |  0  |  0  |  6  |
| {% katex %}g_{n+5}{% endkatex %}|  9  |  3  |  0  |  0  |  0  |  6  |  3  |
| {% katex %}g_{n+6}{% endkatex %}|  3  |  0  |  0  |  0  |  6  |  3  |  9  |

{% katex display %}
\psi =
\begin{pmatrix}
8\cdot 6 + 4\cdot 3 + 8\cdot 9 + 3\cdot 0 \\
8\cdot 3 + 4\cdot 9 + 8\cdot 3 + 0\cdot 0 \\
8\cdot 9 + 4\cdot 3 + 8\cdot 0 + 0\cdot 0 \\
8\cdot 3 + 4\cdot 0 + 8\cdot 0 + 0\cdot 0 \\
8\cdot 0 + 4\cdot 0 + 8\cdot 0 + 0\cdot 3 \\
8\cdot 0 + 4\cdot 0 + 8\cdot 6 + 0\cdot 3 \\
8\cdot 0 + 4\cdot 6 + 8\cdot 3 + 0\cdot 9
\end{pmatrix}
=
\begin{pmatrix}
132 \\
84 \\
84 \\
24 \\
0 \\
48 \\
48
\end{pmatrix}
{% endkatex %}

```python
In [1]: import numpy
In [2]: f = numpy.array([8, 4, 8, 0])
In [3]: g = numpy.array([6, 3, 9, 3])
In [4]: f_bar = numpy.fft.fft(numpy.concatenate((f, numpy.zeros(len(f)-1))))
In [5]: g_bar = numpy.fft.fft(numpy.concatenate((g, numpy.zeros(len(g)-1))))
In [6]: numpy.fft.ifft(numpy.conj(f_bar) * g_bar)
Out[7]:
array([1.3200000e+02+0.j, 8.4000000e+01+0.j, 8.4000000e+01+0.j,
       2.4000000e+01+0.j, 4.0602442e-15+0.j, 4.8000000e+01+0.j,
       4.8000000e+01+0.j])
```

## Autocorrelation
