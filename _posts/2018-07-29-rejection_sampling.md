---
title: Rejection Sampling
key: 20180729
image: /assets/posts/rejection_sampling/weibull_normal_3_efficiency.png
author: Troy Stribling
permlink: /rejetion_sampling.html
comments: false
---

Rejection Sampling is a method for obtaining samples for a known target probability distribution
with no sampler using samples from some other proposal distribution with a sampler.
It is a more general method than
[Inverse CDF Sampling]({{ site.baseurl }}{% link _posts/2018-07-21-inverse_cdf_sampling.md %}) which requires
distribution to have an invertible CDF. Inverse CDF Sampling transforms a
{% katex %}\textbf{Uniform}(0,\ 1){% endkatex %} random variable into a random variable with
a desired target distribution using the inverted CDF of the target distribution. While, Rejection Sampling
is a method for transformation of random variables from arbitrary proposal distributions into a desired target
distribution.

<!--more-->

The implementation of Rejection Sampling requires the consideration of a target distribution,
{% katex %}f_X(X){% endkatex %}, a proposal distribution, {% katex %}f_Y(Y){% endkatex %}, and a {% katex %}\textbf{Uniform}(0,\ 1){% endkatex %} acceptance probability, {% katex %}U{% endkatex %}, with distribution {% katex %}f_U(U)=1{% endkatex %}. A proposal sample, {% katex %}Y{% endkatex %}, is generated using, {% katex %}f_Y(Y){% endkatex %}, and independently a uniform acceptance sample, {% katex %}U{% endkatex %}, is generated
using {% katex %}f_U(U){% endkatex %}.
A criterion is defined for acceptance of a sample, {% katex %}X{% endkatex %}, to be considered a
sample of {% katex %}f_X(X){% endkatex %},

{% katex display %}
U\ \leq\ \frac{f_X(Y)}{cf_Y(Y)}\ \ \ \ \ (1),
{% endkatex %}

where, {% katex %}c{% endkatex %}, is chosen to satisfy
{% katex %}0\ \leq\ f_X(Y)/cf_Y(Y)\ \leq\ 1, \ \ \forall\ Y{% endkatex %}. If equation
{% katex %}(1){% endkatex %} is satisfied the proposed sample {% katex %}Y{% endkatex %} is
accepted as a sample of {% katex %}f_X(X){% endkatex %} where {% katex %}X=Y{% endkatex %}.
If equation {% katex %}(1){% endkatex %} is not satisfied {% katex %}Y{% endkatex %} is discarded.

The acceptance function is defined by,

{% katex display %}
h(Y) = \frac{f_X(Y)}{f_Y(Y)}\ \ \ \ \ (2).
{% endkatex %}

It can be insightful to compare {% katex %}h(y){% endkatex %} to {% katex %}f_X(y){% endkatex %}
when choosing a proposal distribution. If {% katex %}h(y){% endkatex %} does not have a peak that
can enclose the peak in {% katex %}f_X(Y){% endkatex %} then the choice of
{% katex %}f_Y(Y){% endkatex %} should be reconsidered.

The Rejection Sampling algorithm can be summarized in the following steps that are repeated for the generation of each sample,

1. Generate a sample {% katex %}Y \sim f_Y(Y){% endkatex %}.
2. Generate a sample {% katex %}U\sim\textbf{Uniform}(0,\ 1){% endkatex %} independent of {% katex %}Y{% endkatex %}.
3. If equation {% katex %}(1){% endkatex %} is satisfied then {% katex %}X=Y{% endkatex %} is accepted as a sample
of {% katex %}f_X(X){% endkatex %}. If equation {% katex %}(1){% endkatex %} is not satisfied then {% katex %}Y{% endkatex %}
is discarded.

## Proof

To prove that Rejection Sampling works it must be shown that,
{% katex display %}
P\left[Y\ \leq\ y\ |\ U\ \leq\ \frac{f_X(Y)}{cf_Y(Y)}\right]=F_X(y)\ \ \ \ \ (3),
{% endkatex %}

where {% katex %}F_X(y){% endkatex %} is the CDF for {% katex %}f_X(y){% endkatex %},
{% katex display %}
F_X(y) = \int_{-\infty}^{y} f_X(w)dw.
{% endkatex %}

To prove equation {% katex %}(2){% endkatex %} a couple of intermediate steps are required. First,
The joint distribution of {% katex %}U{% endkatex %} and {% katex %}Y{% endkatex %} containing the
acceptance constraint will be evaluated,

