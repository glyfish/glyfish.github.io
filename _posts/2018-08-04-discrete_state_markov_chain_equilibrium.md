---
title: Discrete State Markov Chain Equilibrium
key: 20180804
image: /assets/posts/discrete_state_markov_chain_equilibrium/distribution_comparison.png
author: Troy Stribling
permlink: /discrete_state_markov_chain_equilibrium.html
comments: false
---
A [Markov Chain](https://en.wikipedia.org/wiki/Markov_chain) is a sequence of states
where transitions between states occur in time with
the probability of a transition depending only on the previous state. Here the
states will be assumed a discrete finite set and time a discrete unbounded set. If the
set of states is given by {% katex %}\{x_1,\ x_2,\ldots,\ x_N\}{% endkatex %} the probability
that the process will be in state {% katex %}x_i{% endkatex %} at time {% katex %}t{% endkatex %}
is denoted by {% katex %}P(X_t=x_i){% endkatex %}. Markov Chain equilibrium is defined by
{% katex %}\lim_{t\to\infty}P(X_t=x_i)\ <\ \infty{% endkatex %}, that is, as time advances  
{% katex %}P(X_t=x_i){% endkatex %} becomes independent of time. Here a solution
for this limit is discussed and illustrated with examples.

<!--more-->

## Model

The Markov Chain model is constructed from the set of states
{% katex %}\{x_1,\ x_2,\ldots,\ x_N\}{% endkatex %} in time.
The process starts at time {% katex %}t=0{% endkatex %} with state {% katex %}X_0=x_i{% endkatex %}.
At the next step, {% katex %}t=1{% endkatex %}, the process will assume a state
{% katex %}X_1=x_j{% endkatex %} with probability {% katex %}P(X_1=x_j|X_0=x_i){% endkatex %}.
At the next time step {% katex %}t=2{% endkatex %} the process state will be
{% katex %}X_2=x_k{% endkatex %} with probability,
{% katex display %}
P(X_2=x_k|X_0=x_i, X_1=x_j)=P(X_2=x_k|X_1=x_j),
{% endkatex %}
since the probability of state transition depends only upon the state at the previous time step.
For an arbitrary time the transition to a state at the next time will occur with probability,
{% katex %}P(X_{t+1}=x_j|X_t=x_i){% endkatex %}. Transition probabilities have the form of
a matrix,

{% katex display %}
P_{ij} = P(X_{t+1}=x_j|X_t=x_i).
{% endkatex %}

{% katex %}P{% endkatex %} will be a square {% katex %}N\times N{% endkatex %} matrix
where the size {% katex %}N{% endkatex %} is determined by the number of possible states. Each
row represents the Markov Chain transition probability from that state at
time {% katex %}t{% endkatex %} and the columns the values at {% katex %}t+1{% endkatex %}. It follows that,
{% katex display %}
\begin{gathered}
\sum_{j=1}^{N}P_{ij} = 1\\
P_{ij}\ \geq\ 0
\end{gathered} \ \ \ \ \ (1).
{% endkatex %}

Equation {% katex %}(1){% endkatex %} is the definition of a [Stochastic Matrix](https://en.wikipedia.org/wiki/Stochastic_matrix).

The transition probability across two time steps can be obtained with use of the
[Law of Total Probability](https://en.wikipedia.org/wiki/Law_of_total_probability),
{% katex display %}
\begin{aligned}
P(X_{t+2}=x_j|X_t=x_i) &= \sum_{k=1}^{N} P(X_{t+2}=x_j | X_{t}=x_i, X_{t+1}=x_k)P(X_{t+1}=x_k | X_{t}=x_i) \\
&= \sum_{k=1}^{N} P(X_{t+2}=x_j | X_{t+1}=x_k)P(X_{t+1}=x_k | X_{t}=x_i) \\
&= \sum_{k=1}^{N} P_{kj}P_{ik} \\
&= \sum_{k=1}^{N} P_{ik}P_{kj} \\
&= {(P^2)}_{ij},
\end{aligned}
{% endkatex %}

where the last step follows from the definition of matrix multiplication. It is straight forward but
tedious to use [Mathematical Induction](https://en.wikipedia.org/wiki/Mathematical_induction) to extend the previous result
to the case of an arbitrary time difference, {% katex %}\tau{% endkatex %},
{% katex display %}
P(X_{t+\tau}=x_j|X_t=x_i) = {(P^{\tau})}_{ij}\ \ \ \ \ (2).
{% endkatex %}

It should be noted that since {% katex %}{(P^{\tau})}_{ij}{% endkatex %} is a transition probability it must
satisfy,

{% katex display %}
\begin{gathered}
\sum_{j=1}^{N} {(P^{\tau})}_{ij}\ =\ 1 \\
{(P^{\tau})}_{ij}\ \geq\ 0
\end{gathered} \ \ \ \ \ (3).
{% endkatex %}

To determine the equilibrium solution of the probability distribution of the Markov Chain states,
{% katex %}\{x_1,\ x_2,\ldots,\ x_N\}{% endkatex %} its time variability must be determined.
Begin by considering the distribution of states at {% katex %}t=0{% endkatex %}.
This distribution can be written as a column vector,
{% katex display %}
\pi =
\begin{pmatrix}
\pi_1 \\
\pi_2 \\
\vdots \\
\pi_N
\end{pmatrix} =
\begin{pmatrix}
P(X_0=x_1) \\
P(X_0=x_2) \\
\vdots \\
P(X_0=x_N)
\end{pmatrix},
{% endkatex %}

since it is a probability distribution {% katex %}\pi_i{% endkatex %} must satisfy,
{% katex display %}
\begin{gathered}
\sum_{i=1}^{N} \pi_i\ =\ 1\\
\pi_i\ \geq \ 0
\end{gathered} \ \ \ \ \ (4).
{% endkatex %}

The distribution of states after the first step is given by,
{% katex display %}
\begin{aligned}
P(X_1=x_j) &= \sum_{i=1}^{N} P(X_1=x_j|X_0=x_i)P(X_0=x_i) \\
&= \sum_{i=1}^{N} P_{ij}\pi_{i} \\
&= \sum_{i=1}^{N} \pi_{i}P_{ij} \\
&= {(\pi^{T}P)}_{j},
\end{aligned}
{% endkatex %}

where {% katex %}\pi^T{% endkatex %} is the transpose of {% katex %}\pi{% endkatex %}. Similarly, the distribution of states after the second step is,
{% katex display %}
\begin{aligned}
P(X_2=x_j) &= \sum_{i=1}^{N} P(X_2=x_j|X_1=x_i)P(X_1=x_i) \\
&= \sum_{i=1}^{N} P_{ij}{(\pi^{T}P)}_{i} \\
&= \sum_{i=1}^{N} P_{ij}\sum_{k=1}^{N} \pi_{k}P_{ki} \\
&= \sum_{k=1}^{N} \pi_{k} \sum_{i=1}^{N} P_{ij}P_{ki} \\
&= \sum_{k=1}^{N} \pi_{k} \sum_{i=1}^{N} P_{ki}P_{ij} \\
&= \sum_{k=1}^{N} \pi_{k} {(P^2)}_{kj} \\
&= {(\pi^{T}P^2)}_{j},
\end{aligned}
{% endkatex %}

A pattern is clearly developing. Mathematical Induction can be used to prove the distribution
of states at an arbitrary time {% katex %}t{% endkatex %} is given by,
{% katex display %}
P(X_t=x_j) = {(\pi^{T}P^t)}_{j}
{% endkatex %}

or as a column vector,

{% katex display %}
\pi_{t}^{T} = \pi^{T}P^t\ \ \ \ \ (5).
{% endkatex %}

Where {% katex %}\pi{% endkatex %} and {% katex %}\pi_t{% endkatex %} are the initial state distribution
and the distribution after {% katex %}t{% endkatex %} steps respectively.

## Equilibrium Transition Matrix

The probability of transitioning between two states {% katex %}x_i{% endkatex %} and
{% katex %}x_j{% endkatex %} in {% katex %}t{% endkatex %} time steps was previously shown to be
stochastic matrix {% katex %}P^t{% endkatex %} constrained by equation {% katex %}(3){% endkatex %}.
The equilibrium transition matrix is defined by,
{% katex display %}
P_{E} = \lim_{t\to\infty}P^{t}.
{% endkatex %}
This limit can be determined using
[Matrix Diagonalization](https://en.wikipedia.org/wiki/Diagonalizable_matrix).
The following sections will use this method to construct a form of
{% katex %}P_E{% endkatex %} that will easily allow evaluation equilibrium limit.

### Eigenvectors and Eigenvalues of the Transition Matrix

Matrix Diagonalization requires evaluation of the eigenvalues and eigenvectors, which are defined
by the solutions to the equation,
{% katex display %}
Pv = \lambda v\ \ \ \ \ (6),
{% endkatex %}
where {% katex %}v{% endkatex %} is the eigenvector and {% katex %}\lambda{% endkatex %} eigenvalue.
From equation {% katex %}(6){% endkatex %} it follows that,
{% katex display %}
\begin{aligned}
P^{t}v &= P^{t-1}(Pv)\\
&=P^{t-1}\lambda v\\
&=P^{t-2}(Pv)\lambda\\
&=P^{t-2}\lambda^{2}v \\
&\vdots\\
&=(Pv)\lambda^{t-1}\\
&=\lambda^{t}v.
\end{aligned}
{% endkatex %}

Since {% katex %}P^t{% endkatex %} is a stochastic matrix it satisfies equation {% katex %}(3){% endkatex %}.
As a result of these constraints the limit {% katex %}t\to\infty{% endkatex %} requires that,
{% katex display %}
\lim_{t\to\infty}P^{t}=\lim_{t\to\infty}\lambda^{t}v\ \leq\ \infty.
{% endkatex %}

To satisfy this constraint it is required that {% katex %}\lambda\ \leq\ 1 {% endkatex %}. Next,
it will be shown that {% katex %}\lambda_1=1{% endkatex %} and that a column vector of
{% katex %}1's{% endkatex %} with {% katex %}N{% endkatex %} rows,
{% katex display %}
V_1 =
\begin{pmatrix}
1 \\
1 \\
\vdots \\
1
\end{pmatrix}\ \ \ \ \ (7),
{% endkatex %}
are eigenvalue and eigenvector solutions of equation {% katex %}(6).{% endkatex %}
{% katex display %}
\begin{pmatrix}
P_{11} & P_{12} & \cdots & P_{1N} \\
P_{21} & P_{22} & \cdots & P_{2N} \\
\vdots & \vdots & \ddots & \vdots \\
P_{N1} & P_{N2} & \cdots & P_{NN}
\end{pmatrix}
\begin{pmatrix}
1 \\
1 \\
\vdots \\
1
\end{pmatrix}
=
\begin{pmatrix}
\sum_{j=1}^{N}P_{1j} \\
\sum_{j=1}^{N}P_{2j} \\
\vdots \\
\sum_{j=1}^{N}P_{Nj}
\end{pmatrix}
=
\begin{pmatrix}
1 \\
1 \\
\vdots \\
1
\end{pmatrix}
=\lambda_1 v_1,
{% endkatex %}

where use was made of the stochastic matrix condition from equation {% katex %}(1){% endkatex %}, namely,
{% katex %}\sum_{j=1}^{N}P_{ij}=1{% endkatex %}.

To go further a result from the [Perron-Frobenius Theorem](https://en.wikipedia.org/wiki/Perron–Frobenius_theorem) is needed.
This theorem states that a stochastic matrix will have a largest eigenvalue with multiplicity 1. Here all eigenvalues will satisfy
{% katex %}\lambda_1=1 \ >\ \mid{\lambda_i}\mid,\ \forall\ 1\ <\ i\ \leq\ N{% endkatex %}.

Denote the eigenvector {% katex %}V_j{% endkatex %} by the column vector,
{% katex display %}
V_j =
\begin{pmatrix}
v_{1j} \\
v_{2j} \\
\vdots \\
v_{Nj}
\end{pmatrix},
{% endkatex %}

and let {% katex %}V{% endkatex %} be the matrix with columns that are the eigenvectors of
{% katex %}P{% endkatex %} with {% katex %}V_1{% endkatex %} from equation
{% katex %}(7){% endkatex %} in the first column,
{% katex display %}
V=
\begin{pmatrix}
1 & v_{12} & \cdots & v_{1N} \\
1 & v_{22} & \cdots & v_{2N} \\
\vdots & \vdots & \ddots & \vdots \\
1 & v_{N2} & \cdots & v_{NN}
\end{pmatrix}.
{% endkatex %}

Assume that {% katex %}V{% endkatex %} is invertible and denote the inverse by,

{% katex display %}
V^{-1}=
\begin{pmatrix}
v^{-1}_{11} & v^{-1}_{12} & \cdots & v^{-1}_{1N} \\
v^{-1}_{21} & v^{-1}_{22} & \cdots & v^{-1}_{2N} \\
\vdots & \vdots & \ddots & \vdots \\
v^{-1}_{N1} & v^{-1}_{N2} & \cdots & v^{-1}_{NN}
\end{pmatrix}\ \ \ \ \ (8).
{% endkatex %}

If the identity matrix is represented by {% katex %}I{% endkatex %} then {% katex %}VV^{-1} = I{% endkatex %}.
Let {% katex %}\Lambda{% endkatex %} be a diagonal matrix constructed from the eigenvalues of
{% katex %}P{% endkatex %} using {% katex %}\lambda_1=1{% endkatex %},

{% katex display %}
\Lambda =
\begin{pmatrix}
1 & 0 & \cdots & 0 \\
0 & \lambda_2 & \cdots & 0 \\
\vdots & \vdots & \ddots & \vdots \\
0 & 0 & \cdots & \lambda_N
\end{pmatrix}.
{% endkatex %}

Sufficient information about the eigenvalues and eigenvectors has obtained to prove some very
general results for Markov Chains.
The following section will work through the equilibrium limit using the using these results
to construct a diagonalized representation of the matrix.

### Diagonalization of Transition Matrix

Using the results obtained in the previous section {% katex %}P{% endkatex %} the diagonalized
form of the transition matrix is given by,

{% katex display %}
P = V\Lambda V^{-1},
{% endkatex %}

Using this representation of {% katex %}P{% endkatex %} an expression for {% katex %}P^t{% endkatex %}
is obtained,
{% katex display %}
\begin{aligned}
P^{t} &= P^{t-1}V\Lambda V^{-1} \\
&= P^{t-2}V\Lambda V^{-1}V\Lambda V^{-1} \\
&= P^{t-2}V\Lambda^2 V^{-1}\\
&\vdots \\
&= PV\Lambda^{t-1} V^{-1} \\
&= V\Lambda^{t}V^{-1}
\end{aligned}
{% endkatex %}

Evaluation of {% katex %}\Lambda^{t}{% endkatex %} is straight forward,

{% katex display %}
\Lambda^t =
\begin{pmatrix}
1 & 0 & \cdots & 0 \\
0 & \lambda_2^t & \cdots & 0 \\
\vdots & \vdots & \ddots & \vdots \\
0 & 0 & \cdots & \lambda_N^t
\end{pmatrix}.
{% endkatex %}

Since {% katex %}\mid{\lambda_i}\mid\ <\ 1,\ \forall\ 1\ <\ i\ \leq\ N{% endkatex %} in the limit
{% katex %}t\to\infty{% endkatex %} it is seen that,
{% katex display %}
\Lambda^{\infty} =
\lim_{t\to\infty} \Lambda^t =
\lim_{t\to\infty}
\begin{pmatrix}
1 & 0 & \cdots & 0 \\
0 & \lambda_2^t & \cdots & 0 \\
\vdots & \vdots & \ddots & \vdots \\
0 & 0 & \cdots & \lambda_N^t
\end{pmatrix}
=
\begin{pmatrix}
1 & 0 & \cdots & 0 \\
0 & 0 & \cdots & 0 \\
\vdots & \vdots & \ddots & \vdots \\
0 & 0 & \cdots & 0
\end{pmatrix},
{% endkatex %}

so,
{% katex display %}
\lim_{t\to\infty} P^{t} = \lim_{t\to\infty} V\Lambda^{t} V^{-1} = V\Lambda^{\infty}V^{-1}.
{% endkatex %}

Evaluation of each of the first two terms on the righthand side gives,

{% katex display %}
V\Lambda^{\infty} =
\begin{pmatrix}
1 & v_{12} & \cdots & v_{1N} \\
1 & v_{22} & \cdots & v_{2N} \\
\vdots & \vdots & \ddots & \vdots \\
1 & v_{N2} & \cdots & v_{NN}
\end{pmatrix}
\begin{pmatrix}
1 & 0 & \cdots & 0 \\
0 & 0 & \cdots & 0 \\
\vdots & \vdots & \ddots & \vdots \\
0 & 0 & \cdots & 0
\end{pmatrix}
=
\begin{pmatrix}
1 & 0 & \cdots & 0 \\
1 & 0 & \cdots & 0 \\
\vdots & \vdots & \ddots & \vdots \\
1 & 0 & \cdots & 0
\end{pmatrix}.
{% endkatex %}

It follows that the entire expression on the righthand size is given by,
{% katex display %}
V\Lambda^{\infty} V^{-1} =
\begin{pmatrix}
1 & 0 & \cdots & 0 \\
1 & 0 & \cdots & 0 \\
\vdots & \vdots & \ddots & \vdots \\
1 & 0 & \cdots & 0
\end{pmatrix}
\begin{pmatrix}
v^{-1}_{11} & v^{-1}_{12} & \cdots & v^{-1}_{1N} \\
v^{-1}_{21} & v^{-1}_{22} & \cdots & v^{-1}_{2N} \\
\vdots & \vdots & \ddots & \vdots \\
v^{-1}_{N1} & v^{-1}_{N2} & \cdots & v^{-1}_{NN}
\end{pmatrix}
=
\begin{pmatrix}
v^{-1}_{11} & v^{-1}_{12} & \cdots & v^{-1}_{1N} \\
v^{-1}_{11} & v^{-1}_{12} & \cdots & v^{-1}_{1N} \\
\vdots & \vdots & \ddots & \vdots \\
v^{-1}_{11} & v^{-1}_{12} & \cdots & v^{-1}_{1N}
\end{pmatrix}.
{% endkatex %}

Finally, the equilibrium transition matrix is given by,
{% katex display %}
P_{E} = V\Lambda^{\infty} V^{-1} =
\begin{pmatrix}
v^{-1}_{11} & v^{-1}_{12} & \cdots & v^{-1}_{1N} \\
v^{-1}_{11} & v^{-1}_{12} & \cdots & v^{-1}_{1N} \\
\vdots & \vdots & \ddots & \vdots \\
v^{-1}_{11} & v^{-1}_{12} & \cdots & v^{-1}_{1N}
\end{pmatrix}\ \ \ \ \ (9).
{% endkatex %}

The rows of {% katex %}P_{E}{% endkatex %} are identical and given by the
first row of the inverse of the matrix of eigenvectors, {% katex %}V^{-1}{% endkatex %}
from equation {% katex %}(8){% endkatex %}. Since {% katex %}P^{\infty}{% endkatex %} is a transition
matrix it must satisfy,

{% katex display %}
\begin{gathered}
\sum_{j=1}^{N} v_{ij}^{-1}\ =\ 1\\
v_{ij}^{-1}\ \geq \ 0.
\end{gathered}
{% endkatex %}

## Equilibrium Distribution of States

The equilibrium distribution of states is defined by a solution to equation {% katex %}(5){% endkatex %}
that is independent of time.

{% katex display %}
\pi^{T} = \pi^{T}P^t\ \ \ \ \ (10).
{% endkatex %}

Consider a distribution {% katex %}\pi_{E}{% endkatex %} that satisfies,
{% katex display %}
\pi_{E}^{T} = \pi_{E}^{T}P\ \ \ \ \ (11).
{% endkatex %}

It is easy to show that {% katex %}\pi_{E}{% endkatex %} is the equilibrium solution
by substituting it into equation {% katex %}(10){% endkatex %}.
{% katex display %}
\begin{aligned}
\pi_{E}^{T} &= \pi_{E}^{T}P^t \\
&= (\pi_{E}^{T}P)P^{t-1} \\
&= \pi_{E}^{T}P^{t-1} \\
&= \pi_{E}^{T}P^{t-2} \\
&\vdots \\
&= \pi_{E}^{T}P \\
&= \pi_{E}^{T}.
\end{aligned}
{% endkatex %}

### Relationship Between Distribution of States and Transition Matrix

The relationship between {% katex %}\pi_E{% endkatex %} and {% katex %}P_E{% endkatex %}
will be determined in this section. Begin, by considering an arbitrary initial distribution states
with {% katex %}N{% endkatex %} elements,
{% katex display %}
\pi =
\begin{pmatrix}
\pi_{1} \\
\pi_{2} \\
\vdots \\
\pi_{N}
\end{pmatrix}.
{% endkatex %}

The distribution when the Markov Chain has had sufficient time to reach equilibrium will be given by,
{% katex display %}
\begin{aligned}
\pi^{T}P_{E} &=
\begin{pmatrix}
\pi_{1} & \pi_{2} & \cdots & \pi_{N}
\end{pmatrix}
\begin{pmatrix}
v^{-1}_{11} & v^{-1}_{12} & \cdots & v^{-1}_{1N} \\
v^{-1}_{11} & v^{-1}_{12} & \cdots & v^{-1}_{1N} \\
\vdots & \vdots & \ddots & \vdots \\
v^{-1}_{11} & v^{-1}_{12} & \cdots & v^{-1}_{1N}
\end{pmatrix} \\
&=
\begin{pmatrix}
v^{-1}_{11}\sum_{j=1}^{N} \pi_{j} &
v^{-1}_{12}\sum_{j=1}^{N} \pi_{j} &
\cdots &
v^{-1}_{1N}\sum_{j=1}^{N} \pi_{j}
\end{pmatrix},
\end{aligned}
{% endkatex %}

recall that {% katex %}\sum_{j=1}^{N} \pi_{j} = 1{% endkatex %}, so

{% katex display %}
\pi^{T}P_{E} =
\begin{pmatrix}
v^{-1}_{11} & v^{-1}_{12} & \cdots & v^{-1}_{1N}
\end{pmatrix}.
{% endkatex %}

Thus any initial distribution of states {% katex %}\pi{% endkatex %} will after
sufficient time approach the distribution above. It follows that it will be the solution
of equation {% katex %}(11){% endkatex %} which defines the equilibrium distribution of states,

{% katex display %}
\pi_E =
\begin{pmatrix}
v^{-1}_{11} \\
v^{-1}_{12} \\
\vdots \\
v^{-1}_{1N}
\end{pmatrix}.
{% endkatex %}

### Solution of Equilibrium Equation

An equation for the equilibrium distribution of states, {% katex %}\pi_{E}{% endkatex %}, can
be obtained from equation {% katex %}(11){% endkatex %},
{% katex display %}
\pi^{T}_E\left(P - I\right) = 0\ \ \ \ \ (12),
{% endkatex %}
where {% katex %}I{% endkatex %} is the identity matrix. Equation {% katex %}(12){% endkatex %} alone
is insufficient to obtain a unique solution since the system of linear equations if defines is
[Linearly Dependent](https://en.wikipedia.org/wiki/Linear_independence). In a linearly dependent
system of equations some equations are the result of linear operations on the other equations.
It is straight forward to show that one of the equations defined by {% katex %}(12){% endkatex %} can
be eliminated by summing the other equations and multiplying by {% katex %}-1{% endkatex %}.
If the equations were
linearly independent the only solution would be the trivial zero solution,
{% katex %}{\left( \pi^{T}_E \right)}_{i}\ =\ 0\ \forall\ i{% endkatex %}. A unique solution to
{% katex %}(12){% endkatex %} is obtained by including the normalization constraint,
{% katex display %}
\sum_{j=1}^{N} {\left( \pi_{E}^{T} \right)}_{j} = 1\ \ \ \ \ (13).
{% endkatex %}

## Example

Consider the Markov Chain defined by the transition matrix,
{% katex display %}
P =
\begin{pmatrix}
0.0 & 0.9 & 0.1 & 0.0 \\
0.8 & 0.1 & 0.0 & 0.1 \\
0.0 & 0.5 & 0.3 & 0.2 \\
0.1 & 0.0 & 0.0 & 0.9
\end{pmatrix}.
{% endkatex %}
The state transition diagram below provides a graphical representation of {% katex %}P{% endkatex %}.
<div style="text-align:center;">
  <img src="/assets/posts/discrete_state_markov_chain_equilibrium/transition_diagram.png">
</div>

### Convergence to Equilibrium

Relaxation of both the transition matrix and distribution of states to their equilibrium values
is easily demonstrated with the few lines of Python shown below.

```shell
In [1]: import numpy
In [2]: t = [[0.0, 0.9, 0.1, 0.0],
   ...:      [0.8, 0.1, 0.0, 0.1],
   ...:      [0.0, 0.5, 0.3, 0.2],
   ...:      [0.1, 0.0, 0.0, 0.9]]
In [3]: p = numpy.matrix(t)
In [4]: p**100
Out[4]:
matrix([[0.27876106, 0.30088496, 0.03982301, 0.38053097],
        [0.27876106, 0.30088496, 0.03982301, 0.38053097],
        [0.27876106, 0.30088496, 0.03982301, 0.38053097],
        [0.27876106, 0.30088495, 0.03982301, 0.38053098]])
```

Here the transition matrix from the initial state to states {% katex %}100{% endkatex %} time steps
in the future is computed using equation {% katex %}(2){% endkatex %}. The result obtained is a matrix
with all rows equal. This agrees with the result obtained in the previously for the limit
{% katex %}t\to\infty{% endkatex %} in equation {% katex %}(9){% endkatex %}.
For an initial distribution of states the distribution after {% katex %}100{% endkatex %}
steps is given by,

```shell
In [5]: c = [[0.1],
   ...:      [0.5],
   ...:      [0.35],
   ...:      [0.05]]
In [6]: π = numpy.matrix(c)
In [8]: π.T*p**100
Out[8]: matrix([[0.27876106, 0.30088496, 0.03982301, 0.38053097]])
```

An initial distribution of states is constructed that satisfies
{% katex %}\sum_{i=0}^3 \pi_i = 1{% endkatex %}. Then equation {% katex %}(5){% endkatex %} is used
to compute the future distribution. The result is as expected from the previous analysis for
sufficiently long times, namely, the limiting distribution is the repeated row of the limiting
transition matrix.

The plot below illustrates the convergence of the distribution of states from the previous example.
In the plot the components of {% katex %}\pi_t{% endkatex %} from equation {% katex %}(5){% endkatex %}
are plotted for each time step. The convergence to the limiting value occurs rapidly. Within only
{% katex %}20{% endkatex %} steps {% katex %}\pi_t{% endkatex %} has reached limiting distribution.

<div style="text-align:center;">
  <img src="/assets/posts/discrete_state_markov_chain_equilibrium/distribution_relaxation_1.png">
</div>

### Equilibrium Transition Matrix

### Equilibrium Distribution of States

### Simulation
