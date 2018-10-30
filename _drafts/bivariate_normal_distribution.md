---
title: Bivariate Normal Distribution
key: 20181029
image: /assets/posts/bivariate_normal_distribution/bivariate_pdf_parameterized_contour_sigma_scan.png
author: Troy Stribling
permlink: /bivariate_normal_distribution.html
comments: false
---

The [Bivariate Normal Distribution](https://en.wikipedia.org/wiki/Multivariate_normal_distribution) is a frequently used
multivariable distribution because it provides a simple model of correlation between random variables. It is derived by
application of a linear transformation to a sum of two independent {% katex %}\textbf{Normal}(0,\ 1){% endkatex %}
random variables. Here a derivation is presented that uses a change of variables on the distribution of the independent variable sum.
To provide background a general expression for change of variables of a bivariate integral is discussed. This result is then applied to
obtain the Bivariate Normal Distribution. The first and seconds moments and correlation coefficient are next computed followed by
derivation of the [marginal](https://en.wikipedia.org/wiki/Marginal_distribution) and [conditional](https://en.wikipedia.org/wiki/Conditional_probability_distribution) distributions which are
used in a calculation of the [conditional expectation and variance](https://en.wikipedia.org/wiki/Conditional_expectation).
Finally, the variation in the shape of the distribution as the free parameters are varied is discussed.

<!--more-->

## Bivariate Change of Variables

Consider the PDF of a single variable, {% katex %}f(x){% endkatex %}, and the transformation, {% katex %}y=h(x){% endkatex %} which
is assumed to be invertible and monotonically increasing. The PDF of the transformed variable is given by,

{% katex display %}
g(y) = f(h^{-1}(x)) \frac{dh^{-1}{dx},
{% endkatex %}

where {% katex %}h^{-1}(y){% endkatex %} is the inverse of {% katex %}h(x){% endkatex %}. This result follows from performing a change of variables
on the CDF,

{% katex display %}
\begin{aligned}
P(X\ \leq\ x) &= \int_{-\infty}^{x} f(x) dx \\
&= \int_{-\infty}^{y} f(h^{-1}(x)) \frac{dh^{-1}}{dx} dy \\
&= P(Y\ \leq\ y) \\
&= P(h(Y)\ \leq\ h(x)),
\end{aligned}
{% endkatex %}

where use was made of,

{% katex display %}
dx = \frac{dh^{-1}}{dx} dy.
{% endkatex %}

Next consider the PDF of two variables, {% katex %}f(x,y){% endkatex %}. and the transformation,

{% katex display %}
\begin{aligned}
x &= g(u,v) \\
y &= h(u,v).
\end{aligned}
{% endkatex %}

<div style="text-align:center;">
  <img class="post-image" src="/assets/posts/bivariate_normal_distribution/2DIntegral.png">
</div>

### Area Element as Differential Cross Product

### The Bivariate Jacobian

## Bivariate Normal Distribution

### Sum of Independent {% katex %}\textbf{Normal}(0, 1){% endkatex %} Random Variables

Consider the sums of two independent {% katex %}\textbf{Normal}(0,\ 1){% endkatex %} random variablesm

{% katex display %}
U = X + Y
{% endkatex %}

### Matrix Form of Bivariate Normal Distribution

### Multivariate Normal Distribution

## Bivariate Normal Distribution Properties

### Marginal Distribution

### First and Second Moments and Correlation Coefficient

### Conditional Distribution and Conditional Expectation and variance

### Probability Density Contours

## Conclusions