{% katex display %}
P\left[U\ \leq\ \frac{f_X(Y)}{cf_Y(Y)},\ Y\ \leq\ y\right] = \frac{F_X(y)}{c}\ \ \ \ \ (4).
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
P\left[U\ \leq\ \frac{f_X(Y)}{cf_Y(Y)}\right] = \frac{1}{c}\ \ \ \ \ (5).
{% endkatex %}

This result follows from equation {% katex %}(4){% endkatex %} by taking the
limit {% katex %}y\to\infty{% endkatex %}
and noting that, {% katex %}\int_{-\infty}^{\infty} f_X(y) dy = 1{% endkatex %}.

{% katex display %}
\begin{aligned}
P\left[U\ \leq\ \frac{f_X(Y)}{cf_Y(Y)}\right] &= \int_{-\infty}^{\infty}\int_{0}^{f_X(w)/cf_Y(w)} f_{YU}(w, u) du dw\\
&= \frac{1}{c}\int_{-\infty}^{\infty} f_X(w) dw \\
&= \frac{1}{c}
\end{aligned}
{% endkatex %}

Finally, equation {% katex %}(3){% endkatex %} can be proven, using the definition of [Conditional Probability](https://en.wikipedia.org/wiki/Conditional_probability),
equation {% katex %}(4){% endkatex %} and equation {% katex %}(5){% endkatex %},

{% katex display %}
\begin{aligned}
P\left[Y\ \leq\ y\ |\ U\ \leq\ \frac{f_X(Y)}{cf_Y(Y)}\right] &= \frac{P\left[U\ \leq\ \frac{f_X(Y)}{cf_Y(Y)},\ Y \leq y\right]}{P\left[U\ \leq\ \frac{f_X(Y)}{cf_Y(Y)}\right]}\\
&=\frac{F_X(y)}{c}\frac{1}{1/c}\\
&=F_X(y)
\end{aligned}
{% endkatex %}

## Implementation

An implementation in Python of the Rejection Sampling algorithm,

```python
def rejection_sample(h, y_samples, c):
    nsamples = len(y_samples)
    u = numpy.random.rand(nsamples)
    accepted_mask = (u <= h(y_samples) / c)
    return y_samples[accepted_mask]
```

The above function `rejection_sample(h, y_samples, c)` takes three arguments as input which are
described in the table below.

| Arguments  | Description  |
| :-----: | :---- |
| `h` | The acceptance function. Defined by {% katex %}h(Y)=f_X(Y)/f_Y(Y){% endkatex %}.|
| `y_samples` | Array of samples of {% katex %}Y{% endkatex %} generated using {% katex %}f_Y(Y){% endkatex %}.|
|`c`   |A constant chosen so {% katex %}0\ \leq\ h(Y)/c\ \leq\ 1, \ \ \forall\ Y{% endkatex %} |

The execution of `rejection_sample(h, y_samples, c)` begins by generating an appropriate number of acceptance variable samples, {% katex %}U{% endkatex %}, and
then determines which satisfy the acceptance criterion specified by equation (1). The accepted samples are then returned.

## Examples

Consider the [Weibull Distribution](https://en.wikipedia.org/wiki/Weibull_distribution). The PDF is
given by,

{% katex display %}
f_X(x; k, \lambda) =
\begin{cases}
\frac{k}{\lambda}\left(\frac{x}{\lambda} \right)^{k-1} e^{\left(\frac{-x}{\lambda}\right)^k} & x\ \geq\ 0 \\
0 & x < 0
\end{cases} \ \ \ \ \ (4),
{% endkatex %}

where {% katex %}k\ >\ 0{% endkatex %} is the shape parameter and {%katex %}\lambda\ >\ 0{% endkatex %} the scale parameter.
The CDF is given by,

{% katex display %}
F_X(x; k, \lambda) =
\begin{cases}
1-e^{\left(\frac{-x}{\lambda}\right)^k
} & x \geq 0 \\
0 & x < 0.
\end{cases}
{% endkatex %}

The first and second moments are,

{% katex display %}
\begin{aligned}
\mu & = \lambda\Gamma\left(1+\frac{1}{k}\right) \\
\sigma^2 & = \lambda^2\left[\Gamma\left(1+\frac{2}{k}\right)-\left(\Gamma\left(1+\frac{1}{k}\right)\right)^2\right]
\end{aligned} \ \ \ \ \ (5).
{% endkatex %}

In the examples described here it will be assumed that {% katex %}k=5.0{% endkatex %} and
{% katex %}\lambda=1.0{% endkatex %}. The following plot shows the PDF and CDF using these values.

<img class="post-image" src="/assets/posts/rejection_sampling/weibull_pdf.png">

The following sections will compare the performance of generating Weibull samples using {% katex %}\textbf{Uniform}(0,\ m){% endkatex %} and
{% katex %}\textbf{Normal}(\mu,\ \sigma){% endkatex %} proposal distributions.

### Uniform Proposal Distribution

Here a {% katex %}\textbf{Uniform}(0,\ m){% endkatex %} proposal distribution will be used to
generate samples for the Weibull distribution {% katex %}(4){% endkatex %}. It provides a simple and
illustrative example of the algorithm. The following plot shows the
target distribution {% katex %}f_X(y){% endkatex %}, the proposal distribution
{% katex %}f_Y(y){% endkatex %} and the acceptance function
{% katex %}h(y)=f_X(y)/f_Y(y){% endkatex %} used in this example. The
uniform proposal distribution requires that a bound be placed
on the proposal samples, which will be assumed to be {% katex %}m=1.6{% endkatex %}. Since
the proposal distribution is constant the acceptance function, {% katex %}h(y){% endkatex %}, will be
a constant multiple of the target distribution. This is illustrated in the plot below.

<img class="post-image" src="/assets/posts/rejection_sampling/weibull_uniform_sampled_functions.png">

The Python code used to generate the samples using  `rejection_sample(h, y_samples, c)` is listed
below.

```python
weibull_pdf = lambda v: (k/λ)*(v/λ)**(k-1)*numpy.exp(-(v/λ)**k)

k = 5.0
λ = 1.0

xmax = 1.6
ymax = 2.0
nsamples = 100000

y_samples = numpy.random.rand(nsamples) * xmax
samples = rejection_sample(weibull_pdf, y_samples, ymax)
```

The following plot compares the histogram computed form the generated samples with the target
distribution {% katex %}(4){% endkatex %}. The fit is a little ragged along the peak of the distribution but acceptable.

<img class="post-image" src="/assets/posts/rejection_sampling/weibull_uniform_sampled_distribution.png">

The next two plots illustrate convergence of the sample mean, {% katex %}\mu{% endkatex %},
and standard deviation, {% katex %}\sigma{% endkatex %}, by comparing the cumulative sums
computed from the samples to target distribution values computed
from equation {% katex %}(5){% endkatex %}. The convergence of the sampled
{% katex %}\mu{% endkatex %} is quite rapid. Within only {% katex %}10^2{% endkatex %}
samples {% katex %}\mu{% endkatex %} computed form the samples is very close the the target value
and by {% katex %}10^3{% endkatex %} samples the two values are indistinguishable. The convergence of the sampled {% katex %}\sigma{% endkatex %} to the target value is not as
rapid as the convergence of the sampled {% katex %}\mu{% endkatex %} but is still quick. By
{% katex %}10^3{% endkatex %} samples the two values are near indistinguishable.

<img class="post-image" src="/assets/posts/rejection_sampling/weibull_uniform_mean_convergence.png">

<img class="post-image" src="/assets/posts/rejection_sampling/weibull_uniform_sigma_convergence.png">

Even though {% katex %}10^5{% endkatex %} proposal samples were generated not all were accepted. The
plot below provides insight into the efficiency of the algorithm. In the plot the generated pairs of
acceptance probability {% katex %}U{% endkatex %} and sample proposal {% katex %}Y{% endkatex %} are
plotted with {% katex %}U{% endkatex %} on the vertical axis and {% katex %}Y{% endkatex %} on
the horizontal axis. Also, shown is the scaled acceptance function {% katex %}h(Y)/c{% endkatex %}.
If {% katex %}U\ >\ h(Y)/c{% endkatex %} the sample is rejected and colored orange in the plot and
if {% katex %}U\ \leq\ h(Y)/c{% endkatex %} the sample is accepted, {% katex %}X=Y{% endkatex %} and colored blue. Only {% katex %}31\%{% endkatex %} of the generated samples were accepted.

<img class="post-image" src="/assets/posts/rejection_sampling/weibull_uniform_efficiency.png">

To improve the acceptance percentage of proposed samples a different proposal distribution must be
considered. In the plot above it is seen that the {% katex %}\textbf{Uniform}(0,\ 1.6){% endkatex %} proposal distribution uniformly samples the space enclosed by the rectangle it defines without consideration
for the shape of
the target distribution. The acceptance percentage will be determined by the ratio of the target
distribution area enclosed by the proposal distribution and the proposal distribution area.
As the target distribution becomes sharp
the acceptance percentage will decrease. A proposal distribution that samples the area under
{% katex %}h(Y)/c{% endkatex %} efficiently will have a higher acceptance percentage. It should be kept in mind that rejection of proposal samples is required for the algorithm to work.
If no proposal samples are rejected the proposal and target distributions will be equivalent.

### Normal Proposal Distribution

In this section a sampler using a {% katex %}\textbf{Normal}(\mu,\ \sigma){% endkatex %} proposal
distribution and target Weibull distribution is discussed. A Normal proposal distribution has advantages over
the {% katex %}\textbf{Uniform}(0,\ m){% endkatex %} distribution discussed in the previous
section. First, it can provide unbounded samples, while a uniform proposal requires specifying bounds
on the samples. Second, it is a closer approximation to the target distribution so it should provide samples
that are accepted with greater frequency. A disadvantage of the Normal proposal distribution is that it requires specification of {% katex %}\mu{% endkatex %} and {% katex %}\sigma{% endkatex %}.
If these parameters are the slightest off the performance of the
sampler will be severely degraded. To learn this lesson the first attempt will assume values for
both the parameters that closely match the target distribution. The following plot compares the {% katex %}f_X(y){% endkatex %}, the proposal distribution {% katex %}f_Y(y){% endkatex %} and the acceptance function
{% katex %}h(y){% endkatex %}. There is a large peak in {% katex %}h(y){% endkatex %} to right caused
by the more rapid increase of the Weibull distribution relative to the Normal distribution with the result
that most of its mass is not aligned with the target distribution.

<img class="post-image" src="/assets/posts/rejection_sampling/weibull_normal_1_sampled_functions.png">

The Python code used to generate the samples using  `rejection_sample(h, y_samples, c)` is listed
below.

```python
weibull_pdf = lambda v: (k/λ)*(v/λ)**(k-1)*numpy.exp(-(v/λ)**k)

def normal(μ, σ):
    def f(x):
        ε = (x - μ)**2/(2.0*σ**2)
        return numpy.exp(-ε)/numpy.sqrt(2.0*numpy.pi*σ**2)
    return f

k = 5.0
λ = 1.0

σ = 0.2
μ = 0.95

xmax = 1.6
nsamples = 100000

y_samples = numpy.random.normal(μ, σ, nsamples)
ymax = h(x_values).max()
h = lambda x: weibull_pdf(x) / normal(μ, σ)(x)

samples = rejection_sample(h, y_samples, ymax)
```

The first of the following plots compares the histogram computed from the generated samples with the target
distribution {% katex %}(4){% endkatex %} and the second illustrates which proposal samples were accepted.
The histogram fit is good but only {% katex %}23\%{% endkatex %} of the samples were accepted which is worse
than the result obtained with the uniform proposal previously discussed.

<img class="post-image" src="/assets/posts/rejection_sampling/weibull_normal_1_sampled_distribution.png">

<img class="post-image" src="/assets/posts/rejection_sampling/weibull_normal_1_efficiency.png">

In an attempt to improve the acceptance rate {% katex %}\mu{% endkatex %} of the proposal distribution
is decreased slightly and {% katex %}\sigma{% endkatex %} is increased.
The result is shown in the next plot.
The proposal distribution now covers the target distribution tails. The acceptance function,
{% katex %}h(Y){% endkatex %}, now has its peak inside the target distribution with significant
overlap of mass.

<img class="post-image" src="/assets/posts/rejection_sampling/weibull_normal_3_sampled_functions.png">

The result is much improved. In the plot below it is seen that the percentage of accepted samples has
increased to {% katex %}79\%{% endkatex %}.

<img class="post-image" src="/assets/posts/rejection_sampling/weibull_normal_3_efficiency.png">

The first of the plots below compares the histogram computed from generated samples with the target
distribution {% katex %}(4){% endkatex %} and next two compare the cumulative values of {% katex %}\mu{% endkatex %} and {% katex %}\sigma{% endkatex %} computed from the generated samples with the target distribution values from equation {% katex %}(5){% endkatex %}. The histogram is the best fit of the
examples discussed here and convergence of the sampled {% katex %}\mu{% endkatex %} and
{% katex %}\sigma{% endkatex %} occurs in about {% katex %}10^3{% endkatex %} samples.

<img class="post-image" src="/assets/posts/rejection_sampling/weibull_normal_3_sampled_distribution.png">

<img class="post-image" src="/assets/posts/rejection_sampling/weibull_normal_3_mean_convergence.png">

<img class="post-image" src="/assets/posts/rejection_sampling/weibull_normal_3_sigma_convergence.png">

Sampling the Weibull distribution with a Normal proposal distribution can produce a better result than a
uniform distribution but care must be exercised in selecting the Normal distribution parameters.
Some choices can produce inferior results. Analysis of the the acceptance function
{% katex %}(2){% endkatex %} can provide guidance in parameter selectiion.
