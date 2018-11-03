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

First, consider the PDF of a single variable, {% katex %}f(x){% endkatex %}, and the transformation,
{% katex %}x=x(y){% endkatex %} which is assumed monotonically increasing.
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
{% katex %}f(x,y){% endkatex %}, with CDF,

{% katex display %}
P(\{X,Y\} \in A) = \int_{A} f(x, y) dA,
{% endkatex %}

that defines an integration over a region that computes that probability that {% katex %}X{% endkatex %} and
{% katex %}Y{% endkatex %} are both in the region {% katex %}A{% endkatex %}. The figure below provides an illustration
for a [Cartesian Coordinate System](https://en.wikipedia.org/wiki/Cartesian_coordinate_system) where
{% katex %}dA=dxdy{% endkatex %}.

<div style="text-align:center;">
  <img class="post-image" src="/assets/posts/bivariate_normal_distribution/2DIntegral.png">
</div>

To go further a geometric result relating the [Cross Product](https://en.wikipedia.org/wiki/Cross_product) of two vectors to the area of the
parallelogram enclosed by the vectors is needed. This topic is discussed in the following section.

### Cross Product as Area

Consider two vectors {% katex %}\textbf{A}{% endkatex %} and {% katex %}\textbf{B}{% endkatex %}
separated by an angle {% katex %}\theta{% endkatex %} and rotated by and angle {% katex %}\gamma{% endkatex %}
as shown in the following figure.

<div style="text-align:center;">
  <img class="post-image" src="/assets/posts/bivariate_normal_distribution/CrossProduct.png">
</div>

The vector components projected along the {% katex %}\textbf{x}{% endkatex %} and
{% katex %}\textbf{y}{% endkatex %} axes are given by,

{% katex display %}
\begin{aligned}
\textbf{A} &= A_{x} \textbf{x} + A_{y} \textbf{y} \\
\textbf{B} &= B_{x} \textbf{x} + B_{y} \textbf{y},
\end{aligned}
{% endkatex %}

where,

{% katex display %}
\begin{aligned}
A_{x} &= \mid \textbf{A} \mid \cos{\gamma} \\
A_{y} &= \mid \textbf{A} \mid \sin{\gamma} \\
B_{x} &= \mid \textbf{B} \mid \cos{\left(\gamma + \theta\right)} \\
B_{y} &= \mid \textbf{B} \mid \sin{\left(\gamma + \theta\right)} \\
\end{aligned}\ \ \ \ \ (1).
{% endkatex %}

The cross product of two vectors is another vector perpendicular both vectors. Here, for the figure
above, this direction is perpendicular to the plane of the page, call it {% katex %}\textbf{z}{% endkatex %}.
Then the cross product is defined by the [determinate](https://en.wikipedia.org/wiki/Determinant) computed
computed from the components of the two vectors and projected along, namely,

{% katex display %}
\begin{aligned}
\textbf{A} \times \textbf{B} &=
\begin{vmatrix}
A_{x} & A_{y} \\
B_{x} & B_{y}
\end{vmatrix}\ \textbf{z} \\
&= \left( A_{x}B_{y} - A_{y} B_{x} \right)\ \textbf{z}
\end{aligned}\ \ \ \ \ (2).
{% endkatex %}

Substituting equation {% katex %}(1){% endkatex %} into {% katex %}(2){% endkatex %} and making use of,

{% katex display %}
\begin{gathered}
\sin{\left(\theta + \gamma\right)} = \sin{\theta}\ \cos{\gamma}\ +\ \cos{\theta}\ \sin{\gamma} \\
\cos{\left(\theta + \gamma\right)} = \cos{\theta}\ \cos{\gamma}\ -\ \sin{\theta}\ \sin{\gamma} \\
\sin^2{\gamma}\ +\ \cos^2{\gamma} = 1
\end{gathered}
{% endkatex %}

results in,

{% katex display %}
\mid \textbf{A} \times \textbf{B} \mid = \mid \textbf{A} \mid \mid \textbf{B} \mid \sin{\theta},
{% endkatex %}

which is the area of the parallelogram indicated by orange in the figure above. {% katex %}sin\theta{% endkatex %}
can be assumed positive since {% katex %}\theta{% endkatex %} can always be chosen such that
{% katex %}0\ \leq\ \theta\ \leq\ \pi{% endkatex %}.

### Bivariate Jacobian

Consider the following coordinate transformation,

{% katex display %}
\begin{aligned}
x &= x(u,v) \\
y &= y(u,v)
\end{aligned}\ \ \ \ \ (3).
{% endkatex %}

applied to the integral,

{% katex display %}
\int_{A} f(x, y) dxdy,
{% endkatex %}

where {% katex %}A{% endkatex %} is an arbitrary area in {% katex %}\left(\textit{x}, \textit{y}\right){% endkatex %} coordinates.
The following figure shows how area elements transforms between the {% katex %}\left(\textit{u}, \textit{v} \right){% endkatex %}
and {% katex %}\left(\textit{x}, \textit{y} \right){% endkatex %} coordinates when transformation {% katex %}(3){% endkatex %} is applied.
The left side of the figure shows the {% katex %}\left(\textit{u}, \textit{v} \right){% endkatex %} [Cartesian Coordinate System](https://en.wikipedia.org/wiki/Cartesian_coordinate_system). In this coordinate system vertical lines of constant
{% katex %}\textit{u}{% endkatex %} are colored orange and horizontal lines of constant {% katex %}\textit{v}{% endkatex %} blue. The right side
of the figure shows the result of application of the transformation between the coordinate systems. The lines of constant {% katex %}\textit{u}{% endkatex %} and
{% katex %}\textit{v}{% endkatex %} can be curved and not aligned with the {% katex %}\left(\textit{x}, \textit{y}\right){% endkatex %}
Cartesian coordinates as shown in the figure. The transformed {% katex %}\left(\textit{u}, \textit{v} \right){% endkatex %} coordinates in
this case are [Curvilinear](https://en.wikipedia.org/wiki/Curvilinear_coordinates).

<div style="text-align:center;">
  <img class="post-image" src="/assets/posts/bivariate_normal_distribution/Jacobian.png">
</div>

The differential area elements are indicated in the figure. In the
{% katex %}\left(\textit{u}, \textit{v} \right){% endkatex %}
Cartesian coordinates the differential area element is given by {% katex %}dA=dudv{% endkatex %} but in the Curvilinear
{% katex %}\left(\textit{x}, \textit{y}\right){% endkatex %} the area element is distorted because the differentials defining
the area are not orthogonal. In the infinitesimal limit it will become a parallelogram with area given by the cross product of
{% katex %}d\textbf{X}{% endkatex %} and {% katex %}d\textbf{Y}{% endkatex %} which are tangent vectors to the curves of constant
{% katex %}\textit{u}{% endkatex %} and {% katex %}\textit{v}{% endkatex %} respectively.
To compute the cross product expressions for the differentials are required. From the transformation defined by equation
{% katex %}(3){% endkatex %} the components of the differentials are computed by assuming that for {% katex %}d\textbf{X}{% endkatex %}
{% katex %}\textit{v}{% endkatex %} is constant and for {% katex %}d\textbf{Y}{% endkatex %} {% katex %}\textit{u}{% endkatex %} is
constant. It follows that,

{% katex display %}
\begin{aligned}
d\textbf{X} &= \frac{\partial x}{\partial u}du\ \textbf{x} + \frac{\partial y}{\partial u}dv\ \textbf{y} \\
d\textbf{Y} &= \frac{\partial x}{\partial v}dv\ \textbf{x} + \frac{\partial y}{\partial v}dv\ \textbf{y} \\
\end{aligned}.
{% endkatex %}


## Bivariate Normal Distribution

### Sum of Independent {% katex %}\textbf{Normal}(0, 1){% endkatex %} Random Variables

Consider the sums of two independent {% katex %}\textbf{Normal}(0,\ 1){% endkatex %} random variables

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
