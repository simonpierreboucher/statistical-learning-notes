---
title: "Answer"
---

Let $X$ be the waiting time and $F_{X}(x)$ be the CDF. Then,
\begin{align}
            F_{X}(x) = \begin{cases} 0 &\mbox{ $x < 0$}\newline
                                    \frac{2}{3} &\mbox{ $x = 0$}\newline
                                    \frac{2}{3} + \frac{1}{30}x &\mbox{ $0 < x < 5$}\newline
                                    1 &\mbox{ $5 \leq x$} \end{cases}
        \end{align}
The PDF is simply the derivate of the CDF. Thus, expectation is
\begin{align}
            E[X] = \frac{2}{3}(0) + \int_{0}^{5} \frac{1}{30}x dx + \frac{1}{6}(5) = \frac{5}{4} mins
        \end{align}
