---
title: '第一篇'
comments: true
layout: post
tags:
    - machine learning
    - personalized medicine
---

最近读了一篇nature biotechnology的paper，title是'Crowdsourced analysis of clinical trial data to predict amyotrophic sclerosis progression'. 最开始读这篇文章是抱着主办者网站上说的1 million prize去的，没想到居然被坑，最后发现其实也只有区区5万。 

抱着失落的心情读完文章之后就更失望了。 首先， 他们所谓的clinical feature的data其实是不包括genetic oriented measurements，像gene expression, cnv 等等。 其次， 所有团队用的方法都比较传统（BART的方法可能新一点， 值得一看），基本都是random forest的变体。而且，参赛team只少简直令人发指！（最后仅仅10人提交了有效的code），想想都还不如放在kaggle上去host。 看过文章之后，总体来讲有那么些疑问：
＊ 用前3个月的clinical feature去predict后9个月的progression，这个system design到底有没有问题（比如说input的时候，有些patient可能已经在fast progression阶段，或者已经完成fast progression 的过程， 对于这些patients，似乎没有必要加入到training model里面）
＊ 我还是不太明白他的linear random effects model 是怎么work的（回头看看他的code）

如果能找到关于ALS的gene expression的time course data就太好了！