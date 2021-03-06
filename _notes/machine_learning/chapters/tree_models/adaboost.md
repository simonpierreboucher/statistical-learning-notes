---
title: "Adaboost.M1"
---

## Adaboost.M1

The convention is $N$ is the number of training examples and $M$ is the number of trees.

We consider the two class problem for simpler analysis, and denote them by $Y \in \{-1, 1 \}$. The error rate is defined as
\begin{align}
        \overline{err} = \sum_{i=1}^{N} I(y_{i} \neq G(x_{i}))
    \end{align}
and if the error rate is near $0.5$, then the classifier is no better than a random guess.


Boosting builds trees in a sequential manner, and outputs of all the trees are weighted to get the final output from the classifier
\begin{align}
         y = \sum_{m=1}^{M} \alpha_{m}G_{m}(x)
    \end{align}
where we build a total of $M$ trees and $G_{m}$ is a weak clasifier.


At each step, data weights are recalculated. Observations misclassified at the last step receive higher weight and vice versa. To start off, all observations receive the same weight $1/N$.

### Basic Algorithm

1.  Initialize the weights of all observations as $w_{i} = \cdots = w_{n} = 1/N$

2.  For $m = 1$ to $M$

    1.  Fit a classifier $G_{m}(x)$ to the training data with weights $w_{i}$

    2.  Compute weighted error
        \begin{align}
                        err_{m} = \frac{\sum_{i=1}^{N} w_{i} I(y_{i} \neq G_{m}(x_{i}))}{\sum_{i=1}^{N} w_{i}} \\quad \text{(1)}
                    \end{align}

    3.  Compute tree weight
        \begin{align}
                        \alpha_{m} = log \left( \frac{1 - err_{m}}{err_{m}} \right) \quad \text{(2)}
                    \end{align}

    4.  Update the observation weights as
        \begin{align}
                        w_{i} \leftarrow w_{i} \cdot exp(\alpha_{m} \cdot I(y_{i} \neq G_{m}(x)), i = 1, \ldots, n  \quad \text{(3)}
                    \end{align}

3.  Output the final classifier output $\sum_{m=1}^{M} \alpha_{m}G_{m}(x)$

Note that the algorithm here returns discrete classes and is called _Discrete Adaboost_.

### Forward Stagewise Additive Modelling

The boosting algorithm is one solution to a more general set of problems
\begin{align}
        \minimize_{\beta_{1}, \ldots, \beta_{m}, \gamma_{1}, \ldots, \gamma_{m}} \sum_{i=1}^{N} L \left( y_{i}, \sum_{m=1}^{M} \beta_{m} b(x_{i}; \gamma_{m}) \right)
    \end{align}
where $L(y, f(x))$ is a loss function (log likelihood or squared error) averaged over the data and $b(x; \gamma_{m})$ is a basis function that maps the input vector to a scalar. $\gamma_{m}$ is a set of parameters, which can be parameters of a decision tree like depth, number of nodes and samples in a node.


The above loss function is difficult to directly minimize for a set of basis functions. Instead, the simpler problem to solve is
\begin{align}
        \minimize_{\beta, \gamma} L \left( y, \beta \cdot b(x; \gamma) \right)
    \end{align}
The general algorith then is

1.  Initialize $f_{0}(x) = 0$

2.  for $m = 1$ to $M$

    1.  Compute the parameters
        \begin{align}
                        \beta_{m}, \gamma_{m} = \argmin_{\beta, \gamma} \sum_{i=1}^{N} L(y_{i}, f_{m-1}(x_{i}) + \beta b(x_{i}; \gamma))
                    \end{align}

    2.  Update $f_{m}(x) = f_{m-1}(x) + \beta_{m}b(x;\gamma_{m})$

In the case of regression, the above formulation with least squares loss becomes
\begin{align}
        L = \sum_{i=1}^{N} (y_{i} - f_{m-1}(x_{i}) + \beta b(x_{i}; \gamma))^{2}
        = \sum_{i=1}^{N} (r_{i,m} + \beta b(x_{i}; \gamma))^{2}
    \end{align}
which means we are fitting the new basis function on the residuals of the previous formulation. Though this loss is good for regression, we need different loss function for a classification problem.

### Exponential Loss

Using the class indicators as $Y \in \{-1, 1 \}$, we show that AdaBoost.M1 uses exponential loss to build the stagewise additive model
\begin{align}
        L(y, f(x)) &= exp(-y \cdot f(x))\newline
        \beta_{m}, G_{m} &= \argmin_{\beta, G} \sum_{i=1}^{N} exp(-y_{i}(f_{m-1}(x_{i}) + \beta G(x_{i})))\newline
        &= \argmin_{\beta, G} \sum_{i=1}^{N} w_{i}^{(m)} exp(-\beta y_{i} G(x_{i}))\newline
        \text{with} \quad w_{i}^{(m)} &= exp(-y_{i} f_{m-1}(x_{i})) \quad \text{independent of } \beta \text{ and } G(x) \quad \text{(4)}
    \end{align}
The weights keep changing with each iteration. We can rewrite the last equation as

\begin{align}
        \beta_{m}, G_{m} &= \argmin_{\beta, G} \sum_{i=1}^{N} w_{i}^{(m)} exp(-\beta y_{i} G(x_{i}))\newline
        &= \argmin_{\beta, G} e^{-\beta} \cdot \sum_{y_{i} = G(x_{i})} w_{i}^{(m)} + e^{\beta}  \cdot\sum_{y_{i} \neq G(x_{i})} w_{i}^{(m)}\newline
        &= \argmin_{\beta, G} e^{-\beta} \sum_{i=1}^{N} w_{i}^{(m)} (1 - I(y_{i} \neq G(x_{i}))) + e^{\beta} \sum_{i=1}^{N} w_{i}^{(m)} I(y_{i} \neq G(x_{i}))\newline
        &= \argmin_{\beta, G} (e^{\beta} - e^{-\beta}) \sum_{i=1}^{N} w_{i}^{(m)} I(y_{i} \neq G(x_{i})) + e^{-\beta} \sum_{i=1}^{N} w_{i}^{(m)}
    \end{align}
\begin{align}
        \text{Additionally, } \quad G_{m}(x) &= \argmin_{G(x)} \sum_{i=1}^{N} w_{i}^{(m)} I(y_{i} \neq G(x_{i}))\newline
        \text{Substituiting,}\quad \beta_{m} &= \argmin_{\beta} (e^{\beta} - e^{-\beta}) \sum_{i=1}^{N} G_{m}(x_{i}) + e^{-\beta} \sum_{i=1}^{N} w_{i}^{(m)}\newline
        \text{Differentiating,} \quad \beta_{m} &= \frac{1}{2}\log \left( \frac{\sum_{i=1}^{N}w_{i}}{\sum_{i=1}^{N}w_{i}I(y_{i} \neq G_{m}(x_{i}))} - 1 \right)\newline
        &= \frac{1}{2} \log \left( \frac{1 - err_{m}}{err_{m}} \right) \quad \text{from (1)}
    \end{align}
Hence, the recursive $f(x)$ and $w(x)$ update becomes
\begin{align}
        f_{m}(x) &= f_{m-1}(x) + \beta_{m} G_{m}(x)\newline
        w_{i}^{(m+1)} &= exp(-y_{i} f_{m}(x_{i})) =  exp(-y_{i} (f_{m-1}(x) + \beta_{m} G_{m}(x_{i})))\newline
        &= w_{i} exp(-y_{i}\beta_{m}G_{m}(x_{i})) \quad \text{from (4)}\newline
        \text{Also, } \quad \alpha_{m} &= 2\beta_{m} \quad \text{from (2) and (1)}\newline
        \text{and} \quad I(y_{i} \neq G(x_{i})) &= \frac{1 - y_{i}G(x_{i})}{2}\newline
        w_{i}^{(m+1)} &= w_{i} e^{-\beta_{m}} e^{\alpha I(y_{i} \neq G_{m}(x_{i}))}
    \end{align}

which is similar to the form obtained in equation (3) with an added constant $exp(-\beta_{m})$ same across all the data points and hence makes no difference. The probability of prediction of the classes then becomes
\begin{align}
        P(Y=1|X=x) = \frac{exp(f(x))}{exp(-f(x)) + exp(f(x))} = \frac{1}{1+exp(-2f(x))}\newline
        \text{and} \quad sign(f(x)) = sign \left(\frac{1}{2}\log \left(\frac{P(Y=1|X=x)}{P(Y=-1|X=x)} \right)\right)
    \end{align}
meaning the output that is the sign of the function is sign of log likelihood and thus justified.
