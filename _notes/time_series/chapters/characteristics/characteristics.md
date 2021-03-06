---
title: "Characteristics of Time Series"
---

# Characteristics of Time Series

The study of time series is devoted to application of statistical techniques to data collected over multiple time steps. Traditional techniques are not useful here due to strong correlation between successive observations.

Two separate but not necessarily mutually exclusive approaches exist to study time series

1.  Time Domain approach
    This approach gives importance to lagged relationships, such as how what happens today will effect the obervation 7 days later.

2.  Frequency Domain approach
    This approach gives importance to investigation of cycles, such as economic cycle of periods of expansion/recession.

A time series is a collection of random variables $x_{1}, x_{2}, x_{3}, \ldots$ and can in general be denoted by $\{x_{t}\}$. Such a collection of random variables indexed by time is also called a **random process** and the values collected are often called the **realization** of the random process. In the context of time series analysis, the two can be used interchangeably.


The sampling rate can serve an important purpose when representing time series data. An insufficient sampling rate can completely change the appearance of the data. For instance, when recording the motion of a wheel, it can appear moving backwards at a low enough recording rate. This distortion is called **aliasing**.
