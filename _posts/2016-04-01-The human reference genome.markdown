---
title: '基因的显隐性和人类基因组'
comments: true
layout: post
tags:
    - Genomics
---

今天突然想到一个以前非常忽视的一个问题，这个问题涉及到显隐性遗传，最后涉及到人类基因组序列。首先人类是二倍体生物，也就是说每一个基因都会有两个拷贝，基因决定形状，所以决定某一性状的这一对基因就叫做等位基因（allele）。等位基因可以纯合子（homozygous）或者杂合子 (heterozygous) （也就是高中生物所教的AA，Aa）。纯合子的这一对等位基因序列相同，杂合子的不同。等位基因的这个特点决定了基因的显隐性遗传。

这就牵扯到另外一个问题，因为一对杂合的等位基因的序列不同，所以最后要以哪个序列作为人类的参照基因组呢。

要回答这个问题首先要了解人类基因组的测序的方法。到现在为止，人类的基因组其实只是选取了某一部分群体（貌似是十几个美国人）的DNA来做全基因组的测序。具体到测序方法，叫做clone-based (or hierarchical) approach。首先把一个个体的DNA打断为长度给40000-200000长度的片段，然后插入到质粒当中（比如bacterial artificial chromesome （BACs））。然后把质粒转入bacteria中扩增，然后运用NGS的方法进行测序。 就这样测出来的一个clone的序列叫做component。在测了很多clone的序列得到很多components之后，运用方法将这些component连接起来，最后组成一个长序列，叫做scaffolds。之后在运用方法将scaffolds连接成为染色体。

所以说人类的基因组是好多个基因组的混合体（conglomeration）。也就没办法去区分序列到底是来自哪个allele了。这个问题还是需要解决的，因为并不是所有的基因两个allele的序列都是一样的，甚至两个allele都是表达的。但目前没有太好的解决办法。人类基因组目前也只是在某些区间会有定义为alternative loci（这些区间的序列在不同的人中区别太大，没法用一个序列来代表）。


Reference
=========

1. <http://ncbiinsights.ncbi.nlm.nih.gov/2013/06/05/the-human-reference-genome-understanding-the-new-genome-assemblies/>
2. <http://www.ncbi.nlm.nih.gov/assembly/basics/>
3. <http://www.ncbi.nlm.nih.gov/projects/genome/assembly/grc/human/>
