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

First consider the PDF of a single variable, {% katex %}f(x){% endkatex %}, and the transformation,
{% katex %}x=x(y){% endkatex %} which is assumed monotonically increasing or decreasing.
The PDF of the transformed variable is given by,

{% katex display %}
g(y) = f(x(y)) \frac{dx}{dy},
{% endkatex %}

This result follows from performing a change of variables on the CDF,

{% katex display %}
\begin{aligned}
P(X\ \leq\ x) &= \int_{-\infty}^{x} f(x) dx \\
&= \int_{-\infty}^{y} f(x(w) \frac{dx}{dw} dy \\
&= P(Y\ \leq\ y)
\end{aligned}
{% endkatex %}

where use was made of,

{% katex display %}
dx = \frac{dh}{dx} dy.
{% endkatex %}

The {% katex %}dh/dx{% endkatex %} term stretches or scales {% katex %}dy{% endkatex %} appropriately. In two
dimensions a similar thing happens that is slightly more complicated. Consider the PDF of two variables,
{% katex %}f(x,y){% endkatex %}, with CDF.

{% katex display %}
P(X \cup Y \in A) = \int_{A} f(x, y) dA,
{% endkatex %}

that defines an integration over a region that computes that probability that {% katex %}X{% endkatex %} and
{% katex %}Y{% endkatex %} are both in the region {% katex %}A{% endkatex %}. The figure below provides an illustration.

<div style="text-align:center;">
  <img class="post-image" src="/assets/posts/bivariate_normal_distribution/2DIntegral.png">
</div>

To go further a geometric result is discussed in the following section relating the [Cross Product](https://en.wikipedia.org/wiki/Cross_product) of two vectors to the area of the parallelogram
defined by the intersection of the vectors is needed.

### Area Element as Differential Cross Product

Consider two vectors {% katex %}\vec{\textbf{A}}{% endkatex %} and {% katex %}\vec{\textbf{B}}{% endkatex %}
separated by an angle {% katex %}\theta{% endkatex %} as shown in the following figure. The cross
product of the two vectors is defined by,

{% katex display %}

{% endkatex %}

<div style="text-align:center;">
  <img class="post-image" src="/assets/posts/bivariate_normal_distribution/CrossProduct.png">
</div>

### The Bivariate Jacobian

{% katex display %}
\begin{aligned}
x &= x(u,v) \\
y &= y(u,v).
\end{aligned}
{% endkatex %}

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
