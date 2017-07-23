---
title: 'Time series analysis and its applications笔记'
comments: true
layout: post
tags:
    - Time series analysis
    - R
---

开一篇文章总结time series 相关领域的信息（常用模型， 基本概念等）。 主要是两本书的总结， 第一本是， Shumway的 Time series analysis and its applications. 第二本是Chatfield的 The analysis of Time Series: An introduction. 

- White Noise, a collection of uncorrelated random variables, $$ w_t \sim wn(0, \sigma_w^2) $$

- Moving averages, average with intermediate neighbors in the past and future， $$ v_t = \frac{1}{3}(w_{t-1} + w_t + w_{t+1}) $$

- Auto regression, $$ x_t = ax_{t-1} + bx_{t-2} + w_t $$

- Random walk with drift, $$ x_t = \sigma + x_{t-1} + w_t $$, where $$ \sigma $$ is called the drift 



