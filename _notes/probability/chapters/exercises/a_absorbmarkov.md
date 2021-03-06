---
title: "Answer"
---

Let $a_{i}$ denote the abosorption probabilites into state $4$ starting from $i$
\begin{align}
            a_{5} &= 0, a{4} = 1 \newline
            a_{i} &= \sum_{j} a_{j}p_{ij}\newline
            a_{2} &= a_{1}p_{21} + a_{4}p_{24}\newline
            a_{3} &= a_{1}p_{31} + a_{2}p_{32} + a_{5}p_{35}\newline
            a_{1} &= a_{2}p_{12} + a_{3}p_{13}
        \end{align}
Solving, $a_{1} = \frac{9}{14}, a_{2} = \frac{5}{7}$ and $a_{3} = \frac{15}{28}$ 


Let $\mu_{i}$ denote the expected time till absorption starting from $i$, then
\begin{align}
            \mu_{4} &= 0 \newline
            \mu_{1} &= 1 + \mu_{2}p_{12} + \mu_{3}p_{13} \newline
            \mu_{2} &= 1 + \mu_{1}p_{21} + \mu_{4}p_{24} \newline
            \mu_{3} &= 1 + \mu_{1}p_{31} + \mu_{2}p_{32}
        \end{align}
Solving, $\mu_{1} = \frac{55}{4}, \mu_{2} = 12$ and $\mu_{3} = \frac{111}{8}$
