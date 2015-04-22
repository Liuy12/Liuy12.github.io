---
title: '各种不同算法的总结帖（持续更新）'
comments: true
layout: post
tags:
    - machine learning
    - random forest
    - gradient boosting machine
    - support vector machine
---

转眼间接触machine learning 已经半年多了， 现在开一个帖子写写关于不同machine learning 算法的总结帖。

Random forest
=============

Random forest 是基于decision tree的一种方法，简单的来说就是创建 a multitude of decision trees. 先bootstrap training data， 然后对每一个bootstrap， 创建一个decision tree。为了解决tree 与 tree之间可能有的correlation的问题，在每一个resampling的data上，仅仅只使用原始feature的一部分。在R上的这个可以用来tune的parameter是 mtry， classification 的problem default是 \\( \sqrt{f} \\). Regression 的 default 是 \\( \frac{f}{3} \\). 

Random forest 的 error rate 取决于两个方面，

- 树与树之间的correlation， correlation越大，error也越大 
- 每一个树的predictive power， predictive power 越强， error 越小

以上两点都会随着mtry的增大而增大，减小而减少。所以会存在一个中间的optimal number 可以 tune for。 

Random forest的一些特征

- Random forest 可以用来estimate variable importance（to be added）
- unexcelled in accuracy
- 可以用来estimate missing values (to be added)
- Compute proximities between classes, 可以用来做cluster， outlier detection
- 
-
-







