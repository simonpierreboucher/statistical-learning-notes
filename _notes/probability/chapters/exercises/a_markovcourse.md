---
title: "Answer"
---

1.  The probability of eventually leaving course 6 is 1 as states 15 and 9 are absorbing states.

2.  Here we have to calculate the probability of absortion into state 15. Let $a_{i}$ denote the probability of absorption into state 15 from state $i$. Then, $a_{15} = 1$ and $a_{9} = 0$. Using equations from [section]({{ "/notes/probability/chapters/markov_process/absorption.html" | relative_url }}),
    \begin{align}
                    a_{6-1} &= \frac{1}{2}a_{6-1} + \frac{1}{8} a_{6-2} + \frac{1}{8} a_{6-3} + \frac{1}{8}a_{9} + \frac{1}{8}a_{15}\newline
                    a_{6-2} &= \frac{1}{2}a_{15} + \frac{3}{8}a_{6-1} + \frac{1}{8}a_{6-3}\newline
                    a_{6-3} &= \frac{1}{4}a_{9} + \frac{3}{8}a_{6-1} + \frac{3}{8}a_{6-2}
                \end{align}
    Solving the 3 equations, 3 variable system, $a_{6-1} = 105/184, a_{6-2} = 143/184$ and $a_{6-3} = 93/184$.

3.  Let $\mu_{i}$ denote the expected number of steps to get absorbed starting from state $i$. Then, $\mu_{15} = \mu_{9} = 0$. Using equations from [section]({{ "/notes/probability/chapters/markov_process/absorption.html" | relative_url }}),
    \begin{align}
                    \mu_{6-1} &= 1 + \frac{1}{2}\mu_{6-1} + \frac{1}{8} \mu_{6-2} + \frac{1}{8} \mu_{6-3} + \frac{1}{8}\mu_{9} + \frac{1}{8}\mu_{15}\newline
                    \mu_{6-2} &= 1 + \frac{1}{2}\mu_{15} + \frac{3}{8}\mu_{6-1} + \frac{1}{8}\mu_{6-3}\newline
                    \mu_{6-3} &= 1 + \frac{1}{4}\mu_{9} + \frac{3}{8}\mu_{6-1} + \frac{3}{8}\mu_{6-2}
                \end{align}
    Solving, $\mu_{6-1} = 81/23, \mu_{6-2} = 63/23$ and $\mu_{6-3} = 77/23$.

4.  This question can be done in a manner similar to the equations described above but with a small adjustment. Note that, we can either have 0, 1, or 2 ice creams. Consider $v_{i}(j)$ as the probability of making $j$ additional ice creams from 6-2 to 6-1 or 6-3 to 6-1 transitions, given the current state is $i$. Note $v_{15}(0) = v_{9}(0) = 1$. Then,
    \begin{align}
                    v_{6-1}(0) &= \frac{1}{2}v_{6-1}(0) + \frac{1}{8} v_{6-2}(0) + \frac{1}{8} v_{6-3}(0) + \frac{1}{8}v_{9}(0) + \frac{1}{8}v_{15}(0)\newline
                    v_{6-2}(0) &= \frac{1}{2} v_{15}(0) + \frac{3}{8}(0) + \frac{1}{8}v_{6-3}(0)\newline
                    v_{6-3}(0) &= \frac{1}{4}v_{9}(0) + \frac{3}{8}(0) + \frac{3}{8}v_{6-2}(0)
                \end{align}
    Some of the transitions have been directly replaced with 0 as we are considering 0 ice creams and thus those transitions are not possible (6-2 to 6-1 for instance). Solving, $v_{6-1}(0) = 46/61, v_{6-2}(0) = 34/61$ and $v_{6-3}(0) = 28/61$.


    The same way, we can construct equations for 1 additional steps where $v_{15}(1) = v_{9} = 0$.
    \begin{align}
                    v_{6-1}(1) &= \frac{1}{2}v_{6-1}(1) + \frac{1}{8} v_{6-2}(1) + \frac{1}{8} v_{6-3}(1) + \frac{1}{8}v_{9}(1) + \frac{1}{8}v_{15}(1)\newline
                    v_{6-2}(1) &= \frac{1}{2} v_{15}(1) + \frac{3}{8}v_{6-1}(0) + \frac{1}{8}v_{6-3}(1)\newline
                    v_{6-3}(1) &= \frac{1}{4}v_{9}(1) + \frac{3}{8}v_{6-1}(0) + \frac{3}{8}v_{6-2}(1)
                \end{align}
    In the second equation, after going from 6-2 to 6-1, we can only get 0 more ice creams. Hence, some of the values have been replaced with the $v_{i}(0)$ calculated above. Solving, $v_{6-1}(1) = 690/3721, v_{6-2}(1) = 1242/3721$ and $v_{6-3}(1) = 1518/3721$.


    Note that since the total ice creams are 0, 1, or 2, we have $v_{6-1}(0) + v_{6-1}(1) + v_{6-1}(2) = 1$.
    \begin{align}
        E\[\text{ice creams}\] = 0 \times v_{6-1}(0) + 1 \times v_{6-1}(1) + 2 \times v_{6-1}(2) = 1140/3721
    \end{align}

