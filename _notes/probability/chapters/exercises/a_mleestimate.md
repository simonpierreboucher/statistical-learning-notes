---
title: "Answer"
---

Since the observations are independent, the likelihood of all the observations under some $\theta$ is given by
\begin{align}
            p_{X|\Theta}(x|\theta) &= \prod_{i=1}^{n} \theta \exp(-\theta x_{i})\newline
            log(p_{X|\Theta}(x|\theta)) &= n log(\theta) - \theta(\sum_{i=1}^{n} x_{i})
        \end{align}

Taking the derivatie and maximizing with respect to $\theta$, $\hat{\theta}\_{MLE} = \frac{n}{\sum_{i=1}^{n}x_{i}}$
