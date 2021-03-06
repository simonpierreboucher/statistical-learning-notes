---
title: "Answer"
---

Note that $E[X|Y]$ is a function of $y$.
\begin{align}
            E[E[X|Y]] &= \sum_{y} E[X|Y] p_Y(y)\newline
                     &= \sum_{y} \sum_{x} xp_{X|Y}p_{Y}\newline
                     &= \sum_{y}\sum_{x} xp_{X,Y}(x,y)\newline
                     &= \sum_{x}x\sum_{y}p_{X,Y}(x,y)\newline
                     &= \sum_{x} x p_{X}(x)\newline
                     &= E[X]
        \end{align}
