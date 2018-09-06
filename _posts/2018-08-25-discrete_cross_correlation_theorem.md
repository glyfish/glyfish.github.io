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
of two discrete finite time series {% katex %}\{f_0,\ f_1,\ f_2,\ldots\,\ f_{N-1}\}{% endkatex %} and
{% katex %}\{g_0,\ g_1,\ g_2,\ldots\,\ g_{N-1}\}{% endkatex %} is defined by,

{% katex display %}
\psi_t = \sum_{n=0}^{N-1} f_{n} g_{n+t}\ \ \ \ \ (1),
{% endkatex %}

where {% katex %}t{% endkatex %} is called the *time lag*. Cross correlation provides a measure of the similitude
of two time series when shifted by the time lag. A direct calculation of the cross correlation using the
equation above requires {% katex %}O(N^2){% endkatex %} operations. The Cross Correlation Theorem
provides a method for calculating cross correlation in {% katex %}O(NlogN){% endkatex %}
operations by use of the
[Fast Fourier Transform](https://en.wikipedia.org/wiki/Fast_Fourier_transform). Here the theoretical
background required to understand cross correlation calculations using the Cross Correlation Theorem is discussed.
Example calculations are performed and different implementations using the FFT libraries in `numpy` compared.
The important special case of the cross correlation called [Autocorrelation](https://en.wikipedia.org/wiki/Autocorrelation) is addressed in the final section.

<!--more-->

## Cross Correlation

Cross Correlation can be understood by considering the [Covariance](https://en.wikipedia.org/wiki/Covariance) of
two random variables, {% katex %}X{% endkatex %} and {% katex %}Y{% endkatex %}. Covariance is the
[Expectation](https://en.wikipedia.org/wiki/Expected_value) of the product of the deviations of the random variables
from their respective means,

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
{% katex %}-1\ \leq \rho_{XY}\ \leq 1{% endkatex %}. At the extreme values
{% katex %}X{% endkatex %} and {% katex %}Y{% endkatex %} are either the same or differ by a
multiple of {% katex %}-1{% endkatex %}. At the midpoint value, {% katex %}\rho_{XY}=0{% endkatex %},
{% katex %}X{% endkatex %} and {% katex %}Y{% endkatex %} are independent random variables. If follows that
{% katex %}\rho_{XY}{% endkatex %} provides a measure of possible *dependence* or *similarity* of two random variables.

Consider the first term in equation {% katex %}(2){% endkatex %} when a time series of samples
of {% katex %}X{% endkatex %} and {% katex %}Y{% endkatex %} of equal length are available,

{% katex display %}
\begin{aligned}
E[XY]& \approx \frac{1}{N^2}\sum_{n=0}^{N-1}x_n y_n \\
&\propto\ \psi_{0},
\end{aligned}
{% endkatex %}

if {% katex %}N{% endkatex %} is sufficiently large. Generalizing this result to arbitrary time shifts leads to
equation {% katex %}(1){% endkatex %}.

An important special case of the cross correlation is the autocorrelation which is defined a the cross
correlation of a time series with itself,

{% katex display %}
r_t = \sum_{n=0}^{N-1} x_{n} x_{n+t}\ \ \ \ \ (3).
{% endkatex %}

Building on the interpretation of cross correlation the autocorrelation is viewed as a measure of *dependence*
or *similarity* of the past and future of a time series. For a time lag of {% katex %}t=0{% endkatex %},

{% katex display %}
r_0\ \propto\ E\left[X^2\right].
{% endkatex %}

## Discrete Fourier Transform

This section will discuss properties of the Discrete Fourier Transform that are used in following sections.
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
{% katex %}F_{k}{% endkatex %} the forward transform.

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

where the second step follows from, {% katex %}e^{2\pi i n} = 1,\ \forall\ n{% endkatex %}.

For an arbitrary value of {% katex %}m{% endkatex %},
{% katex display %}
\begin{aligned}
F_{k+mN} &= \sum_{n=0}^{N} f_{n}e^{-2\pi i (n/N)(k+mN)} \\
&= \sum_{n=0}^{N} f_{n} e^{-2\pi i(n/N)k}e^{-2\pi i nm} \\
&= \sum_{n=0}^{N} f_{n} e^{-2\pi i(n/N)k} \\
&= F_{k},
\end{aligned}
{% endkatex %}

since, {% katex %}e^{2\pi i mn} = 1,\ \forall\ m,\ n{% endkatex %}.

### Consequence of Real {% katex %}f_{n}{% endkatex %}

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

Another interesting and related result is,

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
\left\{e^{2\pi i(k/N)n}\right\},
{% endkatex %}

 where {% katex %}n = 0,\ 1,\ 2,\ldots,N-1{% endkatex %}. It forms an [Orthogonal Basis](https://en.wikipedia.org/wiki/Orthogonal_basis) since,

{% katex display %}
\frac{1}{N} \sum_{n=0}^{N-1} e^{2\pi \left[ (m-k)/N\right] n}\ =\ \delta_{mk} =
\begin{cases}
1 & \text{if}\ m\ =\ k \\
0 & \text{if}\ m\ \ne\  k
\end{cases}\ \ \ \ \ (9),
{% endkatex %}

where {% katex %}\delta_{mk}{% endkatex %} is the [Kronecker Delta](https://en.wikipedia.org/wiki/Kronecker_delta).
This result can be proven for {% katex %}m\ \ne\ k{% endkatex %} by noting that the sum in
equation {% katex %}(9){% endkatex %} is a [Geometric Series](https://en.wikipedia.org/wiki/Geometric_series),

{% katex display %}
\frac{1}{N} \sum_{n=0}^{N-1} e^{2\pi i \left[(m-k)/N \right] n} = \frac{1}{N} \frac{1-e^{2\pi i (m-k)}}{1-e^{2\pi i (m-k)/N}}.
{% endkatex %}

Since {% katex %}2\pi i(m-k){% endkatex %} is
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

The Cross Correlation Theorem is a relationship between the Fourier Transform of the cross correlation,
{% katex %}\psi_{t}{% endkatex %} defined by equation {% katex %}(1){% endkatex %} and the
Fourier Transforms of the two time series used in the cross correlation calculation,

{% katex display %}
\Psi_{k} = F_{k}^{\ast}G_{k}\ \ \ \ \ (10),
{% endkatex %}

where,

{% katex display %}
\begin{aligned}
\Psi_{k} &= \sum_{n=0}^{N-1}\psi_{n}e^{-2\pi i (n/N)k} \\
F_{k}^{\ast} &= \sum_{n=0}^{N-1}f_{n}^{\ast}e^{2\pi i (n/N)k} \\
G_{k} &= \sum_{n=0}^{N-1}g_{n}e^{-2\pi i (n/N)k}.
\end{aligned}
{% endkatex %}

To derive equation {% katex %}(10){% endkatex %} consider the Inverse Fourier Transform of the time
series {% katex %}f_{n}{% endkatex %} and {% katex %}g_{n+t}{% endkatex %},

{% katex display %}
\begin{gathered}
f_{n} = \frac{1}{N}\sum_{k=0}^{N-1}F_{k}^{\ast}e^{-2\pi i (k/N)n} \\
g_{n+t} = \frac{1}{N}\sum_{k=0}^{N-1}G_{k}e^{2\pi i (k/N)(n+t)},
\end{gathered}
{% endkatex %}

Substituting these expressions into equation {% katex %}(1){% endkatex %} gives,

{% katex display %}
\begin{aligned}
\psi_t &= \sum_{n=0}^{N-1} f_{n} g_{n+t} \\
&= \sum_{n=0}^{N-1} \left\{   \frac{1}{N}\sum_{k=0}^{N-1}F_{k}^{\ast}e^{-2\pi i (k/N)n} \right\} \left\{ \frac{1}{N}\sum_{m=0}^{N-1}G_{m}e^{2\pi i (m/N)(n+t)} \right\} \\
&= \frac{1}{N}\sum_{k=0}^{N-1}\sum_{m=0}^{N-1} F_{k}^{\ast}G_{m} e^{2\pi i (t/N)m} \frac{1}{N} \sum_{n=0}^{N-1} e^{2\pi i \left[(m-k)/N \right] n} \\
&= \frac{1}{N}\sum_{k=0}^{N-1}\sum_{m=0}^{N-1} F_{k}^{\ast}G_{m} e^{2\pi i (t/N)m} \delta_{mk} \\
&= \frac{1}{N}\sum_{k=0}^{N-1} F_{k}^{\ast}G_{k} e^{2\pi i (t/N)k},
\end{aligned}
{% endkatex %}

where the second step follows from equation {% katex %}(9){% endkatex %}. Equation {% katex %}(10){% endkatex %} follows by taking the Fourier Transform of the previous result,

{% katex display %}
\begin{aligned}
\Psi_{k} &= \sum_{t=0}^{N-1}\psi_{t}e^{-2\pi i (t/N)k} \\
&= \sum_{t=0}^{N-1} \left\{ \frac{1}{N}\sum_{k=0}^{N-1} F_{k}^{\ast}G_{k} e^{2\pi i (t/N)m} \right\}e^{-2\pi i (t/N)k} \\
&= \sum_{m=0}^{N-1} F_{m}^{\ast}G_{m} \frac{1}{N} \sum_{t=0}^{N-1} e^{2\pi i \left[(m-k)/N)\right]t} \\
&= \sum_{m=0}^{N-1} F_{m}^{\ast}G_{m} \delta_{mk} \\
&= F_{k}^{\ast}G_{k},
\end{aligned}
{% endkatex %}

proving the Cross Correlation Theorem defined by equation {% katex %}(10){% endkatex %}.

## Discrete Fourier Transform Example

This section will work through an example calculation of a discrete Fourier Transform that can be worked
out by hand. The manual calculations will be compared with calculations performed using the FFT library
from `numpy`.

The Discrete Fourier Transform of a time series, represented by the column vector {% katex %}f{% endkatex %},
into a column vector of Fourier coefficients, {% katex %}\overline{f}{% endkatex %}, can be represented by the linear
equation,

{% katex display %}
\overline{f} = Tf,
{% endkatex %}

where {% katex %}T{% endkatex %} is the transform matrix computed from the Fourier basis functions.
Each element of the matrix is the value of the basis function used in the calculation.
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

Assume an example time series of,

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
{% katex %}f{% endkatex %}. The Fourier Transform is then used to compute
{% katex %}\overline{f}{% endkatex %}.

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
discussed in the previous section. An additional time series vector also needs to be considered, let,

{% katex display %}
g =
\begin{pmatrix}
6 \\
3 \\
9 \\
3
\end{pmatrix}.
{% endkatex %}

First, consider a direct calculation of the cross correlation,
{% katex %}\psi_t = \sum_{n=0}^{N-1} f_{n} g_{n+t}{% endkatex %}. The following python function
`cross_correlate_sum(x, y)` implements the required summation.

```python
In [1]: import numpy
In [2]: def cross_correlate_sum(x, y):
   ...:     n = len(x)
   ...:     correlation = numpy.zeros(len(x))
   ...:     for t in range(n):
   ...:         for k in range(0, n - t):
   ...:             correlation[t] += x[k] * y[k + t]
   ...:     return correlation
   ...:

In [3]: f = numpy.array([8, 4, 8, 0])
In [4]: g = numpy.array([6, 3, 9, 3])
In [5]: cross_correlate_sum(f, g)
Out[5]: array([132.,  84.,  84.,  24.])
```

`cross_correlate_sum(x, y)` takes two vectors, `x` and `y`, as arguments. It assumes that `x` and `y` are equal length and after allocating storage for the result performs the double summation required to compute the cross
correlation for all possible time lags, returning the result. It is also seen that
{% katex %}O(N^2){% endkatex %} operations are required to perform the calculation, where
{% katex %}N{% endkatex %} is the length of the input vectors.

Verification of following results requires a manual calculation. A method of organizing the calculation to facilitate  
this is shown in table below. The table rows are constructed from the elements of
{% katex %}f_{n}{% endkatex %} and the time lagged
elements of {% katex %}g_{n+t}{% endkatex %} for each value {% katex %}t{% endkatex %}.
The column is indexed by the element number
{% katex %}n{% endkatex %}. The time lag shift performed on the vector {% katex %}g_{n+t}{% endkatex %} results
in the translation of the components to the left that increases for each row as the time lag increases. Since the number of elements in
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

The cross correlation, {% katex %}\psi_t{% endkatex %}, for a value of the time lag,
{% katex %}t{% endkatex %}, is computed for each {% katex %}n{% endkatex %} by multiplication
of {% katex %}f_{n}{% endkatex %} and {% katex %}g_{n+t}{% endkatex %} and summing the results.
The outcome of this calculation is shown as the column vector
{% katex %}\psi{% endkatex %} below where each row corresponds to a different time lag value.

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

The result is the same as determined by `cross_correlate_sum(x,y)`.

Now that an expectation of a result is established it can be compared with the
a calculation using using the Cross Correlation Theorem from equation {% katex %}(10){% endkatex %}
where {% katex %}f{% endkatex %} and {% katex %}g{% endkatex %} are represented by Discrete Fourier
Series. It was previously shown that the Fourier representation is periodic, see
equation {% katex %}(6){% endkatex %}. It follows that the time lag shifts of
{% katex %}g_{n+t}{% endkatex %} will by cyclic as shown in the calculation table below.

| {% katex %}n{% endkatex %}      |  0  |  1  |  2  |  3  |
| :---: | :---:  | :---:  | :---: | :---: |
| {% katex %}f_{n}{% endkatex %}  |  8  |  4  |  8  |  0  |
| {% katex %}g_{n}{% endkatex %}  |  6  |  3  |  9  |  3  |
| {% katex %}g_{n+1}{% endkatex %}|  3  |  9  |  3  |  6  |
| {% katex %}g_{n+2}{% endkatex %}|  9  |  3  |  6  |  3  |
| {% katex %}g_{n+3}{% endkatex %}|  3  |  6  |  3  |  9  |

Performing the calculation following the steps previously described has the following outcome,

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
\end{pmatrix}.
{% endkatex %}

This is different from what was obtained from a direct evaluation of the cross correlation sum. Even though
the result is different the calculation could be correct since periodicity of {% katex %}f{% endkatex %}
and {% katex %}g{% endkatex %} was not assumed when the sum was evaluated. Below a calculation
using the Cross Correlation Theorem implemented in Python is shown.

```python
In [1]: import numpy
In [2]: f = numpy.array([8, 4, 8, 0])
In [3]: g = numpy.array([6, 3, 9, 3])
In [4]: f_bar = numpy.fft.fft(f)
In [5]: g_bar = numpy.fft.fft(g)
In [6]: numpy.fft.ifft(f_bar * g_bar)
Out[6]: array([132.+0.j,  72.+0.j, 132.+0.j,  84.+0.j])
```

In the calculation {% katex %}f{% endkatex %} and {% katex %}g{% endkatex %} are defined and their Fourier Transforms
are computed. The Cross Correlation Theorem is then used to compute the Fourier Transform of the cross correlation, which is then inverted. The result obtained is the same as obtained in the
manual calculation verifying the results. Since the calculations seem to be correct the problem must be that
periodicity of the Fourier representations {% katex %}f{% endkatex %} and {% katex %}g{% endkatex %} was
not handled properly. This analysis *artifact* is called [aliasing](https://en.wikipedia.org/wiki/Aliasing).
The following example attempts to correct for this problem.

The cross correlation calculation table for periodic {% katex %}f{% endkatex %} and
{% katex %}g{% endkatex %} can be made to resemble the table for the nonperiodic case by
padding the end of the vectors with {% katex %}N-1{% endkatex %} zeros, where {% katex %}N{% endkatex %} is
the vector length, creating a new periodic vector of length {% katex %}2N-1{% endkatex %}. This
new construction is shown below.

| {% katex %}n{% endkatex %}      |     |     |     |  0  |  1  |  2  |  3  |  4  |  5  |  6  |
| :---: | :---:  | :---:  | :---: | :---:  | :---:  | :---: | :---: | :---: | :---: | :---: | :---: |
| {% katex %}f_{n}{% endkatex %}  |     |     |     |  8  |  4  |  8  |  0  |     |     |     |
| {% katex %}g_{n}{% endkatex %}  |     |     |     |  6  |  3  |  9  |  3  |  0  |  0  |  0  |
| {% katex %}g_{n+1}{% endkatex %}|     |     |  6  |  3  |  9  |  3  |  0  |  0  |  0  |  6  |
| {% katex %}g_{n+2}{% endkatex %}|     |  6  |  3  |  9  |  3  |  0  |  0  |  0  |  6  |  3  |
| {% katex %}g_{n+3}{% endkatex %}|  6  |  3  |  9  |  3  |  0  |  0  |  0  |  6  |  3  |  9  |
| {% katex %}g_{n+4}{% endkatex %}|  3  |  9  |  3  |  0  |  0  |  0  |  6  |  3  |  9  |  3  |
| {% katex %}g_{n+5}{% endkatex %}|  9  |  3  |  0  |  0  |  0  |  6  |  3  |  9  |  3  |  0  |
| {% katex %}g_{n+6}{% endkatex %}|  3  |  0  |  0  |  0  |  6  |  3  |  9  |  3  |  0  |  0  |

It follows that,

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

The first {% katex %}N{% endkatex %} elements of this result are the same as obtained calculating the sum
directly. The same result is achieved by discarding the last {% katex %}N-1{% endkatex %} elements.
Verification is shown below where the previous example is extended by padding the tail of both
{% katex %}f{% endkatex %} and {% katex %}g{% endkatex %} with three zeros.

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
The following function, `cross_correlate(x, y)`, generalizes the calculation to vectors of arbitrary but equal lengths.

```python
def cross_correlate(x, y):
    n = len(x)
    x_padded = numpy.concatenate((x, numpy.zeros(n-1)))
    y_padded = numpy.concatenate((y, numpy.zeros(n-1)))
    x_fft = numpy.fft.fft(x_padded)
    y_fft = numpy.fft.fft(y_padded)
    h_fft = numpy.conj(x_fft) * y_fft
    cc = numpy.fft.ifft(h_fft)
    return cc[0:n]
```

## Autocorrelation

The autocorrelation, defined by equation {% katex %}(3){% endkatex %}, is the special case of the
cross correlation of a time series with itself. It provides a measure of the *dependence* or
*similarity* of its past and future. The version of the Cross Correlation Theorem for autocorrelation is
given by,

{% katex display %}
R_{k} = F^{\ast}_{k}F_{k} = {\mid F_{k} \mid}^{2}\ \ \ \ \ (14).
{% endkatex %}

{% katex %}R_{k}{% endkatex %} is the *weight* of each of the coefficients in the Fourier Series
representation of the time series and is known as the [Power Spectrum](https://en.wikipedia.org/wiki/Spectral_density).

When discussing the autocorrelation of a time series, {% katex %}f{% endkatex %}, the autocorrelation coefficient is
useful,

{% katex display %}
\gamma_{\tau} = \frac{1}{\sigma_{f}^2}\sum_{n=0}^{t} \left(f_{n} - \mu_f \right) \left(f_{n+\tau} - \mu_f\right)\ \ \ \ \ (15),
{% endkatex %}

where {% katex %}\mu_{f}{% endkatex %} and {% katex %}\sigma_{f}{% endkatex %} are the time series mean and
standard deviation respectively. Below a Python implementation calculating the autocorrelation coefficient
is given.

```python
def autocorrelate(x):
    n = len(x)
    x_shifted = x - x.mean()
    x_padded = numpy.concatenate((x_shifted, numpy.zeros(n-1)))
    x_fft = numpy.fft.fft(x_padded)
    r_fft = numpy.conj(x_fft) * x_fft
    ac = numpy.fft.ifft(r_fft)
    return ac[0:n]/ac[0]
```

`autocorrelate(x)` takes a single argument, `x`, that is the time series used in the calculation.
The function first shifts
`x` by its mean, then adds padding to remove aliasing and computes its FFT. Equation {% katex %}(14){% endkatex %} is
next used to compute the Discrete Fourier Transform of the autocorrelation which is inverted. The final result is
normalized by the zero lag autocorrelation which equals {% katex %}\sigma_f^2{% endkatex %}.

### AR(1) Equilibrium Autocorrelation

The equilibrium properties of the AR(1) random process are discussed in some detail in
[Continuous State Markov Chain Equilibrium]({{ site.baseurl }}{% link _posts/2018-08-16-continuous_state_markov_chain_equilibrium.md %}).
AR(1) is defined by the difference equation,

{% katex display %}
X_{t} = \alpha X_{t-1} + \varepsilon_{t}\ \ \ \ \ (16),
{% endkatex %}

where {% katex %}t=0,\ 1,\ 2,\ldots{% endkatex %} and the {% katex %}\varepsilon_{t}{% endkatex %} are identically
distributed independent {% katex %}\textbf{Normal}(0,\ \sigma^2){% endkatex %} random variables.

The
equilibrium mean and standard deviation, {% katex %}\mu_E{% endkatex %} and {% katex %}\sigma_E{% endkatex %} are given by,

{% katex display %}
\begin{gathered}
\mu_{E} = 0 \\
\sigma_{E} = \frac{\sigma^2}{1 - \alpha^2}.
\end{gathered}\ \ \ \ \ (17)
{% endkatex %}

The equilibrium autocorrelation with time lag {% katex %}\tau{% endkatex %} is defined by,

{% katex display %}
r_{\tau}^{E} = \lim_{t\to\infty} E\left[X_t X_{t+\tau} \right]
{% endkatex %}

If equation {% katex %}(16){% endkatex %} is used to evaluate a few steps beyond an arbitrary time {% katex %}t{% endkatex %}
it is seen that,

{% katex display %}
\begin{aligned}
X_{t+1} &= \alpha X_t + \varepsilon_{t+1} \\
X_{t+2} &= \alpha X_{t+1} + \varepsilon_{t+2} \\
X_{t+3} &= \alpha X_{t+2} + \varepsilon_{t+3}.
\end{aligned}
{% endkatex %}

Substituting the equation for {% katex %}t+1{% endkatex %} into the equation for {% katex %}t+2{% endkatex %} and that
result into the equation for {% katex %}t+3{% endkatex %} gives,

{% katex display %}
X_{t+3} = \alpha^{3} X_t + \sum_{n=1}^{3}\alpha^{n-1} \varepsilon_{t+n}.
{% endkatex %}

If this procedure is continued for {% katex %}\tau{% endkatex %} steps the following is obtained,

{% katex display %}
X_{t+\tau} = \alpha^{\tau} X_t + \sum_{n=1}^{\tau}\alpha^{n-1} \varepsilon_{t+n}.
{% endkatex %}

It follows that the autocorrelation is given by,

{% katex display %}
\begin{aligned}
r_{\tau} &= E\left[X_t X_{t+\tau} \right] \\
&=E\left[ X_{t}\left( \alpha^{\tau} X_t + \sum_{n=1}^{\tau}\alpha^{n-1} \varepsilon_{t+n} \right) \right] \\
&= E\left[ \alpha^{\tau} X_t^2 + \sum_{n=1}^{\tau}\alpha^{n-1} X_t \varepsilon_{t+n}\right] \\
&= \alpha^{\tau} E\left[X_t^2\right] + \sum_{n=1}^{\tau}\alpha^{n-1} E\left[ X_t \varepsilon_{t+n}\right].
\end{aligned}
{% endkatex %}

To go further the summation in the last step needs to be evaluated. In a
[previous post]({{ site.baseurl }}{% link _posts/2018-08-16-continuous_state_markov_chain_equilibrium.md %}) it was
shown that,

{% katex display %}
X_t = \alpha^t X_{0} + \sum_{i=1}^t \alpha^{t-i} \varepsilon_{i}.
{% endkatex %}

Using this result the summation term becomes,

{% katex display %}
\begin{aligned}
E\left[ X_t \varepsilon_{t+n}\right] &= E\left[ \alpha^t X_{0}\varepsilon_{t+n} + \sum_{i=1}^t \alpha^{t-i} \varepsilon_{i}\varepsilon_{t+n} \right] \\
&= \alpha^{t} X_0 E\left[ \varepsilon_{t+n} \right] + \sum_{i=1}^{t} \alpha^{t-i} E\left[ \varepsilon_{i} \varepsilon_{t+n} \right] \\
&= 0,
\end{aligned}
{% endkatex %}

since the {% katex %}\varepsilon_{t}{% endkatex %} are independent and identically distributed random variables. It follows that,

{% katex display %}
\begin{aligned}
r_{\tau} &= E\left[X_t X_{t+\tau} \right] \\
&= \alpha^{\tau} E\left[X_t^2\right]
\end{aligned}
{% endkatex %}

Evaluation of the equilibrium limit gives,

{% katex display %}
\begin{aligned}
r_{\tau}^{E} &= \lim_{t\to\infty} E\left[X_t X_{t+\tau} \right] \\
&= \lim_{t\to\infty} \alpha^{\tau} E\left[X_t^2\right] \\
&= \alpha^{\tau} \sigma_{E}^{2}.
\end{aligned}
{% endkatex %}

The last step follows from {% katex %}(17){% endkatex %} by assuming that
{% katex %}\mid\alpha\mid\ < 1{% endkatex %} so that {% katex %}\mu_{E}{% endkatex %} and
{% katex %}\sigma_{E}{% endkatex %}
are finite. A simple form of the autocorrelation coefficient in the equilibrium limit
follows from equation {% katex %}(15){% endkatex %},

{% katex display %}
\begin{aligned}
\gamma_{\tau}^{E} &= \frac{r_{\tau}}{\sigma^2_E} \\
&= \alpha^{\tau}
\end{aligned}\ \ \ \ \ (18).
{% endkatex %}

{% katex %}\gamma_{\tau}^{E}{% endkatex %}
remains finite for increasing {% katex %}\tau{% endkatex %} only for {% katex %}\mid\alpha\mid\ \leq\ 1{% endkatex %}.

### AR(1) Simulations

In this section the results obtained for AR(1) equilibrium autocorrelation in the previous section will be compared with
simulations. Below an implementation in Python of an AR(1) simulator based on the difference equation representation from
equation {% katex %}(16){% endkatex %} is listed.

```python
def ar_1_series(α, σ, x0=0.0, nsamples=100):
    samples = numpy.zeros(nsamples)
    ε = numpy.random.normal(0.0, σ, nsamples)
    samples[0] = x0
    for i in range(1, nsamples):
        samples[i] = α * samples[i-1] + ε[i]
    return samples
```

The function `ar_1_series(α, σ, x0, nsamples)` takes four arguments: `α` and `σ` from equation {% katex %}(16){% endkatex %},
the initial value of {% katex %}x{% endkatex %}, `x0`, and the number of desired samples, `nsamples`. It begins by allocating storage
for the sample output followed by generation of `nsamples` values of {% katex %}\varepsilon \sim \textbf{Normal}(0,\ \sigma^2){% endkatex %}
with the requested standard deviation, {% katex %}\sigma{% endkatex %}.
The samples are then created using the AR(1 ) difference equation, equation {% katex %}(5){% endkatex %}.

The plots below show examples of simulated time series with {% katex %}\sigma=1{% endkatex %} and values of
{% katex %}\alpha{% endkatex %} satisfying {% katex %}\alpha\ <\ 1{% endkatex %}.
It is seen that for smaller {% katex %}\alpha{% endkatex %} values the series more frequently change direction
and have smaller variance. This is expected from equation {% katex %}(17){% endkatex %}.

<div style="text-align:center;">
  <img class="post-image" src="/assets/posts/discrete_cross_correlation_theorem/ar1_alpha_sample_comparison.png">
</div>

The next series of plots compare the autocorrelation coefficient from equation {% katex %}(18){% endkatex %}, obtained in
the equilibrium limit, with an autocorrelation coefficient calculation using the previously discussed `autocorrelate(x)` function on
the generated sample data. Recall that `autocorrelate(x)` performs a calculation using the Cross Correlation Theorem.
Equation {% katex %}(18){% endkatex %} does a good job of capturing the time scale for the series to become uncorrelated.

<div style="text-align:center;">
  <img class="post-image" src="/assets/posts/discrete_cross_correlation_theorem/ar1_alpha_equilibrium_autocorrelation_comparison.png">
</div>

## Conclusions

The Discrete Cross Correlation Theorem provides a more efficient method of calculating time series
correlations than directly evaluating the sums. For a time series of length N it decreases the cost
of the calculation from {% katex %}O(N^2){% endkatex %} to {% katex %}O(NlogN){% endkatex %} by
use of the Fast Fourier Transform.
An interpretation of the cross correlation as the time lagged covariance of two random variables was presented
and followed by a discussion of the properties of Discrete Fourier Transforms needed to prove the
Cross Correlation Theorem. After building sufficient tools the theorem was derived by application of the
Discrete Fourier Transform to equation {% katex %}(1){% endkatex %}, which defines cross correlation.
An example manual calculation of a Discrete Fourier Transform was performed and compared with a calculation
using the FFT library form `numpy`. Next, manual calculations of cross correlation using a tabular method
to represent the summations were presented and followed by a calculation using the Discrete
Cross Correlation Theorem which illustrated the problem of aliasing. The next example calculation eliminated
aliasing and recovered a result equal to the direct calculation of the summations.
A *dealiased* implementation using `numpy` FFT libraries was then presented. Finally,
the Discrete Cross Correlation Theorem for the special case of the autocorrelation was
discussed and a `numpy` FFT implementation was provided and followed by an example calculation using the AR(1)
random process. In conclusion the autocorrelation coefficient in the equilibrium limit for AR(1) was evaluated
and shown to be finite only for values of the AR(1) parameter that satisfy
{% katex %}\mid\alpha\mid\ < \ 1{% endkatex %}. This result is compared to direct simulations and shown to
provide a good estimate of the decorrelation time of the process.
