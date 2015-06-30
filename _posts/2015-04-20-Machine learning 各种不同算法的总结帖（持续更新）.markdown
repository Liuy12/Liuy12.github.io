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

# Random forest

Random forest 是基于decision tree的一种方法，简单的来说就是创建 a multitude of decision trees. 先bootstrap training data， 然后对每一个bootstrap， 创建一个decision tree。为了解决tree 与 tree之间可能有的correlation的问题，在每一个resampling的data上，仅仅只使用原始feature的一部分。在R上的这个可以用来tune的parameter是 mtry， classification 的problem default是 \\( \sqrt{f} \\). Regression 的 default 是 \\( \frac{f}{3} \\). 也因为此，random forest并不需要传统的cross validation。可以用其oob (out of bag error) 来进行筛选model。

Random forest 的 error rate 取决于两个方面，

- 树与树之间的correlation， correlation越大，error也越大 
- 每一个树的predictive power， predictive power 越强， error 越小

以上两点都会随着mtry的增大而增大，减小而减少。所以会存在一个中间的optimal number 可以 tune for。 

Random forest的一些特征

- Random forest 可以用来estimate variable importance

1. Permutation-based：基本流程就是，对于每一个tree的oob cases， random permute某一个variable，然后计算跟不permute之间的差异。然后取每个tree的average，计算出这个variable的raw imp score。然后，assume 树与树之间的score是independent的，计算出standard error。从而算出Z score。Assume normal distribution， 算出p value。

2. Gini impurity based：每一个针对某个variable的decision tree的分割，分下来的branch 都会比parent tree得gini impurity小。把所有针对于某个variable的分割的gini impurity decrease通加起来就会迅速的得到某一个variable的important score。通常会跟permutation-based方法结果类似。

- unexcelled in accuracy

但是，以我自己的使用经历来看，rf通常都会是中等偏上的结果。（待验证）

- 可以用来estimate missing values

1. 在所有同一个class的这个variable中求median（Non-categorical）或者是mode （categorical）

2. 第二种方式相对来说比较费时，但是相对精确。首先粗略估计一下missing value的值（可以用第一种方法来作为起始值），做一次rf，然后算sample之间的proximity，missing value的值就是weighted average或者weighted class frequency，weights就是proximity。然后循环继续同样过程，通常5-6次循环即可。

3. 对于test set的missing data的处理，如果能够使用label的话，可以运用上面两种方式，如果不能使用label的话，假设每一个sample都belong to所有的class，然后去做filling。

- Compute proximities between classes, 可以用来做cluster， outlier detection

在tree grow完成以后，将training data和oob一起过一遍tree，如果某一个sample跟另外一个在同一个terminal node里面，proximity加一。然后normalize到所有的tree

- 可以用来测设不同variable之间的interaction。（need to try sometime）

interaction就是比如说，一个variablesplit会导致对于另外一个variable的split more/less likely。

- scaling

类似于MDS，不过是运用proximity。

# Generalized Boosted Regression Models (gbm)

 其实现在还是不太清楚gbm是stand for Generalized Boosted Regression Models 还是，gradient boosted models。。





