---
title: "Variational Inference"
---

## Variational Inference

The posterior in Bayesian Models with latent variables is quite hard to compute if we do not choose a conjuagte prior for the likelihood function. However, we need to have some approximate version of the posterior in order to make inferences later on. One approach to approximate posterior is to find a distribution $q \in Q$ ($Q$ is a family of distributions) that can minimize the KL divergence
\begin{align}
    p(z|X) \approx \min_{q \in Q} \KL{q(z)}{p(z|X)}\end{align}
where $z$ is the latent variable and $X$ is the data
And we do not need to worry about the normalization constant $Z$ ($p(X)$)
\begin{align}
    \KL{q(z)}{p(z|X)} &= \int_{z} q(z) log\frac{q(z)}{p(X|z)p(z)/Z}\newline
    &= \int_{z} q(z) log\frac{q(z)}{p(X|z)p(z)}  + log(Z)\newline
    p(z|X) &\approx \min_{q \in Q} \KL{q(z)}{p(X|z)p(z)} \quad (p(X|z)p(z) = p^{\*}(z))\end{align}
since $\int_{z} q(z) dz = 1$. We only need to work with likelihood and prior in this case.

#### E-step in EM

utilizes the variational inference technique. The E-step requires us to calculate the posterior of the latent variables, which can be hard to compute. VI can help us approximate that distribution assuming that we restrict ourselves to a family of distributions.
\begin{align}
    q(z) = p(z|x, \theta) \approx \min_{q \in Q}KL{q(z)}{p^{\*}(z)}\end{align}
This is called Variational EM.

### Mean Field Approximations

This is a Variational Inference method where we assume the distribution $q$ to be factorized over the latent variables across all dimensions $d$, i.e.,
\begin{align}
    Q = \\{q | q(z) = \prod_{i=1}^{d}q_{i}(z_{i}) \\}\newline
    q(z) = \argmin_{q \in Q} \KL{\prod_{i=1}^{d}q_{i}(z_{i})}{p^{\*}(z)}\end{align}
and we minimize the KL divergence using coordinated gradient descent. First find the minima for $q_{1}$ keeping everything else fixed, then $q_{2}$, and so on. We will repeat this loop until convergence.


Since we are minimizing over one component at a time, the functional form of any $q_{k}(z_{k})$ can be derived as follows
\begin{gather}
    \min_{q_{k}} \KL{\bigg(\prod_{j=1}^{d}q_{j} \bigg)(z_{j})}{p^{\*}(z)} = \min_{q_{k}} \int \bigg(\prod_{j=1}^{d}q_{j}(z_{j}) \bigg) log \frac{\prod_{j=1}^{d}q_{j}(z_{j})}{p^{\*}(z)} dz \newline
    = \min_{q_{k}} \bigg\\{ \sum_{i=1}^{d} \int \bigg(\prod_{j=1}^{d} q_{i}(z_{i}) \bigg) log(q_{i}(z_{i})) dz - \int \bigg(\prod_{j=1}^{d}q_{j}(z_{j})\bigg) log(p^{\*}(z)) dz \bigg\\}\newline
    = \min_{q_{k}} \bigg\\{ \int \bigg(q_{k}(z_{k})log(q_{k}(z_{k})) \bigg( \int \prod_{j=1, j\neq k}^{d} q_{j}(z_{j}) dz_{\neq j} \bigg) dz_{k} \bigg)\newline + \sum_{i=1, i\neq k}^{d} \int \bigg( q_{i}(z_{i})log(q_{i}(z_{i})) \bigg( \int \prod_{j=1,j \neq i}^{d}q_{j}(z_{j}) dz_{\neq i} \bigg) dz_{i}\bigg) - \int \bigg(\prod_{j=1}^{d}q_{j}(z_{j})\bigg) log(p^{\*}(z)) dz \bigg\\}\newline
    \text{Note that} \quad \int \prod_{j=1}^{d} q_{j}(z_{j}) dz = \int \bigg(\prod_{j=1}^{d} \bigg) q_{j}(z_{j}) dz_{1} dz_{2}\ldots dz_{d} = \prod_{j=1}^{d} \bigg( \int q_{j}(z_{j}) dz_{j}\bigg) = 1\newline
    \min_{q_{k}} \bigg\\{ \int q_{k}(z_{k})log(q_{k}(z_{k})) dz_{k} + \sum_{i=1, i\neq k}^{d} q_{i}(z_{i})log(q_{i}(z_{i})) dz_{i} \quad \newline \quad  -\int q_{k}(z_{k}) \bigg(\bigg(\prod_{j=1,j\neq k}^{d}q_{j}(z_{j})\bigg) log(p^{\*}(z)) dz_{\neq j} \bigg) dz_{k} \bigg\\}\newline
    = \min_{q_{k}} \bigg\\{ \int q_{k}(z_{k}) \bigg[ log(q_{k}(z_{k})) - \bigg(\bigg(\prod_{j=1,j\neq k}^{d}q_{j}(z_{j})\bigg) log(p^{\*}(z)) dz_{\neq j} \bigg) \bigg] dz_{k} \bigg\\}\newline
    \text{because} \quad \sum_{i=1, i\neq k}^{d} q_{i}(z_{i})log(q_{i}(z_{i})) dz_{i} \quad \text{is constant wrt $q_{k}$}
