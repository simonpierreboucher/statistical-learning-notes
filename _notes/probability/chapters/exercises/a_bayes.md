---
title: "Answer"
---

From Bayes' theorem
\begin{align}
            f_{Q|X}(q|x) &= \frac{f_{X|Q}(x|q) f_{Q}(q)}{f_{X}(x)}\newline
                        &= \frac{f_{X|Q}(x|q) f_{Q}(q)}{\int_{0}^{1} f_{X|Q}(x|q) f_{Q}(q) dq}
        \end{align}
We will need to solve separately for $x = 0$ and $x = 1$ as $x$ is discrete.
\begin{align}
            f_{Q|X=0}(q|x=0) &= \frac{(1-q) \times 6q(1-q)}{\int_{0}^{1} (1-q)\times 6q(1-q) dq} = 12q(1-q)^{2}\newline
            f_{Q|X=1}(q|x=1) &= \frac{q \times 6q(1-q)}{\int_{0}^{1} q\times 6q(1-q) dq} = 12q^{2}(1-q)
        \end{align}
