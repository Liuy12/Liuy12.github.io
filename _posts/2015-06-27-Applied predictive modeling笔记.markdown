---
title: 'Applied predictive modeling笔记'
comments: true
layout: post
tags:
    - machine learning
---

# Data preprocessing 

### Centering and scaling ###

Centering and scaling are generally used to improve numerical stability of some calculations

* PLS might benefit from centering and scaling 

### Transformation

Transformation is generally used to remove skewness

* log, inverse, square root transformation. 

Box-cox transformation:

$$   
x^* =
\begin{cases}
\frac{x^\lambda - 1}{\lambda},  & \text{if $\lambda$ $\neq$ 0} \\
log(x), & \text{if $\lambda$ = 0}
\end{cases} 
$$