\end{gather}

We now make three observations

1.  adding a constant to the minimization equation will still give the same result

2.  $\int q_{k}(z_{k}) dz_{k} = 1$ because $q_{k}$ is a probability distribution

3.  $\int \left(\prod_{j=1,j\neq k}^{d}q_{j}(z_{j})\right) log(p^{\*}(z)) dz_{\neq j} = E_{q_{-k}}[log(p^{\*}(z))]$
    since $\prod_{j=1,j\neq k}^{d}q_{j}(z_{j})$ is a valid probability distribution. Notice that the expectation integrates over all $z$ except $z_{k}$ and thus is a function of just $z_{k}$. $q_{-k}$ in the expectation just means that we are considering the product $\prod_{j=1,j\neq k}^{d} q_{j}(z_{j})$ as the probability distribution. We convert this expectation to a positive value by taking the exponent, and then get a valid probability distribution as
    \begin{align}
            t(z_{k}) = \frac{exp(E_{q_{-k}}[log(p^{\*}(z))])}{\int exp(E_{q_{-k}}[log(p^{\*}(z))]) dz_{k}}
        \end{align}
    The denominator of this expression is a constant which we can introduce in the equation as is without any alteration

Rewriting the minimization equation so far
\begin{gather}
    \min_{q_{k}} \left\\{\int q_{k}(z_{k}) \left[ log(q_{k}(z_{k})) - log(exp(E_{q_{-k}}[log(p^{\*}(z))])) \right] dz_{k} \quad\newline \quad+ \left( \int exp(E_{q_{-k}}[log(p^{\*}(z))]) dz_{k} \right) \int q_{k}(z_{k}) dz_{k} \right\\}\newline
    = \min_{q_{k}} \left\\{ \int q_{k}(z_{k}) log(q_{k}(z_{k})) - log\frac{exp(E_{q_{-k}}[log(p^{\*}(z))])}{\int exp(E_{q_{-k}}[log(p^{\*}(z))]) dz_{k}} \right\\}
    = \min_{q_{k}}\left\\{\int q_{k}(z_{k}) log\frac{q_{k}(z_{k})}{t(z_{k})}  \right\\}\newline
    \min_{q_{k}} \KL{\left(\prod_{j=1}^{d}q_{j} \right)(z_{j})}{p^{\*}(z)} = \min_{q_{k}} \KL{q_{k}(z_{k})}{t(z_{k})}\newline
\end{gather}
and we know that KL divergence is minimized when the two functions conincide, i.e.
\begin{align}
    q_{k}(z_{k}) = t(z_{k}) = \frac{exp(E_{q_{-k}}[log(p^{\*}(z))])}{\int exp(E_{q_{-k}}[log(p^{\*}(z))]) dz_{k}}\newline
    \implies log(q_{k}(z_{k})) = E_{q_{-k}}[log(p^{\*}(z))] - constant
\end{align}
which is the expectation of the posterior on $z$ over $q$ without the current component being minimized.
