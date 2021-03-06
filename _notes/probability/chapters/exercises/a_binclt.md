---
title: "Answer"
---

The exact answer will be
\begin{align}
            \sum_{k=0}^{21}\binom{36}{k}(\frac{1}{2})^{36} = 0.8785
        \end{align}
But the same can be estimated using the CLT as follows
\begin{align}
            \mu &= np = 18\newline
            \sigma^{2} &= np(1-p) = 9\newline
            P(S_{n} \leq 21) &\approx P(\frac{S_{n} - 18}{3} \leq \frac{21-18}{3}) \approx 0.843
        \end{align}
Our estimate is in the rough range of the answer but not quite close. We can do better using the $\frac{1}{2}$ correction
\begin{align}
            P(S_{n} \leq 21) = P(S_{n} < 22) \quad\text{since $S_{n}$ is an integer}\newline
            \text{Consider}\quad P(S_{n} <= 21.5) \quad\text{as a compromise between the two}\newline
            P(S_{n} <= 21.5) = P(\frac{S_{n} - 18}{3} \leq \frac{21.5 - 18}{3}) \approx 0.879
        \end{align}
In a similar manner, $P(S_{n}=19) = P(18.5 \leq S_{n} \leq 19.5)$ using $\frac{1}{2}$ correction.