5.  We need to recalculate the the transition probabilities since we are conditioning on the event $A$ that we land up in state 15.
    \begin{align}
                    P_{ij \vert A} &= P(X_{n+1}=j \vert X_{i}=i,A)\newline
                    &= \frac{P(X_{n+1}=j, X_{n}=i, A)}{P(X_{n}=i, A)}\newline
                    &= \frac{P(A \vert X_{n+1}=j, X_{n}=i) P(X_{n+1}=j \vert X_{n}=i) P(X_{n}=i)}{P(A \vert X_{n}=i) P(X_{n}=i)}\newline
                    &= \frac{P(A \vert X_{n+1}=j) P(X_{n+1}=j \vert X_{n}=i)}{P(A \vert X_{n}=i)}\newline
                    &= \frac{a_{j}}{a_{i}} P_{ij}
                \end{align}
    where $a_{i}$ is the probability of absorption into state 15 starting from state $i$. Since markov process is only dependent on the last state, absoprtion probabilities are not dependent on $n$.


    We can write equations similar to [section]({{ "/notes/probability/chapters/markov_process/absorption.html" | relative_url }}) for calculating the expected number of steps with the adjusted transition probabilities
    \begin{align}
                    \mu_{6-1} &= 1 + \frac{a_{6-1}}{a_{6-1}}\frac{1}{2} \mu_{6-1} + \frac{a_{6-2}}{a_{6-1}}\frac{1}{8} \mu_{6-2} + \frac{a_{6-3}}{a_{6-1}}\frac{1}{8} \mu_{6-3} + \frac{a_{15}}{a_{6-1}}\frac{1}{8} \mu_{15}+ \frac{a_{9}}{a_{6-1}}\frac{1}{8} \mu_{9}\newline
                    \mu_{6-2} &= 1 + \frac{a_{6-1}}{a_{6-2}}\frac{3}{8} \mu_{6-1} + \frac{a_{6-3}}{a_{6-2}}\frac{1}{8} \mu_{6-3} + \frac{a_{15}}{a_{6-2}}\frac{1}{2} \mu_{15}\newline
                    \mu_{6-3} &= 1 + \frac{a_{6-1}}{a_{6-3}}\frac{3}{8} \mu_{6-1} + \frac{a_{6-2}}{a_{6-3}}\frac{3}{8} \mu_{6-2} + \frac{a_{9}}{a_{6-3}}\frac{1}{4} \mu_{9}\newline
                \end{align}
    where $\mu_{15} = \mu_{9} = 0, a_{15} = 1$, and $a_{9} = 0$. The absorption probabilities can be taken from part 2. Solving, $\mu_{6-1} = 1763/483$.

6.  The changed probabilites become
    \begin{align}
        P(X_{n+1}=15 \vert X_{n}=6-1) &= P(X_{n+1}=6-2 \vert X_{n}=6-1)\newline
        &= P(X_{n+1}=6-3 \vert X_{n}=6-1) = 1/6\newline
        P(X_{n+1}=6-1 \vert X_{n}=6-2) &= 3/4\newline
        P(X_{n+1}=6-3 \vert X_{n}=6-2)=1/4
    \end{align}
    We then use equations from [section]({{ "/notes/probability/chapters/markov_process/absorption.html" | relative_url }}) to calculate the expected values
    \begin{align}
                    \mu_{6-1} &= 1 + \frac{1}{2}\mu_{6-1} + \frac{1}{6} \mu_{6-2} + \frac{1}{6} \mu_{6-3} + \frac{1}{6}\mu_{9}\newline
                    \mu_{6-2} &= 1 + \frac{3}{4}\mu_{6-1} + \frac{1}{4}\mu_{6-3}\newline
                    \mu_{6-3} &= 1 + \frac{1}{4}\mu_{9} + \frac{3}{8}\mu_{6-1} + \frac{3}{8}\mu_{6-2}
                \end{align}
    where $\mu_{15} = 0$. Solving, $\mu_{6-1} = 86/13, \mu_{6-2} = 98/13$ and $\mu_{6-3} = 82/13$.

7.  If we look carefully at the new probabilities, states 15 and 9 become recurrent. Far into the future, we are sure to land up in those states, and will be in either one of those. By symmetry, the two should be same. $\pi_{15} = \pi_{9} = 1/2$.

8.  We assume that 6-1 is an absorbing state, and accordingly calculate the probabilities. Note that there will not be an equation for 6-1 since we are then already in the final state.
    \begin{align}
                    \mu_{6-2} &= 1 + \frac{1}{8} \mu_{6-3} + \frac{1}{2} \mu_{15}\newline
                    \mu_{6-3} &= 1 + \frac{3}{8} \mu_{6-2} + \frac{1}{4} \mu_{9}\newline
                    \mu_{9} &= 1 + \frac{7}{8} \mu_{9}\newline
                    \mu_{15} &= 1 + \frac{7}{8} \mu_{15}\newline
                \end{align}
    Solving, $\mu_{6-2} = 344/61, \mu_{6-3} = 312/61$ and $\mu_{9} = \mu_{15} = 8$. Plugging these into the following equation (which corresponds to taking one step out of 6-1),
    \begin{align}
                    \mu_{6-1} = 1 + \frac{1}{2} \mu_{6-1} + \frac{1}{8} \mu_{15} + \frac{1}{8} \mu_{6-2} + \frac{1}{8} \mu_{6-3} + \frac{1}{8} \mu_{15} = \frac{265}{61}
                \end{align}
