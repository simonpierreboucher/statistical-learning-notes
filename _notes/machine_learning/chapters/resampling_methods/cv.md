---
title: "Cross Validation"
---

## Cross Validation

Validation set approach forms the bedrock for cross validation. We randomly split our training data into two sets, a training set and a validation set. We fit our models on the training set, and the validation set will serve as the unseen data. We can evaluate different models on the validation test to judge which of them performs the best.

A small problem with this approach is that the validation error will depend on the split of the data, i.e., we can expect slighlty different validation errors based on which subset of data we train. Hence, it is better to do this sampling multiple times in order to confidently select models and report test errors.

Furthermore, by preparing a validation set, we are reducing the size of our training set. Larger training data will usually result in better models. Hence, we might be overestimating the test errors in this case. A simple way to avoid this problem is to choose small sizes of the validation set and do the tests multiple times.

### Leave One Out CV

An extension of the validation approach, here we train the data on all but on example. This way, if the data has $n$ examples, we build $n$ models (each trained on $n-1$ examples) and the error is
\begin{align}
        \text{test error } = \frac{1}{n}\sum_{i=1}^{n} error_{i}\newline
    \end{align}
where $error_{i}$ is the error on $i^{th}$ observation from the model trained on remaining $n-1$ observations


Computing this can be extremely expensive when $n$ is large. For linear regression, there exists a trick by which the time taken to get $test\;error$ is exactly the same as the time to fit a single model on entire data set !

\begin{align}
        \hat{\beta} &= (X^{T}X)^{-1}X^{T}Y \newline
        \text{Hat matrix  } H &= X(X^{T}X)^{-1}X^{T}
    \end{align}

Let $X_{i}$ denote the matrix $X$ but with the $i^{th}$ row removed, and similarly $Y_{i}$. Let $x_{i}^{T}$ denote the $i^{th}$ row of $X$ and $h_{i}$ be the diagonal entry of $H$. Then we have the following
\begin{align}
        X_{i}^{T}X_{i} &= X^{T}X - x_{i}x_{i}^{T}\newline
        X_{i}^{T}Y &= X^{T}Y - x_{i}y_{i}\newline
        h_{i} &= x_{i}^{T}(X^{T}X)^{-1}x \newline
        \hat{\beta_{i}} &= (X_{i}^{T}X_{i})^{-1}X_{i}^{T}Y_{i} \newline
        e_{i} &= y_{i} - x_{i}^{T}\hat{\beta_{i}}
    \end{align}

Also, we have the Sherman--Morrison formula for calculating the inverse of a perturbated matrix using the original matrix. Let $A$ be the original invertible square matrix and $u,v$ be column vectors. Then,
\begin{align}
        (A + uv^{T})^{-1} = A^{-1} - \frac{A^{-1}uv^{T}A^{-1}}{1+v^{T}A^{-1}u}
    \end{align}
The formula can be verified by evaluating $LHS * RHS = RHS * LHS = I$.


