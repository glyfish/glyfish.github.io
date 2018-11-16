---
title: Bivariate Normal Distribution
key: 20181029
image: /assets/posts/bivariate_normal_distribution/bivariate_pdf_parameterized_contour_correlation_sigma2_scan.png
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
&= \int_{-\infty}^{y} f(x(w)) \frac{dx}{dw} dy \\
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
P(\{X,Y\} \in A) = \int_{A} f(x, y) dA\ \ \ \ \ (1),
{% endkatex %}

that defines an integration over a region that computes that probability that {% katex %}X{% endkatex %} and
{% katex %}Y{% endkatex %} are both in the region {% katex %}A{% endkatex %}. The figure below provides an illustration for a [Cartesian Coordinate System](https://en.wikipedia.org/wiki/Cartesian_coordinate_system)
where {% katex %}dA=dxdy{% endkatex %}.

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
{% katex %}\textbf{y}{% endkatex %} unit vectors is given by,

{% katex display %}
\begin{aligned}
\textbf{A} &= A_{x}\textbf{x} + A_{y}\textbf{y} \\
\textbf{B} &= B_{x}\textbf{x} + B_{y}\textbf{y},
\end{aligned}
{% endkatex %}

where,

{% katex display %}
\begin{aligned}
A_{x} &= \mid \textbf{A} \mid \cos{\gamma} \\
A_{y} &= \mid \textbf{A} \mid \sin{\gamma} \\
B_{x} &= \mid \textbf{B} \mid \cos{\left(\gamma + \theta\right)} \\
B_{y} &= \mid \textbf{B} \mid \sin{\left(\gamma + \theta\right)} \\
\end{aligned}\ \ \ \ \ (2).
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
\end{vmatrix}\textbf{z} \\
&= \left( A_{x}B_{y} - A_{y} B_{x} \right)\textbf{z}
\end{aligned}\ \ \ \ \ (3).
{% endkatex %}

Substituting equation {% katex %}(2){% endkatex %} into {% katex %}(3){% endkatex %} and making use of,

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

which is the area of the parallelogram indicated by orange in the figure above.
{% katex %}sin\theta{% endkatex %} can be assumed positive since
{% katex %}\theta{% endkatex %} can always be chosen such that
{% katex %}0\ \leq\ \theta\ \leq\ \pi{% endkatex %}.

### Bivariate Jacobian

Consider the following coordinate transformation,

{% katex display %}
\begin{aligned}
x &= x(u,v) \\
y &= y(u,v)
\end{aligned}\ \ \ \ \ (4).
{% endkatex %}

applied to the integral,

{% katex display %}
\int_{A} f(x, y) dxdy,
{% endkatex %}

where {% katex %}A{% endkatex %} is an arbitrary area in
{% katex %}\left(\textit{x}, \textit{y}\right){% endkatex %} coordinates. The following figure shows how area elements transforms between the {% katex %}\left(\textit{u}, \textit{v} \right){% endkatex %}
and {% katex %}\left(\textit{x}, \textit{y} \right){% endkatex %} coordinates when
transformation {% katex %}(4){% endkatex %} is applied. The left side of the figure shows the
{% katex %}\left(\textit{u}, \textit{v} \right){% endkatex %}
[Cartesian Coordinate System](https://en.wikipedia.org/wiki/Cartesian_coordinate_system).
In this system vertical lines of constant {% katex %}\textit{u}{% endkatex %}
are colored orange and horizontal lines of constant {% katex %}\textit{v}{% endkatex %} blue. The right side
of the figure shows the result of application of the transformation between the coordinate systems.
The lines of constant {% katex %}\textit{u}{% endkatex %} and {% katex %}\textit{v}{% endkatex %}
can be curved and not aligned with the {% katex %}\left(\textit{x}, \textit{y}\right){% endkatex %}
Cartesian coordinates as shown in the figure. The transformed
{% katex %}\left(\textit{u}, \textit{v} \right){% endkatex %} coordinates in this case are [Curvilinear](https://en.wikipedia.org/wiki/Curvilinear_coordinates).

<div style="text-align:center;">
  <img class="post-image" src="/assets/posts/bivariate_normal_distribution/Jacobian.png">
</div>

Consider the differential area elements indicated in the figure. In the
{% katex %}\left(\textit{u}, \textit{v} \right){% endkatex %}
Cartesian coordinates the differential area element is given by {% katex %}dA=dudv{% endkatex %} but
in the Curvilinear {% katex %}\left(\textit{x}, \textit{y}\right){% endkatex %} the area element is
distorted because the differentials defining the area are not orthogonal. In the infinitesimal limit it
will become a parallelogram with area given by the cross product of {% katex %}d\textbf{X}{% endkatex %}
and {% katex %}d\textbf{Y}{% endkatex %} which are tangent vectors to the curves of constant
{% katex %}\textit{v}{% endkatex %} and {% katex %}\textit{u}{% endkatex %} respectively.
To compute the cross product expressions for the differentials are required. From the transformation
defined by equation {% katex %}(4){% endkatex %} the components of the differentials are computed by
assuming {% katex %}\textit{v}{% endkatex %} is constant for {% katex %}d\textbf{X}{% endkatex %} and
{% katex %}\textit{u}{% endkatex %} is constant for {% katex %}d\textbf{Y}{% endkatex %}.
It follows that,

{% katex display %}
\begin{aligned}
d\textbf{X} &= \frac{\partial x}{\partial u}du\textbf{x} + \frac{\partial y}{\partial u}du\textbf{y} \\
d\textbf{Y} &= \frac{\partial x}{\partial v}dv\textbf{x} + \frac{\partial y}{\partial v}dv\textbf{y}
\end{aligned}.
{% endkatex %}

Now the cross product of the differentials above is given by,

{% katex display %}
\begin{aligned}
d\textbf{X} \times d\textbf{Y}
&= \begin{vmatrix}
\frac{\small{\partial x}}{\small{\partial u}}\small{du} &
\frac{\small{\partial y}}{\small{\partial u}}\small{du} \\
\frac{\small{\partial x}}{\small{\partial v}}\small{dv} &
\frac{\small{\partial y}}{\small{\partial v}}\small{dv}
\end{vmatrix}\textbf{z} \\
&= \begin{vmatrix}
\frac{\small{\partial x}}{\small{\partial u}} &
\frac{\small{\partial y}}{\small{\partial u}} \\
\frac{\small{\partial x}}{\small{\partial v}} &
\frac{\small{\partial y}}{\small{\partial v}}
\end{vmatrix}dudv\textbf{z} \\
&= \left(\frac{\partial x}{\partial u}\frac{\partial y}{\partial v} - \frac{\partial y}{\partial u}\frac{\partial x}{\partial v} \right)dudv\textbf{z}.
\end{aligned}
{% endkatex %}

The bivariate [Jacobian](https://en.wikipedia.org/wiki/Jacobian_matrix_and_determinant) is defined by,

{% katex display %}
\begin{aligned}
\mid J \mid &= \left| \frac{\partial\left(x, y\right)}{\partial\left(u, v\right)} \right| \\
&= \begin{vmatrix}
\frac{\small{\partial x}}{\small{\partial u}} &
\frac{\small{\partial x}}{\small{\partial v}} \\
\frac{\small{\partial y}}{\small{\partial u}} &
\frac{\small{\partial y}}{\small{\partial v}}
\end{vmatrix} \\
&= \left(\frac{\partial x}{\partial u}\frac{\partial y}{\partial v} - \frac{\partial y}{\partial u}\frac{\partial x}{\partial v} \right).
\end{aligned}
{% endkatex %}

In general the determinate of a matrix equals the determinate of the transpose of the matrix. It follows
that the area element in {% katex %}\left(\textit{u}, \textit{v} \right){% endkatex %}
coordinates is given by,

{% katex display %}
dA = \mid d\textbf{X} \times d\textbf{Y} \mid = \mid J \mid dudv.
{% endkatex %}

{% katex %}\mid J \mid{% endkatex %} will scale the differential area {% katex %}dudv{% endkatex %}
by the amount required by the transform in a manner similar to that seen for a single variable.

Finally the transform of the bivariate integral in equation {% katex %}(1){% endkatex %} is given by,

{% katex display %}
\int_{A} f(x, y) dxdy = \int_{A'} f(x(u,v), y(u, v)) \mid J \mid dudv,
{% endkatex %}

where {% katex %}A'{% endkatex %} is the region obtained by applying transform {% katex %}(4){% endkatex %}
to {% katex %}A{% endkatex %}.

## Bivariate Normal Distribution

The Bivariate Normal Distribution with random variables {% katex %}U{% endkatex %} and
{% katex %}V{% endkatex %} is obtained by applying the following linear transformation to
two independent {% katex %}\textbf{Normal}(0, 1){% endkatex %} random variables
{% katex %}X{% endkatex %}
and {% katex %}Y,{% endkatex %}

{% katex display %}
\begin{pmatrix}
U - \mu_{u} \\
V - \mu_{v}
\end{pmatrix} =
\begin{pmatrix}
\sigma_{u} & 0 \\
\sigma_{v}\gamma & \sigma_{v}\sqrt{1 - \gamma^{2}}
\end{pmatrix}
\begin{pmatrix}
X \\
Y
\end{pmatrix},\ \ \ \ \ (5)
{% endkatex %}

where {% katex %}\mu_{u}{% endkatex %}, {% katex %}\mu_{v}{% endkatex %},
{% katex %}\sigma_{u}{% endkatex %}, {% katex %}\sigma_{v}{% endkatex %} and
{% katex %}\gamma{% endkatex %} are scalar constants named in anticipation of
being the mean, standard deviation and correlation coefficient
of the distribution.

An equation for {% katex %}X{% endkatex %} and {% katex %}Y{% endkatex %} in terms
{% katex %}U{% endkatex %}
and {% katex %}V{% endkatex %} is obtained by inverting the transformation matrix,

{% katex display %}
\begin{pmatrix}
X \\
Y
\end{pmatrix} =
\frac{1}{\sigma_{u}\sigma_{v}\sqrt{1 - \gamma^2}}
\begin{pmatrix}
\sigma_{v}\sqrt{1 - \gamma^{2}} & 0 \\
-\sigma_{v}\gamma & \sigma_{u}
\end{pmatrix}
\begin{pmatrix}
U - \mu_{u} \\
V - \mu_{v}
\end{pmatrix},\ \ \ \ (6)
{% endkatex %}

where it is assumed that {% katex %}\gamma^{2} \ne 1.{% endkatex %}
The distribution for {% katex %}X{% endkatex %} and {% katex %}Y{% endkatex %} is found by making use
of the assumption that they are independent {% katex %}\textbf{Normal}(0, 1){% endkatex %}, namely,

{% katex display %}
f(x,y) = \frac{1}{2\pi}e^{-(x^2 + y^2) / 2}\ \ \ \ \ (7).
{% endkatex %}

In the previous section it was shown that if a transformation of the form,

{% katex display %}
\begin{aligned}
x &= x(u, v) \\
y &= y(u, v),
\end{aligned}
{% endkatex %}

is available the transformed density is given by,

{% katex display %}
g(u, v) = \mid J \mid f(x(u,v), y(u,v))\ \ \ \ \ (8),
{% endkatex %}

where {% katex %}\mid J \mid{% endkatex %} is the Jacobian,

{% katex display %}
\mid J \mid = \frac{\partial x}{\partial u}\frac{\partial y}{\partial v} -
  \frac{\partial y}{\partial u}\frac{\partial x}{\partial v}.
{% endkatex %}

The transformations {% katex %}x(u,v){% endkatex %} and {% katex %}y(u,v){% endkatex %} can be determined
from equation {% katex %}(6){% endkatex %},

{% katex display %}
\begin{aligned}
x(u,v) & = \frac{\left(u-\mu_{u} \right)}{\sigma_{u}} \\
y(u,v) & = \frac{1}{\sqrt{1-\gamma^2}}
             \left[
               \frac{1}{\sigma_{v}}\left(v-\mu_{v}\right) -
               \frac{\gamma}{\sigma_{u}}\left(u-\mu_{u}\right)
             \right]
\end{aligned}\ \ \ \ \ (9).
{% endkatex %}

It follows that the Jacobian is given by,

{% katex display %}
\mid J \mid = \frac{1}{\sigma_{u}\sigma_{v} \sqrt{1-\gamma^{2}}}.\ \ \ \ \ (10)
{% endkatex %}

With the goal of keeping things simple in the evaluation of equation {% katex %}(8){% endkatex %}
the exponential argument term of equation {% katex %}(7){% endkatex %},
{% katex %}x^2 + y^2,{% endkatex %} will first be considered. Now, making use of
equations {% katex %}(9){% endkatex %} gives,

{% katex display %}
\begin{aligned}
x^2 + y^2 &= x^2(u,v) + y^2(u,v) \\
&= \frac{\left(u-\mu_{u} \right)^2}{\sigma_{u}^2} +
   \frac{1}{1-\gamma^2}\left[\frac{1}{\sigma_{v}}\left(v-\mu_{v}\right) -
               \frac{\gamma}{\sigma_{u}}\left(u-\mu_{u}\right)\right]^2 \\
&= \frac{\left(u-\mu_{u} \right)^2}{\sigma_{u}^2} +
   \frac{\gamma^2}{1-\gamma^2}\frac{\left(u - \mu_u \right)^2}{\sigma_u^2} +
   \frac{1}{1-\gamma^2}\frac{\left(v-\mu_v\right)^2}{\sigma_v^2} -
   \frac{2\gamma}{1-\gamma^2}\frac{\left(u - \mu_u \right)\left(v-\mu_v\right)}{\sigma_u\sigma_v} \\
&= \frac{1}{1-\gamma^2}   
   \left[
      \frac{\left(u-\mu_{u} \right)^2}{\sigma_{u}^2}\left(1-\gamma^2+\gamma^2\right)
      \frac{\left(v-\mu_v\right)^2}{\sigma_{v}^2} -
      2\gamma\frac{\left(v-\mu_v\right)}{\sigma_{v}}\frac{\left(u-\mu_{u} \right)}{\sigma_{u}}
   \right] \\
&= \frac{1}{1-\gamma^2}
   \left[
      \frac{\left(u-\mu_{u} \right)^2}{\sigma_{u}^2} +
      \frac{\left(v-\mu_v\right)^2}{\sigma_v^2} -
      2\gamma\frac{\left(v-\mu_v\right)}{\sigma_{v}}\frac{\left(u-\mu_{u} \right)}{\sigma_{u}}
   \right],
\end{aligned}\ \ \ \ \ (11)
{% endkatex %}

where the first step expands the right most squared term and the last two steps aggregate terms with
the same posers of {% katex %}\left(u-\mu_{u} \right){% endkatex %} and
{% katex %}\left(v-\mu_v\right){% endkatex %}. Making use of equations {% katex %}(10){% endkatex %} and
{% katex %}(11){% endkatex %} the Bivariate Normal Distribution PDF defined by equation
{% katex %}(8){% endkatex %} gives,

{% katex display %}
\begin{aligned}
g(u, v) &= \mid J \mid f(x(u,v), y(u,v)) \\
&= \frac{1}{2\pi\sigma_{u}\sigma_{v} \sqrt{1-\gamma^{2}}}e^{
  \frac{\footnotesize{-1}}{\footnotesize{2(1-\gamma^2)}}
     \left[
        \frac{\footnotesize{\left(u-\mu_{u} \right)^2}}{\footnotesize{\sigma_{u}^2}} +
        \frac{\footnotesize{\left(v-\mu_v\right)^2}}{\footnotesize{\sigma_v^2}} -
        2\gamma\frac{\footnotesize{\left(v-\mu_v\right)}}{\footnotesize{\sigma_{v}}}\frac{\footnotesize{\left(u-\mu_{u} \right)}}{\footnotesize{\sigma_{u}}}
     \right]
}
\end{aligned}\ \ \ \ \ (12)
{% endkatex %}

With this form of the Bivariate Normal Distribution PDF extensions to higher dimensions is not clear. In the
next section a form us described that makes this clear.

### Matrix Form of Bivariate Normal Distribution

A matrix form for the Bivariate Normal Distribution PDF can be constructed that scales
to higher dimensions. Here the version for two variables is compared with the results of the
previous section followed by a discussion of extension to an arbitrary number of dimensions.
To begin consider the column vectors,

{% katex display %}
\begin{gathered}
Y = \begin{pmatrix}
     y_1 \\
     y_2
    \end{pmatrix}
\mu = \begin{pmatrix}
      \mu_1 \\
      \mu_2
      \end{pmatrix} \\
P = \begin{pmatrix}
      \sigma_{1}^{2} & \gamma\sigma_{1}\sigma_{2} \\
      \gamma\sigma_{1}\sigma_{2} & \sigma_{2}^{2}
      \end{pmatrix} \\
\end{gathered}\ \ \ \ \ (13)
{% endkatex %}

Here {% katex %}Y{% endkatex %} is the column vector of Bivariate Normal Random variables,
{% katex %}\mu{% endkatex %} the column vector of mean values and {% katex %}P{% endkatex %} is called the
[Covariance Matrix](https://en.wikipedia.org/wiki/Covariance_matrix). To continue the inverse of the
covariance matrix and determinate are required. First, the inverse is given by,

{% katex display %}
P^{-1} = \frac{1}{\sigma_1^2\sigma_2^2\left(1 - \gamma^2 \right)}
\begin{pmatrix}
   \sigma_{2}^{2} & -\gamma\sigma_{1}\sigma_{2} \\
   -\gamma\sigma_{1}\sigma_{2} & \sigma_{1}^{2}
\end{pmatrix},
{% endkatex %}

and the determinate is,

{% katex display %}
\mid P \mid = \sigma_1^2\sigma_2^2\left(1 - \gamma^2 \right)
{% endkatex %}

Now consider the product where {% katex %}\left( Y - \mu\right)^{T}{% endkatex %} is the
[transpose](https://en.wikipedia.org/wiki/Transpose) of {% katex %}\left( Y - \mu\right){% endkatex %},

{% katex display %}
\left( Y - \mu\right)^{T}P^{-1}\left(Y-\mu\right).
{% endkatex %}

The two rightmost terms evaluate to,

{% katex display %}
\begin{aligned}
P^{-1}\left(Y-\mu\right) &=
   \frac{1}{\sigma_1^2\sigma_2^2\left(1 - \gamma^2 \right)}
   \begin{pmatrix}
      \sigma_{2}^{2} & -\gamma\sigma_{1}\sigma_{2} \\
      -\gamma\sigma_{1}\sigma_{2} & \sigma_{1}^{2}
   \end{pmatrix}
   \begin{pmatrix}
       y_1 -\mu_1 \\
       y_2 -\mu_2
   \end{pmatrix} \\
&= \frac{1}{\sigma_1^2\sigma_2^2\left(1 - \gamma^2 \right)}
   \begin{pmatrix}
      \sigma_{2}^{2}\left(y_1 -\mu_1\right) - \gamma\sigma_{1}\sigma_{2}\left(y_2 -\mu_2\right) \\
      -\gamma\sigma_{1}\sigma_{2}\left(y_1 -\mu_1\right) +  \sigma_{1}^{2}\left(y_2 -\mu_2\right)
  \end{pmatrix}.
\end{aligned}
{% endkatex %}

Continuing the evaluation by including the left most term gives,

{% katex display %}
\begin{aligned}
\left( Y - \mu\right)^{T}P^{-1}\left(Y-\mu\right) &=
\frac{1}{\sigma_1^2\sigma_2^2\left(1 - \gamma^2 \right)}
\begin{pmatrix}
    y_1 -\mu_1 & y_2 -\mu_2
\end{pmatrix}
\begin{pmatrix}
   \sigma_{2}^{2}\left(y_1 -\mu_1\right) - \gamma\sigma_{1}\sigma_{2}\left(y_2 -\mu_2\right) \\
   -\gamma\sigma_{1}\sigma_{2}\left(y_1 -\mu_1\right) + \sigma_{1}^{2}\left(y_2 -\mu_2\right)
\end{pmatrix} \\
&= \frac{1}{\sigma_1^2\sigma_2^2\left(1 - \gamma^2 \right)}
      \left\{
         \left[\sigma_{2}^{2}\left(y_1 -\mu_{1}\right) - \gamma\sigma_{1}\sigma_{2}\left(y_{2} -\mu_{2}\right)\right]
         \left(y_{1}-\mu_{1} \right) +
         \left[\sigma_{1}^{2}\left(y_{2} -\mu_{2}\right) -\gamma\sigma_{1}\sigma_{2}\left(y_{1} -\mu_{1}\right)\right]
        \left(y_{2}-\mu_{2} \right)
      \right\} \\
&= \frac{1}{\sigma_1^2\sigma_2^2\left(1 - \gamma^2 \right)}
      \left[
         \sigma_{2}^{2}\left(y_{1} -\mu_{1}\right)^{2} +
         \sigma_{1}^{2}\left(y_{2} -\mu_{2}\right)^{2} -
         2\gamma\sigma_{1}\sigma_{2}\left(y_{2} -\mu_{2}\right)\left(y_{1}-\mu_{1} \right)
      \right] \\
&= \frac{1}{1-\gamma^2}
   \left[
      \frac{\left(y_{1}-\mu_{1} \right)^2}{\sigma_{1}^2} +
      \frac{\left(y_{2}-\mu_{2}\right)^2}{\sigma_{2}^2} -
      2\gamma\frac{\left(y_{1}-\mu_{1}\right)}{\sigma_{1}}\frac{\left(y_{2}-\mu_{2}\right)}{\sigma_{2}}
   \right],
\end{aligned}
{% endkatex %}

Which is the same as obtained using transform {% katex %}(9){% endkatex %} in the previous section. Comparing
the expression for {% katex %}\mid P\mid{% endkatex %} with equation {% katex %}(12){% endkatex %} gives,

{% katex display %}
\mid J \mid = \frac{1}{\sqrt{\mid P \mid}}.
{% endkatex %}

Putting these results together produces the desired result,

{% katex display %}
\begin{aligned}
g(u, v) &= \mid J \mid f(x(u,v), y(u,v)) \\
&= \frac{1}{2\pi\sqrt{\mid P \mid}} e^{
  \left[-\frac{1}{2}\footnotesize{\left( Y - \mu\right)^{T}P^{-1}\left(Y-\mu\right)}\right]
}.
\end{aligned}
{% endkatex %}

To extend to an arbitrary number of dimensions, {% katex %}n{% endkatex %}, the covariance matrix
defined in equation {% katex %}(13){% endkatex %} needs to written in a more general form,

{% katex display %}
P_{ij} = \text{Cov}(Y_{i}, Y_{j}).
{% endkatex %}

and the Multivariate Normal PDF becomes,

{% katex display %}
\begin{aligned}
g(u, v) &= \mid J \mid f(x(u,v), y(u,v)) \\
&= \frac{1}{(2\pi)^{n/2}\sqrt{\mid P \mid}} e^{
  \left[-\frac{1}{2}\footnotesize{\left( Y - \mu\right)^{T}P^{-1}\left(Y-\mu\right)}\right]
}
\end{aligned}.
{% endkatex %}

To determine the linear transformation, analogous to equation {% katex %}(9){% endkatex %},
{% katex %}P_{ij}{% endkatex %} is factored using
[Cholesky Decomposition](https://en.wikipedia.org/wiki/Cholesky_decomposition).

In a following section it will be shown that for two variables,

{% katex display %}
\text{Cov}(Y_{i}, Y_{j}) =
\begin{cases}
\gamma\sigma_{i}\sigma_{j} & i\ \neq\ j \\
\sigma_{i}^{2} & i = j
\end{cases}.
{% endkatex %}

which is equivalent to equation {% katex %}(13){% endkatex %}.

## Bivariate Normal Distribution Properties

The previous sections discussed the derivation of the Bivariate Normal random variables as
[linear combinations](https://en.wikipedia.org/wiki/Linear_combination) of independent unit normal
random variables. Linear combinations were constructed by application of a linear transformation
that includes five independent parameters. In this section interpretations of the parameters are
provided and variation in the distribution as the parameters are varied is described. First, the
[Marginal Distributions](https://en.wikipedia.org/wiki/Marginal_distribution) are calculated and it is
shown that four of the distribution parameters correspond to the means and standard deviations.
Next, the correlation coefficient of the distribution random variables is computed and shown to
correspond to the remaining parameter. The remaining sections consider how changes in the
parameters affect the distribution shape. This includes an analysis of PDF contours and the
linear transformation used to construct the distribution from the independent
{% katex %}\textbf{Normal}(0,1){% endkatex %} random variables.

### Marginal Distributions

The Marginal Distributions for Bivariate Normal random variables {% katex %}u{% endkatex %} and {% katex %}v{% endkatex %}
are defined by,

{% katex display %}
\begin{aligned}
g(u) &= \int_{-\infty}^{\infty} g(u, v) dv \\
g(v) &= \int_{-\infty}^{\infty} g(u, v) du,
\end{aligned}
{% endkatex %}

where {% katex %}g(u,v){% endkatex %} is defined by equation {% katex %}(12).{% endkatex %} First,
consider evaluation of the integral for {% katex %}g(u),{% endkatex %}

{% katex display %}
\begin{aligned}
g(u) &= \int_{-\infty}^{\infty} g(u, v) dv \\
&= \frac{1}{2\pi\sigma_{u}\sigma_{v} \sqrt{1-\gamma^{2}}}\int_{-\infty}^{\infty}e^{
  -\frac{\footnotesize{1}}{\footnotesize{2(1-\gamma^2)}}
     \left[
        \frac{\footnotesize{\left(u-\mu_{u} \right)^2}}{\footnotesize{\sigma_{u}^2}} +
        \frac{\footnotesize{\left(v-\mu_v\right)^2}}{\footnotesize{\sigma_v^2}} -
        2\gamma\frac{\footnotesize{\left(v-\mu_v\right)}}{\footnotesize{\sigma_{v}}}\frac{\footnotesize{\left(u-\mu_{u} \right)}}{\footnotesize{\sigma_{u}}}
     \right]
}dv\\
&= \frac{1}{2\pi\sigma_{u}\sigma_{v} \sqrt{1-\gamma^{2}}}e^{
     \frac{\footnotesize{-1}}{\footnotesize{2(1-\gamma^2)}}
     \frac{\footnotesize{\left(u-\mu_{u} \right)^2}}{\footnotesize{\sigma_{u}^2}}
  }
  \int_{-\infty}^{\infty}e^{
  -\frac{\footnotesize{1}}{\footnotesize{2(1-\gamma^2)}}
     \left[
        \frac{\footnotesize{\left(v-\mu_{v} \right)^2}}{\footnotesize{\sigma_{v}^2}} -
        2\gamma\frac{\footnotesize{\left(v-\mu_v\right)}}{\footnotesize{\sigma_{v}}}\frac{\footnotesize{\left(u-\mu_{u} \right)}}{\footnotesize{\sigma_{u}}}
     \right]
}dv\\
&= \frac{1}{2\pi\sigma_{u}\sigma_{v} \sqrt{1-\gamma^{2}}}e^{
     \frac{\footnotesize{-1}}{\footnotesize{2(1-\gamma^2)}}
     \frac{\footnotesize{\left(u-\mu_{u} \right)^2}}{\footnotesize{\sigma_{u}^2}}
  }
  \int_{-\infty}^{\infty}e^{
  -\frac{\footnotesize{1}}{\footnotesize{2(1-\gamma^2)}}
     \left\{
        \left[
           \frac{\footnotesize{\left(v-\mu_{v} \right)}}{\footnotesize{\sigma_{v}}} -
           \gamma\frac{\footnotesize{\left(u-\mu_{u} \right)}}{\footnotesize{\sigma_{u}}}
        \right]^{2}
        -\gamma^{2}\frac{\footnotesize{\left(u-\mu_u\right)^2}}{\footnotesize{\sigma_u^2}}
     \right\}
}dv\\
&= \frac{1}{2\pi\sigma_{u}\sigma_{v} \sqrt{1-\gamma^{2}}}e^{
     -\frac{\footnotesize{1}}{\footnotesize{2(1-\gamma^2)}}
     \frac{\footnotesize{\left(u-\mu_{u} \right)^2}}{\footnotesize{\sigma_{u}^2}}
  }
  e^{
    \frac{\footnotesize{\gamma}}{\footnotesize{2(1-\gamma^2)}}
    \frac{\footnotesize{\left(u-\mu_{u} \right)^2}}{\footnotesize{\sigma_{u}^2}}
  }
  \int_{-\infty}^{\infty}e^{
  \frac{\footnotesize{-1}}{\footnotesize{2(1-\gamma^2)}}
    \left[
       \frac{\footnotesize{\left(v-\mu_{v} \right)}}{\footnotesize{\sigma_{v}}} -
       \gamma\frac{\footnotesize{\left(u-\mu_{u} \right)}}{\footnotesize{\sigma_{u}}}
    \right]^{2}
}dv\\
&= \frac{1}{2\pi\sigma_{u}\sigma_{v} \sqrt{1-\gamma^{2}}}e^{
     -\frac{\footnotesize{\left(u-\mu_{u} \right)^2}}{\footnotesize{2\sigma_{u}^2}}
  }
  \sqrt{\footnotesize{2\pi\sigma_{v}^{2}(1-\gamma^2)}}\\
&= \frac{1}{\sqrt{2\pi\sigma_{u}^{2}}}e^{
     -\frac{\footnotesize{\left(u-\mu_{u} \right)^2}}{\footnotesize{2\sigma_{u}^2}}
  }.
\end{aligned}
{% endkatex %}

In the first step the {% katex %}u{% endkatex %} dependence is factored out for
evaluation of the integral over {% katex %}v{% endkatex %}. The next step completes the
square of the exponential followed by also factoring the introduced {% katex %}u{% endkatex %}
term outside the {% katex %}v{% endkatex %} integral, which is followed by simplification of the
{% katex %}u{% endkatex %} exponential argument. Finally, the integral over
{% katex %}v{% endkatex %} is evaluated yielding a
{% katex %}\textbf{Normal}(\mu_{u}, \sigma_{u}){% endkatex %} distribution. Similarly,
the marginal distribution {% katex %}g(v){% endkatex %} is given by,

{% katex display %}
g(v) = \int_{-\infty}^{\infty} g(u, v) du
= \frac{1}{\sqrt{2\pi\sigma_{v}^{2}}}e^{
     -\frac{\footnotesize{\left(v-\mu_{v} \right)^2}}{\footnotesize{2\sigma_{v}^2}}
  },\ \ \ \ \ (19)
{% endkatex %}

which is a {% katex %}\textbf{Normal}(\mu_{v}, \sigma_{v}){% endkatex %} distribution. The
plot below shows examples of {% katex %}\textbf{Normal}(\mu, \sigma){% endkatex %} as
{% katex %}\mu{% endkatex %} and {% katex %}\sigma{% endkatex %} are varied. The effect of
{% katex %}\mu{% endkatex %} is to translate the distribution along the {% katex %}u{% endkatex %}
axis and {% katex %}\sigma{% endkatex %} scales the distribution width.

<div style="text-align:center;">
  <img class="post-image" src="/assets/posts/bivariate_normal_distribution/normal_distribution_parameters.png">
</div>

The mean and variance are now readily determined for both {% katex %}u{% endkatex %} and
 {% katex %}v{% endkatex %},

{% katex display %}
\begin{aligned}
\text{E}[U] &= \int_{-\infty}^{\infty}\int_{-\infty}^{\infty} ug(u, v) dvdu \\
&= \int_{-\infty}^{\infty}u\int_{-\infty}^{\infty} g(u, v) dvdu \\
&= \int_{-\infty}^{\infty}ug(u) dvdu \\
&= \mu_{u},
\end{aligned}
{% endkatex %}

and,

{% katex display %}
\begin{aligned}
\text{Var}[U] &= \int_{-\infty}^{\infty}\int_{-\infty}^{\infty} (u -\mu_{u})^2 g(u, v) dvdu \\
&= \int_{-\infty}^{\infty}(u -\mu_{u})^2 \int_{-\infty}^{\infty} g(u, v) dvdu \\
&= \int_{-\infty}^{\infty}(u -\mu_{u})^2g(u) dvdu \\
&= \sigma_{u}^{2}.
\end{aligned}
{% endkatex %}

Similarly for {% katex %}v{% endkatex %},

{% katex display %}
\begin{aligned}
\text{E}[V] &= \int_{-\infty}^{\infty}\int_{-\infty}^{\infty} vg(u, v) dudv = \mu_{v} \\
\text{Var}[V] &= \int_{-\infty}^{\infty}\int_{-\infty}^{\infty} (v -\mu_{v})^2 g(u, v) dudv = \sigma_{v}^{2}.
\end{aligned}
{% endkatex %}

### Conditional Distribution

The [Conditional Distributions](https://en.wikipedia.org/wiki/Conditional_probability_distribution)
for {% katex %}u{% endkatex %} and {% katex %}v{% endkatex %} are
defined by,

{% katex display %}
\begin{aligned}
g(u|v) &= \frac{g(u, v)}{g(v)} \\
g(v|u) &= \frac{g(u, v)}{g(u)}
\end{aligned}
{% endkatex %}

Using equations {% katex %}(12){% endkatex %} and {% katex %}(19){% endkatex %}
{% katex %}g(u|v){% endkatex %} is evaluated as follows,

{% katex display %}
\begin{aligned}
g(u|v) &= \frac{g(u, v)}{g(v)} \\
&= \frac{1}{2\pi\sigma_{u}\sigma_{v} \sqrt{1-\gamma^{2}}}e^{
  \frac{\footnotesize{-1}}{\footnotesize{2(1-\gamma^2)}}
     \left[
        \frac{\footnotesize{\left(u-\mu_{u} \right)^2}}{\footnotesize{\sigma_{u}^2}} +
        \frac{\footnotesize{\left(v-\mu_v\right)^2}}{\footnotesize{\sigma_v^2}} -
        2\gamma\frac{\footnotesize{\left(v-\mu_v\right)}}{\footnotesize{\sigma_{v}}}\frac{\footnotesize{\left(u-\mu_{u} \right)}}{\footnotesize{\sigma_{u}}}
     \right]
}
\sqrt{2\pi\sigma_{v}^{2}}e^{
     \frac{\footnotesize{\left(v-\mu_{v} \right)^2}}{\footnotesize{2\sigma_{v}^2}}
  }\\
&= \frac{1}{\sigma_{u}\sqrt{2\pi\left(1-\gamma^{2}\right)}}e^{
  \frac{\footnotesize{-1}}{\footnotesize{2(1-\gamma^2)}}
     \left[
        \frac{\footnotesize{\left(u-\mu_{u} \right)^2}}{\footnotesize{\sigma_{u}^2}} -
        2\gamma\frac{\footnotesize{\left(v-\mu_v\right)}}{\footnotesize{\sigma_{v}}}\frac{\footnotesize{\left(u-\mu_{u} \right)}}{\footnotesize{\sigma_{u}}}
     \right]
}
e^{
     \left\{
        \frac{\footnotesize{\left(v-\mu_{v} \right)^2}}{\footnotesize{2\sigma_{v}^2}} +
        \frac{\footnotesize{1}}{\footnotesize{2(1-\gamma^2)}}
        \left[
           \frac{\footnotesize{\left(v-\mu_v\right)^2}}{\footnotesize{\sigma_v^2}}
        \right]
      \right\}
  }\\
&= \frac{1}{\sigma_{u}\sqrt{2\pi\left(1-\gamma^{2}\right)}}e^{
  \frac{\footnotesize{-1}}{\footnotesize{2(1-\gamma^2)}}
     \left\{
        \frac{1}{\footnotesize{\sigma_{u}}}
        \left[
           u - \left(\mu_u + \frac{\footnotesize{\gamma\sigma_u}}{\footnotesize{\sigma_v}} \left(\footnotesize{v-\mu_{v}}\right)\right)
         \right]^2 -
          \frac{\footnotesize{\gamma^2\sigma_v^2}}{\footnotesize{\sigma_v^2}}  \left(\footnotesize{v-\mu_{v}})\right)^2
     \right\}
}
e^{
     \left\{
        \frac{\footnotesize{\left(v-\mu_{v} \right)^2}}{\footnotesize{2\sigma_{v}^2}} -
        \frac{\footnotesize{1}}{\footnotesize{2(1-\gamma^2)}}
        \left[
           \frac{\footnotesize{\left(v-\mu_v\right)}}{\footnotesize{\sigma_v^2}}
        \right]
      \right\}
  }\\
&= \frac{1}{\sigma_{u}\sqrt{2\pi\left(1-\gamma^{2}\right)}}e^{
  \frac{\footnotesize{-1}}{\footnotesize{2(1-\gamma^2)}}
     \left\{
        \frac{1}{\footnotesize{\sigma_{u}}}
        \left[
           u - \left(\mu_u + \frac{\footnotesize{\gamma\sigma_u}}{\footnotesize{\sigma_v}} \left(\footnotesize{v-\mu_{v}}\right)\right)
         \right]^2
     \right\}
}
e^{
     \left\{
        \frac{\footnotesize{\left(v-\mu_{v} \right)^2}}{\footnotesize{2\sigma_{v}^2}}
        \left[
             \footnotesize{1} - \frac{\footnotesize{1-\gamma^2}}{\footnotesize{1-\gamma^2}}
        \right]
      \right\}
  }\\
  &= \frac{1}{\sigma_{u}\sqrt{2\pi\left(1-\gamma^{2}\right)}}e^{
    \frac{\footnotesize{-1}}{\footnotesize{2\sigma_{u}^{2}(1-\gamma^2)}}
        \left\{
           u - \left[\mu_u + \frac{\footnotesize{\gamma\sigma_u}}{\footnotesize{\sigma_v}} \left(\footnotesize{v-\mu_{v}}\right)\right]
        \right\}^2
  },
\end{aligned}\ \ \ \ \ (23)
{% endkatex %}

In the first step the {% katex %}\left(v-\mu_{v}\right)^2{% endkatex %} terms are collected and
then the square is completed for the remaining terms. Once again the
{% katex %}\left(v-\mu_{v}\right)^2{% endkatex %} terms are collected and the finial result obtained,
a {% katex %}\textbf{Normal}{% endkatex %} distribution.

Similarly, {% katex %}g(v|U){% endkatex %} is given by,

{% katex display %}
\begin{aligned}
g(v|u) &= \frac{g(u, v)}{g(u)} \\
&= \frac{1}{\sigma_{v}\sqrt{2\pi\left(1-\gamma^{2}\right)}}e^{
  \frac{\footnotesize{-1}}{\footnotesize{2\sigma_{u}^{2}(1-\gamma^2)}}
      \left\{
         v - \left[\mu_v + \frac{\footnotesize{\gamma\sigma_v}}{\footnotesize{\sigma_u}} \left(\footnotesize{u-\mu_{u}}\right)\right]
      \right\}^2
}.
\end{aligned}
{% endkatex %}

The conditional mean and variance can now be easily evaluated,

{% katex display %}
\begin{aligned}
\text{E}[U|V] &= \int_{-\infty}^{\infty} ug(u|v) du \\
&=\left[\mu_u + \frac{\footnotesize{\gamma\sigma_u}}{\footnotesize{\sigma_v}} \left(\footnotesize{v-\mu_{v}}\right)\right],
\end{aligned}
{% endkatex %}

{% katex display %}
\begin{aligned}
\text{E}[V|U] &= \int_{-\infty}^{\infty} vg(v|u) dv \\
&= \left[\mu_v + \frac{\footnotesize{\gamma\sigma_v}}{\footnotesize{\sigma_u}} \left(\footnotesize{u-\mu_{u}}\right)\right],
\end{aligned}
{% endkatex %}

{% katex display %}
\begin{aligned}
\text{Var}[U|V] &= \text{E}[\left(U - \text{E}[U|V]\right)^{2}] \\
&= \int_{-\infty}^{\infty} \left(u - \text{E}[U|V]\right)^{2}g(u|v) du \\
&= \sigma_{u}^{2}(1-\gamma^2),
\end{aligned}
{% endkatex %}

{% katex display %}
\begin{aligned}
\text{Var}[V|U] &= \text{E}[\left(V - \text{E}[V|U]\right)^{2}] \\
&= \int_{-\infty}^{\infty} \left(v - \text{E}[V|U]\right)^{2}g(v|u) dv \\
&=\sigma_{v}^{2}(1-\gamma^2),
\end{aligned}
{% endkatex %}

For both {% katex %}u{% endkatex %} and {% katex %}v{% endkatex %} the conditional expectation
is a linear function of the conditioned variable. This is illustrated in the following plot
where {% katex %}g(u|v){% endkatex %} is plotted for several values of {% katex %}v{% endkatex %}.
It is seen that increasing values of {% katex %}v{% endkatex %} leads to increasing translations
of the distribution.

<div style="text-align:center;">
  <img class="post-image" src="/assets/posts/bivariate_normal_distribution/bivariate_conditional_pdf_y_scan.png">
</div>

Consider the variation of {% katex %}\text{E}[U|V]{% endkatex %},  
{% katex %}\text{E}[V|U]{% endkatex %}, {% katex %}\text{Var}[U|V]{% endkatex %} and {% katex %}\text{Var}[U|V]{% endkatex %} with {% katex %}\gamma{% endkatex %}. For
{% katex %}\gamma = 0{% endkatex %} each reduces to the values corresponding to its
marginal distribution discussed in the previous section. For both conditional distributions
increasing {% katex %}\gamma{% endkatex %} leads to decreasing variance resulting in a
sharper peak. Additionally, changing the sign of {% katex %}\gamma{% endkatex %} results in a
reflection of the distribution about the mean. Each of these properties is illustrated on the
plot below where {% katex %}g(u|v){% endkatex %} is plotted for values of
{% katex %}\gamma{% endkatex %} ranging from {% katex %}-1{% endkatex %} to
{% katex %}1.{% endkatex %}

<div style="text-align:center;">
  <img class="post-image" src="/assets/posts/bivariate_normal_distribution/bivariate_conditional_pdf_correlation_scan.png">
</div>

### Correlation Coefficient

The [Cross Correlation](https://en.wikipedia.org/wiki/Cross-correlation) of the two Bivariate
Normal random variable {% katex %}U{% endkatex %} and {% katex %}V{% endkatex %} is defined
by,

{% katex display %}
\begin{aligned}
\text{E}[UV] &= \int_{-\infty}^{\infty}\int_{-\infty}^{\infty} uv g(u,v) dvdu \\
&= \int_{-\infty}^{\infty}\int_{-\infty}^{\infty} uv g(u|v)g(v) dvdu,
\end{aligned}
{% endkatex %}

the last step follows from the definition of conditional probability. Now, substituting
equations {% katex %}(23){% endkatex %} and {% katex %}(19){% endkatex %} into the equation
above yields,

{% katex display %}
\begin{aligned}
\text{E}[UV] &= \int_{-\infty}^{\infty}\int_{-\infty}^{\infty} uv g(u|v)g(v) dvdu \\
&=\int_{-\infty}^{\infty}\int_{-\infty}^{\infty}uv
\frac{1}{\sigma_{u}\sqrt{2\pi\left(1-\gamma^{2}\right)}}e^{
  \frac{\footnotesize{-1}}{\footnotesize{2\sigma_{u}^{2}(1-\gamma^2)}}
      \left\{
         u - \left[\mu_u + \frac{\footnotesize{\gamma\sigma_u}}{\footnotesize{\sigma_v}} \left(\footnotesize{v-\mu_{v}}\right)\right]
      \right\}^2
}
\frac{1}{\sqrt{2\pi\sigma_{v}^{2}}}e^{
     -\frac{\footnotesize{\left(v-\mu_{v} \right)^2}}{2\footnotesize{\sigma_{v}^2}}
}dvdu \\
&= \frac{1}{\sigma_{u}\sqrt{2\pi\left(1-\gamma^{2}\right)}}
\frac{1}{\sqrt{2\pi\sigma_{v}^{2}}}
\int_{-\infty}^{\infty}ve^{
     -\frac{\footnotesize{\left(v-\mu_{v} \right)^2}}{2\footnotesize{\sigma_{v}^2}}
}
\int_{-\infty}^{\infty}ue^{
  \frac{\footnotesize{-1}}{\footnotesize{2\sigma_{u}^{2}(1-\gamma^2)}}
      \left\{
         u + \left[\mu_u - \frac{\footnotesize{\gamma\sigma_u}}{\footnotesize{\sigma_v}} \left(\footnotesize{v-\mu_{v}}\right)\right]
      \right\}^2
}dudv\\
&= \frac{1}{\sqrt{2\pi\sigma_{v}^{2}}}
\int_{-\infty}^{\infty}ve^{
     -\frac{\footnotesize{\left(v-\mu_{v} \right)^2}}{2\footnotesize{\sigma_{v}^2}}
}
 \left[\mu_u + \frac{\footnotesize{\gamma\sigma_u}}{\footnotesize{\sigma_v}} \left(\footnotesize{v-\mu_{v}}\right)\right] dv\\
 &=\mu_{u}\mu_{v} + \frac{\gamma\sigma_{u}}{\sigma_{v}}\sigma_{v}^{2} \\
 &=\mu_{u}\mu_{v} + \gamma\sigma_{u}\sigma_{v}.
\end{aligned}
{% endkatex %}

Using this result gives the [Covariance](https://en.wikipedia.org/wiki/Covariance),

{% katex display %}
\text{Cov}(U,V) = \text{E}[UV] - \text{E}[U]E[V] = \gamma\sigma_{v}\sigma_{u},
{% endkatex %}

and finally the [Correlation Coefficient](https://en.wikipedia.org/wiki/Pearson_correlation_coefficient),

{% katex display %}
\gamma = \frac{\text{Cov}(U,V)}{\sigma_{v}\sigma_{u}}.
{% endkatex %}

### Distribution Parameter Limits

In previous sections it was shown that the free parameters used in the definition of
the Bivariate Normal distribution, equation {% katex %}(12){% endkatex %}, are its the mean,
variance and correlation coefficient. Here a sketch of the change in shape of the
distribution as these parameters are varied is discussed by evaluating
limits of the parameters for the equation defining the PDF contours.
The following section will describe the results of a more detailed numerical analysis.

The equation satisfied by the contours is obtained by setting the argument of the
exponential in equation {% katex %}(12){% endkatex %} to a constant {% katex %}C^{2}{% endkatex %},

{% katex display %}
C^{2} =
  \frac{\left(u-\mu_{u} \right)^2}{\sigma_{u}^2} + \frac{\left(v-\mu_v\right)^2}{\sigma_v^2} -
  2\gamma\frac{\left(v-\mu_v\right)}{\sigma_{v}}\frac{\left(u-\mu_{u} \right)}{\sigma_{u}},
{% endkatex %}

where{% katex %}C^{2}{% endkatex %} is a constant related to the value of the contour. To develop an
expectation of the behavior the following limits of this equation are considered,
{% katex %}\gamma\to 0{% endkatex %}, {% katex %}\gamma\to \pm 1{% endkatex %},
{% katex %}\sigma_{v}/\sigma_{u}\to 1{% endkatex %},
{% katex %}\sigma_{v}/\sigma_{u}\to 0{% endkatex %} and
{% katex %}\sigma_{v}/\sigma_{u}\to \infty{% endkatex %}.

First, consider the limit {% katex %}\gamma\to 0{% endkatex %} which is easily evaluated
using the equation above,

{% katex display %}
C^{2} = \frac{\left(u-\mu_{u} \right)^{2}}{\sigma_{u}^{2}} + \frac{\left(v-\mu_v\right)^2}{\sigma_{v}^{2}},
{% endkatex %}

which is the equation of an ellipse with axes of length {% katex %}2C\sigma_{u}{% endkatex %} and
{% katex %}2C\sigma_{v}{% endkatex %}. The aspect ratio of the ellipse is given by
{% katex %}\sigma_{v}/\sigma_{u}{% endkatex %}. It follows that in the limit
{% katex %}\sigma_{v}/\sigma_{u}\to 1{% endkatex %} the contour approaches a circle and in the limits
{% katex %}\sigma_{v}/\sigma_{u}\to 0{% endkatex %} or
{% katex %}\sigma_{v}/\sigma_{u}\to \infty{% endkatex %} the contour approaches a line along the
{% katex %}u{% endkatex %} or {% katex %}v{% endkatex %} axes respectively.

To evaluate the limit {% katex %}\gamma\to \pm 1{% endkatex %} it must be noted that
{% katex %}\gamma^{2}\ne \pm 1{% endkatex %} was assumed in the derivation of inverse of the
Bivariate Transformation, equation {% katex %}(6){% endkatex %}.
To evaluate the limit it should be evaluated before inverting the transformation defining the distribution.
Taking the limit results in a transformation that is valid for {% katex %}\gamma^{2}= 1{% endkatex %}, namely,

{% katex display %}
\begin{pmatrix}
U - \mu_{u} \\
V - \mu_{v}
\end{pmatrix} =
\begin{pmatrix}
\sigma_{u} & 0 \\
\pm\sigma_{v} & 0
\end{pmatrix}
\begin{pmatrix}
X \\
Y
\end{pmatrix}.
{% endkatex %}

Evaluation of the equation above gives,

{% katex display %}
\left(V-\mu_{v}\right) = \pm \frac{\sigma_{v}}{\sigma_{u}} \left(U-\mu_{u}\right).
{% endkatex %}

It follows that as the limit is approached contours will approach a line with slope
{% katex %}\pm \frac{\sigma_{v}}{\sigma_{u}}{% endkatex %}. The slope is positive
for positive correlation and negative for negative correlation. It follows that in the limit
{% katex %}\frac{\sigma_{v}}{\sigma_{u}}\to 1{% endkatex %} the contour slope approaches
approaches a circle with {% katex %}\pm 1{% endkatex %} and in the limit
{% katex %}\frac{\sigma_{v}}{\sigma_{u}}\to 0{% endkatex %} or
{% katex %}\sigma_{v}/\sigma_{u}\to \infty{% endkatex %} the contour approaches a line along the
{% katex %}u{% endkatex %} or {% katex %}v{% endkatex %} axes respectively which is the same
as obtained in the limit {% katex %}\gamma\to 0.{% endkatex %}

The following two plots show the surface an contour plots for a distribution with
{% katex %}\sigma_{v}/\sigma_{u}=1{% endkatex %} and {% katex %}\gamma=0,{% endkatex %}
which is the case where {% katex %}u{% endkatex %} and {% katex %}v{% endkatex %} are
uncorrelated with equal variances. The contours are circles are previously described.
The surface plot is included to show how poor it is at giving a sense of the distribution shape
though it does assist in imagining the how the contour plot would be projected into the
third dimension.

<div style="text-align:center;">
  <img class="post-image" src="/assets/posts/bivariate_normal_distribution/bivariate_pdf_contour_plot_0.0_1.png">
</div>

<div style="text-align:center;">
  <img class="post-image" src="/assets/posts/bivariate_normal_distribution/bivariate_pdf_surface_plot_0.0_1.png">
</div>

The next plot also has {% katex %}\gamma=0{% endkatex %} but
{% katex %}\sigma_{v}/\sigma_{u}=2{% endkatex %}. The contours become ellipses with the axis
aligned with the {% katex %}v{% endkatex %} axis.

<div style="text-align:center;">
  <img class="post-image" src="/assets/posts/bivariate_normal_distribution/bivariate_pdf_contour_plot_0.0_2.png">
</div>

The final plot has {% katex %}\gamma=0.5{% endkatex %} and {% katex %}\sigma_{v}/\sigma_{u}=1{% endkatex %}.
The contours are symmetric about the line {% katex %}v=u{% endkatex %} as expected for the limit
{% katex %}\gamma\to \pm 1{% endkatex %}. If the correlation were negative the contours would be reflected about the
origin and symmetric about the line {% katex %}v=-u{% endkatex %}.

<div style="text-align:center;">
  <img class="post-image" src="/assets/posts/bivariate_normal_distribution/bivariate_pdf_contour_plot_0.5_1.png">
</div>

### Probability Density Contours

This section will describe a more detailed analysis of the Bivariate Normal PDF contours
consisting of a larger range of values for the parameters {% katex %}\gamma{% endkatex %}
and {% katex %}\sigma_{v}/\sigma{u}{% endkatex %}.

The equation satisfied by the contours is obtained by setting the argument of the
exponential in equation {% katex %}(12){% endkatex %} to a constant {% katex %}C^{2}{% endkatex %},

{% katex display %}
\begin{aligned}
C^{2} &=
  \frac{\left(u-\mu_{u} \right)^2}{\sigma_{u}^2} + \frac{\left(v-\mu_v\right)^2}{\sigma_v^2} -
  2\gamma\frac{\left(v-\mu_v\right)}{\sigma_{v}}\frac{\left(u-\mu_{u} \right)}{\sigma_{u}} \\
&= \frac{\left(u-\mu_{u} \right)^2}{\sigma_{u}^2} + \frac{\left(v-\mu_v\right)^2}{\sigma_v^2} -
  2\gamma\frac{\left(v-\mu_v\right)}{\sigma_{v}}\frac{\left(u-\mu_{u} \right)}{\sigma_{u}} +
  \frac{\gamma^{2}}{\sigma_{v}^{2}}\left(v-\mu_v\right)^{2} - \frac{\gamma^{2}}{\sigma_{v}^{2}}\left(v-\mu_v\right)^{2} \\
&= \left[\frac{\left(u-\mu_{u} \right)}{\sigma_{u}} - \frac{\gamma}{\sigma_{v}}\left(v-\mu_v\right)\right]^{2}  +
  \frac{\left(v-\mu_v\right)^2}{\sigma_v^2}(1-\gamma^2).
\end{aligned}
{% endkatex %}

Here the equation is put into a form that is more easily evaluated by completing the square of the original equation.
If both sides are divided by {% katex %}C^{2}{% endkatex %} the equation below is obtained,

{% katex display %}
\frac{1}{C^2}\left[\frac{\left(u-\mu_{u} \right)}{\sigma_{u}} - \frac{\gamma}{\sigma_{v}}\left(v-\mu_v\right)\right]^{2}  +
  \frac{\left(v-\mu_v\right)^2}{C^2\sigma_v^2}(1-\gamma^2) = 1.
{% endkatex %}

This equation is equivalent the equation of a unit circle which satisfies the equation,

{% katex display %}
\sin^{2}{\theta} + \cos^2{\theta} = 1.
{% endkatex %}

If {% katex %}\theta{% endkatex %} is assumed a parametric parameter satisfying
{% katex %}0\geq \theta \geq 2\pi{% endkatex %} a parametric equation for the contours is obtained
by making the following change of variables,

{% katex display %}
\begin{aligned}
\sin{\theta} &= \frac{1}{C}\left[\frac{\left(u-\mu_{u} \right)}{\sigma_{u}} - \frac{\gamma}{\sigma_{v}}\left(v-\mu_v\right)\right] \\
\cos{\theta} &= \frac{\left(v-\mu_v\right)}{C\sigma_v}\sqrt{(1-\gamma^2)}.
\end{aligned}
{% endkatex %}

Solving these equations for {% katex %}u{% endkatex %} and {% katex %}v{% endkatex %} yields the following
parametric equations,

{% katex display %}
\begin{aligned}
u &= C\sigma_{u}\left[\sin{\theta} + \frac{\gamma}{\sqrt{1-\gamma^2}}\cos{\theta}\right] + \mu_{u} \\
v &= \frac{C\sigma_v}{\sqrt{1-\gamma^2}}\cos{\theta} + \mu_{v}.
\end{aligned}\ \ \ \ \ (30)
{% endkatex %}

To plot the actual contours a relation between the constant {% katex %}C{% endkatex %} and the the value of the
PDF along the contour is required. This relation is obtained by replacing the argument of the exponential
in equation {% katex %}(12){% endkatex %} with {% katex %}C^{2}{% endkatex %},

{% katex display %}
K=\frac{1}{2\pi\sigma_{u}\sigma_{v} \sqrt{1-\gamma^{2}}}e^{\frac{\footnotesize{-C^2}}{\footnotesize{2(1-\gamma^2)}}},
{% endkatex %}

where {% katex %}K{% endkatex %} is the PDF value along the contour. Solving this equation for
{% katex %}C{% endkatex %} gives,

{% katex display %}
C = \left[-2\left(1-\gamma^2\right)\ln{\left(2K\pi\sigma_{u}\sigma_{v}\sqrt{1-\gamma^{2}}\right)}\right]^{\frac{\footnotesize{1}}{\footnotesize{2}}}.
{% endkatex %}

If {% katex %}C{% endkatex %} is assumed to be real the following constraint must be satisfied,

{% katex display %}
K\ <\ \frac{1}{2\pi\sigma_{u}\sigma_{v}\sqrt{1-\gamma^{2}}},
{% endkatex %}

which places an upper bound on the value of the peak of the distribution. The following two plots of the
parametric equations defined by {% katex %}(30){% endkatex %} should be compared to the contour plots
from the previous section.

<div style="text-align:center;">
  <img class="post-image" src="/assets/posts/bivariate_normal_distribution/bivariate_pdf_parameterized_contour_correlation_0.0.png">
</div>

<div style="text-align:center;">
  <img class="post-image" src="/assets/posts/bivariate_normal_distribution/bivariate_pdf_parameterized_contour_correlation_0.5.png">
</div>

To get a sense of how the contour shape varies with the distribution parameters the following two plots scan the range of
{% katex %}\sigma_{v}/\sigma{u}{% endkatex %} with {% katex %}\gamma = 0{% endkatex %} to illustrate the
limits {% katex %}\sigma_{v}/\sigma{u}\to 0{% endkatex %} and {% katex %}\sigma_{v}/\sigma{u}\to\infty{% endkatex %}
respectively with {% katex %}\gamma=0{% endkatex %}.
This result agrees with the expectation obtained in the previous analysis.
The {% katex %}\sigma_{v}/\sigma{u}\to 0{% endkatex %} series of contours approaches the {% katex %}u{% endkatex %} axis and
the {% katex %}\sigma_{v}/\sigma{u}\to\infty{% endkatex %} contours approach the {% katex %}v{% endkatex %} axis.


<div style="text-align:center;">
  <img class="post-image" src="/assets/posts/bivariate_normal_distribution/bivariate_pdf_parameterized_contour_sigma1_scan.png">
</div>

<div style="text-align:center;">
  <img class="post-image" src="/assets/posts/bivariate_normal_distribution/bivariate_pdf_parameterized_contour_sigma2_scan.png">
</div>

The next two plots illustrate the limit {% katex %}\gamma\to 1{% endkatex %} and {% katex %}\gamma\to -1{% endkatex %} respectively
with {% katex %}\sigma_{u}/\sigma{v}=1{% endkatex %}. The {% katex %}\gamma\to 1{% endkatex %} plot is converging to the line
{% katex %}v=u{% endkatex %} and the {% katex %}\gamma\to -1{% endkatex %} to the line {% katex %}v=-u{% endkatex %} as described
in the previous section. Note that as {% katex %}\gamma{% endkatex %} approaches the limit the semi-axis of the contour increases along
the appropriate limiting line without any rotation of the contour.

<div style="text-align:center;">
  <img class="post-image" src="/assets/posts/bivariate_normal_distribution/bivariate_pdf_parameterized_contour_positive_correlation_scan.png">
</div>

<div style="text-align:center;">
  <img class="post-image" src="/assets/posts/bivariate_normal_distribution/bivariate_pdf_parameterized_contour_negative_correlation_scan.png">
</div>

The final two plots look at the {% katex %}\gamma\to 1{% endkatex %} limit but this time the first plot has
{% katex %}\sigma_{v}/\sigma{u}=0.5{% endkatex %} and the second {% katex %}\sigma_{v}/\sigma{u}=2{% endkatex %}. The first is converging to th e line {% katex %}v=0.5u{% endkatex %} and the second converges to the
line {% katex %}v=2u{% endkatex %} in agreement with the previous analysis. The behavior of the
contour as the limit is approached is more interesting since the ellipse has to rotate to reach the limit.
This is caused by the {% katex %}\gamma=0{% endkatex %} contour also being an ellipse. As
{% katex %}/gamma{% endkatex %} increases it begins to distort along the limiting line. The result
is the vector sum of the semi-major axis of the {% katex %}\gamma=0{% endkatex %} contour and the
developing component along the limiting line. If the {% katex %}\gamma=0{% endkatex %}
contour were a symmetric circle the axis is uniform and lies along the limiting line.

<div style="text-align:center;">
  <img class="post-image" src="/assets/posts/bivariate_normal_distribution/bivariate_pdf_parameterized_contour_correlation_sigma1_scan.png">
</div>

<div style="text-align:center;">
  <img class="post-image" src="/assets/posts/bivariate_normal_distribution/bivariate_pdf_parameterized_contour_correlation_sigma2_scan.png">
</div>

### Coordinate Transformation

In the derivation of the Bivariate Normal Distribution the linear transform defined by
equation {% katex %}(6){% endkatex %} is applied two independent
{% katex %}\textbf{Normal}(0, 1){% endkatex %} random variables {% katex %}x{% endkatex %}
and {% katex %}y{% endkatex %}. This transformation is shown below.

{% katex display %}
\begin{aligned}
x(u,v) &= \frac{1}{\sigma_{u}}\left(u-\mu_{u}\right) \\
y(u,v) &= \frac{1}{\sqrt{1-\gamma^{2}}}\left[\frac{1}{\sigma_{v}}\left(v-\mu_{v}\right) -
  \frac{\gamma}{\sigma_{u}}\left(u-\mu_{u)}\right)\right].
\end{aligned}
{% endkatex %}

Contours of constant {% katex %}u{% endkatex %} and {% katex %}v{% endkatex %}
using the transform are plotted in the {% katex %}(x, y){% endkatex %} coordinate system to understand
how the transform distorts area elements.

Contours of constant {% katex %}u = C_{u}{% endkatex %} in {% katex %}(x, y){% endkatex %}
coordinates are defined by,

{% katex display %}
\begin{aligned}
x &= \frac{1}{\sigma_{u}}\left(C_{u}-\mu_{u}\right) \\
y &= \frac{1}{\sqrt{1-\gamma^{2}}}\left[\frac{1}{\sigma_{v}}\left(v-\mu_{v}\right) -
  \frac{\gamma}{\sigma_{u}}\left(C_{u}-\mu_{u)}\right)\right],
\end{aligned}
{% endkatex %}

This set of equations defines contours which are lines of constant {% katex %}x{% endkatex %}
since the equation for {% katex %}x{% endkatex %} is constant.

The contours of constant {% katex %}v=C_{v}{% endkatex %} are defined by,

{% katex display %}
\begin{aligned}
x &= \frac{1}{\sigma_{u}}\left(u-\mu_{u}\right) \\
y &= \frac{1}{\sqrt{1-\gamma^{2}}}\left[\frac{1}{\sigma_{v}}\left(C_{v}-\mu_{v}\right) -
  \frac{\gamma}{\sigma_{u}}\left(u-\mu_{u)}\right)\right].
\end{aligned}
{% endkatex %}

Substituting the expression for {% katex %}x{% endkatex %} into the expression for
{% katex %}y{% endkatex %} gives,

{% katex display %}
y = \frac{-\gamma}{\sqrt{1-\gamma^{2}}}x +
  \frac{1}{\sigma_{v}\sqrt{1-\gamma^{2}}}\left(C_{v}-\mu_{v}\right),
{% endkatex %}

which is the equation a line with slope {% katex %}-\gamma/\sqrt{1-\gamma^{2}}{% endkatex %}.

The following plot shows the transformation for parameter values
{% katex %}\gamma=0{% endkatex %}, {% katex %}\mu_{u}=\mu_{v}=0{% endkatex %} and
{% katex %}\sigma_{u}=\sigma_{v}=1{% endkatex %} which results in the transform,

{% katex display %}
\begin{aligned}
x &= C_{u} \\
y &= C_{v},
\end{aligned}
{% endkatex %}

and Jacobian, from equation {% katex %}(10){% endkatex %}, is given by,

{% katex display %}
\mid J \mid = \frac{1}{\sigma_{u}\sigma_{v} \sqrt{1-\gamma^{2}}} = 1.
{% endkatex %}

It follows that area elements satisfy,

{% katex display %}
dxdy = |J|dudv = dudv.
{% endkatex %}

Thus, for this particular choice of transform parameters area elements are preserved. Inspection of the plot and transform
confirms that this is the case since the transform exactly maps {% katex %}(u,v){% endkatex %} coordinates onto
{% katex %}(x,y){% endkatex %} coordinates.

<div style="text-align:center;">
  <img class="post-image" src="/assets/posts/bivariate_normal_distribution/bivariate_normal_transformation_correlation_0.png">
</div>

The next plot has parameters {% katex %}\gamma=0{% endkatex %} and {% katex %}\sigma_{u}=1{% endkatex %}
and {% katex %}\sigma_{v}=2{% endkatex %}. The transform associated this parameter choice is given by,

{% katex display %}
\begin{aligned}
x &= C_{u} \\
y &= \frac{1}{\sigma_{2}}C_{v},
\end{aligned}
{% endkatex %}

with Jacobian,

{% katex display %}
\mid J \mid = \frac{1}{\sigma_{u}\sigma_{v} \sqrt{1-\gamma^{2}}} = \frac{1}{2}.
{% endkatex %}

Once again the {% katex %}u{% endkatex %} contours are aligned with lines of constant{% katex %}x{% endkatex %} but
the {% katex %}v{% endkatex %} contours are compressed by a factor of {% katex %}2{% endkatex %} relative to lines of
constant {% katex %}y{% endkatex %}. If follows that the transform reduces the size of {% katex %}(u,v){% endkatex %}
area elements by a factor of {% katex %}1/2{% endkatex %} when transformed, namely,

{% katex display %}
dxdy = |J|dudv = \frac{1}{2}dudv.
{% endkatex %}

What this is saying is that for an arbitrary area element {% katex %}dudv{% endkatex %} in the {% katex %}(u,v){% endkatex %}
coordinates when transformed to the {% katex %}(x,y){% endkatex %} the element has {% katex %}1/2{% endkatex %} the area. This is seen
to be the case in the plot where the area of a {% katex %}(u,v){% endkatex %} rectangle is reduced by a factor of
{% katex %}(2){% endkatex %}.

<div style="text-align:center;">
  <img class="post-image" src="/assets/posts/bivariate_normal_distribution/bivariate_normal_transformation_correlation_0_sigma.png">
</div>

The final plot introduces the impact of correlation to the transform by using the parameters
{% katex %}\gamma=0.5{% endkatex %} and {% katex %}\sigma_{u}=\sigma_{v}=1{% endkatex %}. The transform corresponding
to this parameter choice is given by,

{% katex display %}
\begin{aligned}
x &= C_{u} \\
y &= \frac{2}{\sqrt{3}}\left(-\frac{1}{2}x + C_{v}\right),
\end{aligned}
{% endkatex %}

with Jacobian,

{% katex display %}
\mid J \mid = \frac{1}{\sigma_{u}\sigma_{v} \sqrt{1-\gamma^{2}}} = \frac{2}{\sqrt{3}}.
{% endkatex %}

The {% katex %}u{% endkatex %} contours are aligned again but now since there is correlation the
{% katex %}v{% endkatex %} contours are lines with a negative slope so rectangular area elements in
{% katex %}(u,v){% endkatex %} are transformed into parallelograms in {% katex %}(x, y){% endkatex %}
coordinates. The area of one of the parallelograms is {% katex %}\Delta y\Delta x{% endkatex %}, where
{% katex %}\Delta y{% endkatex %} is the spacing between contours of constant {% katex %}v{% endkatex %}
and {% katex %}\Delta x{% endkatex %} is the spacing between contours of constant {% katex %}u{% endkatex %}.
Now, from the transform above it is seen that,

{% katex display %}
\begin{aligned}
\Delta x &= \Delta C_{u} \\
\Delta y &= \frac{2}{\sqrt{3}}\Delta C_{v},
\end{aligned}
{% endkatex %}

but {% katex %}\Delta C_{u}=\Delta C_{v} = 1{% endkatex %}, so the area of the parallelogram is given by,

{% katex display %}
\Delta y \Delta x = \frac{2}{\sqrt{3}},
{% endkatex %}

which is equal to the Jacobian. It follows that for this transformation area elements are increased in size by
a factor of {% katex %}2/\sqrt{3}{% endkatex %},

{% katex display %}
dxdy = |J|dudv = \frac{2}{\sqrt{3}}dudv.
{% endkatex %}

<div style="text-align:center;">
  <img class="post-image" src="/assets/posts/bivariate_normal_distribution/bivariate_normal_transformation_correlation_0.5.png">
</div>

## Conclusions

The Bivariate Normal distribution is widely used because it provides a simple model of correlated random variables.
It is interesting to study because it is modeled as a linear transform of independent {% katex %}\textbf{Normal}(0, 1){% endkatex %}
random variables that can serve as an introduction to the concepts used in application of transformations to multivariate integrals.
Here the background needed to understand transformations of multivariate integrals was developed by starting with a discussion of the
interpretation of the vector cross product as an area. This idea was then applied to the derivation of the Jacobian Matrix for an arbitrary
multivariate transformation of differential area elements. Next, the transformation used to define the Bivariate Normal distribution was
introduced and applied do a distribution of two independent {% katex %}\textbf{Normal}(0, 1){% endkatex %} random variables. The Jacobian
matrix was next computed and the Bivariate Normal PDF derived. A matrix form of the Bivariate Normal PDF
based the idea of the [Covariance Matrix](https://en.wikipedia.org/wiki/Covariance_matrix) was introduced and shown to
be equivalent to the linear transform version first discussed. The covariance matrix for more easily extends to higher dimensions.
The linear transform used to define the Bivariate Normal PDF introduced five parameters. It was next shown that these parameters are the
means, {% katex %}\mu_{u}, \mu_{v}{% endkatex %}, variance, {% katex %}\sigma_{u}, \sigma_{v}{% endkatex %} and correlation coefficient,
{% katex %}\gamma{% endkatex %}, of the distribution. This was followed by a derivation of the conditional distributions which are useful
in simulations. The change in shape of the distribution was then investigated by
evaluating limits for the PDF contours that included {% katex %}\gamma\to 0{% endkatex %}, {% katex %}\gamma\to \pm 1{% endkatex %},
{% katex %}\sigma_{v}/\sigma_{u}\to 1{% endkatex %}, {% katex %}\sigma_{v}/\sigma_{u}\to 0{% endkatex %} and
{% katex %}\sigma_{v}/\sigma_{u}\to \infty{% endkatex %}. This was followed by numerically investigating the convergence to these limits
using a parametric form of the equation defining the PDF contours. The last topic discussed was the variation of the linear coordinate
transformation with the distribution parameters. It was shown that resulting changes in area of transformed area elements were described
by the Jacobian.
