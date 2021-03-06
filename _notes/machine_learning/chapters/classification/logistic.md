---
title: "Logistic Regression"
---

## Logistic Regression

Modelling binary response with linear regression might produce values outside the range $[0,1]$ ( and possibly negative as well). Hence we use a logistic function to compress the outputs to $[0,1]$ range.

\begin{align}
        p(Y=1 \vert X) = \frac{1}{1+e^{-(\beta_{0}+\beta_{1}X)}}\newline
        odds = \frac{p(Y=1 \vert X)}{1-p(Y=1 \vert X)} = e^{\beta_{0} + \beta_{1}X}
    \end{align}

-   The solution to this model is obtained via **Maximum Likelihood Estimation**.

-   Odds are also used to interpret probability. A low value of odds (close to $0$) indicates a low probability while a high value (close to $\inf$) indicates a high probability.

-   One unit change in $X$ will cause $\beta_{1}$ change in the $log\;odds$.

### Loss Function

**Maximum Likelihood** is used to determine the coefficients. Basic intuition is to choose such a pair of $\beta's$ that will make the predicted probability as close to the correct binary response ($0$ or $1$) as possible.

\begin{align}
        \text{Denoting} \quad p(x_{i}) &= P(Y = 1 \vert X = x_{i}) = 1/(1 + exp(-\beta^{T}x_{i}))\newline
        \text{likelihood function} &= l(\beta_{0},\beta_{1}) = \prod_{i:y_{i}=1} p(x_{i}) \prod_{i^{'}:y_{i^{'}}=0}(1-p(x_{i^{'}}))\newline
        \text{Taking logarithm, } logloss &= \sum_{i:y_{i}=1} \log p(x_{i}) + \sum_{i^{'}:y_{i^{'}}=0}\log (1-p(x_{i^{'}}))\newline
        &= \sum_{i=1}^{n} y\log p(x_{i}) + (1-y)\log (1+p(x_{i})) \tag\*{since $y = 0$ or $1$}\newline
        &= \sum_{i=1}^{n} y\beta^{T}x_{i} - log(1 + exp(\beta^{T}x_{i}))
    \end{align}
All the formulae listed here and above extend for the case of multiple variables, wherein we simply replace the sum $\beta_{0} + \beta_{1}X$ with $\beta_{0} + \beta_{1}X_{1} + \cdots + \beta_{p}X_{p}$.


For the below derivations, refer to the [appendix]({{ "/notes/machine_learning/chapters/appendix/matrix_logistic_reg.html" | relative_url }}) for complete matrix calculus
To solve for the $\beta$s, we consider the derivate of the _log likelihood_
\begin{align}
        \frac{\partial l}{\partial \beta} = \sum_{i=1}^{n} x_{i}(y_{i} - p_{i}) = 0
    \end{align}
which are $p-1$ non linear equations. We use the Newton-Raphson method and Hessian matrix for solving them
\begin{align}
        \frac{\partial^{2}l}{\partial \beta \partial \beta^{T}} &= -\sum_{i=1}^{n} x_{i}x_{i}^{T}p(x_{i})(1 - p(x_{i}))\newline
        \beta^{new} &= \beta^{old} - \bigg( \frac{\partial^{2}l}{\partial \beta \partial \beta^{T}} \bigg)^{-1} \frac{\partial l}{\partial \beta}
    \end{align}

Converting the above equations to function form for ease of obtaining solution
\begin{align}
        \frac{\partial l}{\partial \beta} &= X^{T}(y - \mathbf{p})\newline
        \frac{\partial^{2}l}{\partial \beta \partial \beta^{T}} &= -X^{T}WX\newline
        \text{where} \quad W &= diag([p(x_{1})(1-p(x_{1})), \ldots, p(x_{n})(1-p(x_{n}))])
    \end{align}

and the Newton-Raphson update becomes
\begin{align}
        \beta^{new} &= \beta^{old} + (X^{T}WX)^{-1} X^{T}(y - \mathbf{p})\newline
        &= (X^{T}WX)^{-1} X^{T}W(X\beta^{old} + W^{-1}(y - \mathbf{p}))\newline
        &= (X^{T}WX)^{-1} X^{T}Wz
    \end{align}
where $z$ is called the adjusted response (target in regression). The equation is same as the closed form solution for linear regression, with an added weight term of $W$. Hence, this equation solves the weighted least squares problem
\begin{align}
        \beta^{new} = \argmin_{\beta} (z - X\beta)^{T}W(z - X\beta)
    \end{align}
and is known as _iteratively reweighted least squares_ (IRLS). The equation converges since _log likelihood_ is concave. In case of non convergence/huge jumps, reducing the step size helps.


Further, the above formulation allows us to get the distribution of $\beta$
\begin{align}
        \hat{\beta} \sim \mathcal{N}(\beta, (X^{T}WX)^{-1})
    \end{align}

Wald test, rao score test


In case of multiple classes, $y$ becomes a matrix of shape $n \times K$ and $W$ is non-diagonal, but the solution to IRLS is not simple. Methods like co-ordinate wise gradient descent works better.


### Deviance

LL denotes the log likelihood.

Deviance is a goodness of fit statistic and commonly employed for generalized linear models. It is a replacement of RSS in the context of maximum likelihood. Deviance is a bivariate function and satisfies $d(y,y) = 0$ and $d(y, u) > 0 \; \forall \; y \neq u$. For the total deviance $D(y, \hat{y})$ over a set of data with observed response $y$ and predicted response $y$, we sum up the individual deviances, $D(y, \hat{y}) = \sum_{i} d(y_{i}, \hat{y}\_{i})$.


The total deviance can be written in the log likelihood form
\begin{align}
        D(y, \hat{y}) &= -2(LL(\text{fitted model}) - LL(\text{saturated model})))\newline
                      &= -2 \times LL(\text{fitted model}) \quad \text{for logistic regression}
    \end{align}

where $\theta_{0}$ are the parameters of the fitted model, and $\theta_{s}$ are the parameters of the saturated model. A saturated model is the one where we have different parameters for each of the observations so that the model is fit exactly. $p$ is the joint probability.


##### Nested Models

is an important concept required to understand saturated models. A model is said to be nested within another model if the first model can be obtained by putting constraints on the parameters of the second model. For instance, a gaussian with $0$ mean is nested within any gaussian of single variable. Similarly, the regression $\beta_{0} + \beta_{1}x$ is nested within $\beta_{0} + \beta_{1}x + \beta_{2}x^{2}$.


Thus, the saturated model is an higher dimensional/unconstrainted version of the fitted model, and they belong to the same family of models. If we further constraint the fitted model to have a single parameter, we will call it the null model.


##### Null and Residual Deviance

are often discussed in the case of logistic regression and will be reported by a statistical software.
\begin{align}
         \text{Null Deviance} \quad &= \quad -2(LL(\text{fitted model}) - LL(\text{null model}))\newline
         \text{Residual Deviance} \quad &= \quad -2(LL(\text{fitted model}) - LL(\text{saturated model}))
    \end{align}

The coefficient of 2 allows both the deviances to have a $\chi^{2}$ distribution. Thus, the deviances can be used to calculate the *p-values* and decide whether the deviances are strongly evidenced by the data or not.


For logistic regression, the null model is an intercept only model
\begin{align}
        \text{Null Model}\; \rightarrow P(Y=1 \vert X) = \frac{1}{1 + exp(-\beta_{0})}\newline
        \beta_{0} = log \big( \frac{\text{count of 1s}}{\text{count of 0s}} \big), \quad p = \frac{1}{1 + exp(-\beta_{0})} = \text{fraction of 1s}
    \end{align}
where the $\beta_{0}$ can be intuitively obtained by setting the probabilitiy equal to fractions of 1s, or maximising the log likelihood.


The saturated model on the other hand has individual parameters for each observations and makes the perfect predictions. For this model, the likelihood is simply 1 and the LL becomes 0.

##### $\mathbf{R^{2}}$

is also defined in the context of LL as
\begin{align}
        R^{2} = \frac{LL(\text{fitted model}) - LL(\text{null model})}{LL(\text{saturated model}) - LL(\text{null model})}
    \end{align}

the numerator measures how better the model is over null model, and the denominator helps scale the $R^{2}$ between 0 (null model) and 1 (perfect model). Although very similar to $R^{2}$ from linear regression, this is defined slightly differently.

### Multiple Classes

Note that the log of odds formula above is the ratio of probability of $Y=1$ to probability of $Y=0$. Here, $Y=0$ can be seen as a reference class. Similarly, in the case of $K$ classes, we will have $K-1$ classifiers, each classifying with respect to the last class (since the decision boundary has to be between two classes) and will take the form
\begin{align}
        log \bigg( \frac{P(Y=1 \vert X=x)}{P(Y=K \vert X=x)} \bigg) &= \beta_{1,0} + \beta_{1}^{T}x\newline
        log \bigg( \frac{P(Y=2 \vert X=x)}{P(Y=K \vert X=x)} \bigg) &= \beta_{2,0} + \beta_{2}^{T}x\newline
        \vdots\newline
        log \bigg( \frac{P(Y=K-1 \vert X=x)}{P(Y=K \vert X=x)} \bigg) &= \beta_{K-1,0} + \beta_{K-1}^{T}x\newline
        \text{and} \quad \sum_{k=1}^{K} P(Y=k \vert X=x) &= 1\newline
        \text{Giving} \quad P(Y=k \vert X=x) &= \frac{exp(\beta_{k,0} + \beta_{k}^{T}x)}{1 + \sum_{j=1}^{K-1} exp(\beta_{j,0} + \beta_{j}^{T}x)} \quad \text{for $j<K$}\newline
        P(Y=K \vert X=x) &= \frac{1}{1 + \sum_{j=1}^{K-1} exp(\beta_{j,0} + \beta_{j}^{T}x)}\newline
        \text{Parameter Set} \quad \theta &= \{ \beta_{1,0}, \beta_{1}, \ldots, \beta_{K-1,0}, \beta_{K-1} \}
    \end{align}