Substituiting $A = X^{T}X$ and $-u = v = x_{i}$,
\begin{align}
        (X^{T}X - x_{i}x_{i}^{T})^{-1} &= (X^{T}X)^{-1} + \frac{(X^{T}X)^{-1}x_{i}x_{i}^{T}(X^{T}X)^{-1}}{1-x_{i}^{T}(X^{T}X)^{-1}x_{i}}\newline
        (X_{i}^{T}X_{i})^{-1} &= (X^{T}X)^{-1} + \frac{(X^{T}X)^{-1}x_{i}x_{i}^{T}(X^{T}X)^{-1}}{1-x_{i}^{T}(X^{T}X)^{-1}x_{i}}\newline
        \hat{\beta_{i}}(X_{i}^{T}Y_{i})^{-1} &= (X^{T}X)^{-1} + \frac{(X^{T}X)^{-1}x_{i}x_{i}^{T}(X^{T}X)^{-1}}{1-x_{i}^{T}(X^{T}X)^{-1}x_{i}}\newline
        \hat{\beta_{i}} &= \[(X^{T}X)^{-1} + \frac{(X^{T}X)^{-1}x_{i}x_{i}^{T}(X^{T}X)^{-1}}{1-x_{i}^{T}(X^{T}X)^{-1}x_{i}}\](X_{i}^{T}Y_{i})\newline
        \hat{\beta_{i}} &= \[(X^{T}X)^{-1} + \frac{(X^{T}X)^{-1}x_{i}x_{i}^{T}(X^{T}X)^{-1}}{1-h_{i}}\](X^{T}Y - x_{i}y_{i})\newline
        \hat{\beta_{i}} &= \hat{\beta} - \[\frac{(X^{T}X)^{-1}x_{i}}{1-h_{i}}\](y_{i}(1-h_{i})-x_{i}^{T}\hat{\beta_{i}+h_{i}y_{i}})\newline
        &= \hat{\beta} - \[\frac{(X^{T}X)^{-1}x_{i}(y_{i}-x_{i}^{T}\hat{\beta})}{1-h_{i}}\]\newline
        \text{Subsequently, } e_{i} &= y_{i} - x_{i}^{T}\hat{\beta_{i}}\newline
                                &= y_{i} - x_{i}^{T}(\hat{\beta} - \[\frac{(X^{T}X)^{-1}x_{i}(y_{i}-x_{i}^{T}\hat{\beta})}{1-h_{i}}\])\newline
                                &= (y_{i}-x_{i}^{T}\hat{\beta}) + \frac{h_{i}(y_{i}-x_{i}^{T}\hat{\beta})}{1-h_{i}}\newline
            \text{or, } e_{i} &= \frac{y_{i}-x_{i}^{T}\hat{\beta}}{1-h_{i}}\newline
                             &= \frac{y_{i}-\hat{y_{i}}}{1-h_{i}}
    \end{align}
Applying this formula for all the errors across the $n$ models,
\begin{align}
        \text{test error } &= \frac{1}{n}\sum_{i=1}^{n} error_{i}\newline
                        &= \frac{1}{n}\sum_{i=1}^{n} (\frac{y_{i}-\hat{y_{i}}}{1-h_{i}})^{2}
    \end{align}
which can be computed by simply building a single model on all the $n$ data points.

### $k$-Fold Cross Validation

Here, we divide the data set into k groups of roughly the equal size, and build $k$ models. In each model, one of the folds is chosen as the validation set while the remaining $k-1$ folds are used for training. Let $MSE_{1}, MSE_{2}, \ldots, MSE_{k}$ denote the individual errors on the $k$ subsets (when they are used for validation), then the estimate of test MSE is
\begin{align}
        MSE = \frac{1}{k}\sum_{i=1}^{k}MSE_{i}
    \end{align}

Typically, $k = 5, 10$. Leave one out CV is a special case of $k$-fold CV. The MSE calculated above can help determine the best flexibility to chose for a given model (using the minima of MSE vs fliexibility) or help compare different types of models using the value of MSE.


### $k$-Fold CV bias-variance

$k$-fold CV is preferable over LOOCV not just because of computational issues, but also due to a bias variance tradeoff. Note that the more data we use, it is expected that we will have less bias. By this logic, we expect LOOCV to have a low bias compared to $k$-fld CV. However, variance is also of interest in a statistical model. The $n$ models that we fit in LOOCV are correlated with one another, since we have almost identical data sets being used for training. Hence, the average of the MSE's of such correlated models will tend to have a higher variance. On the other hand, $k$-fold CV effectively creates less correlated models which have quite lower variance in comparison. Hence $k$-fold CV is preferable in most of the cases.
\begin{align}
        Var(X + Y) = Var(X) + Var(Y) + 2Cov(X,Y)
    \end{align}
Hence, higher the correlation, higher the variance of sum of two random variables.
