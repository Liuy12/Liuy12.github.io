---
title: 'Comparison of differential expression methods for RNA-seq'
comments: true
layout: post
tags:
    - RNA-seq
    - Genomics
---

This is mainly the summary results from 'Comprehensive evaluation of differential gene expression analysis methods for RNA-seq data' <http://www.genomebiology.com/2013/14/9/r95#B27>. 

## Normalization

- reads per kilobase per million reads (RPKM)

$$ RPKM = ({10^9} \cdot C)/(N \cdot L) $$

Where C stands for number of reads mapped to a gene (Number of fragments in terms of FPKM); N stands for total library size; L stands for gene length. 

RPKM and FPKM has some obvious limitations. RPKM and FPKM are both propotional to the real abundance of a transcript/isoform, however, for different experiments, the propotional ratio is not constant. This means that you need to do further normalization after RPKM or FPKM.

All these methods have one limitations in common: It is relative abundance, which means the expression changes in one gene can also affect the relative abundance of other genes. 

- TPM

$$ TPM = p\cdot{10^6} $$

Or can also be formulated by: 

$$ TPM = \frac{FPKM\cdot{10^6}}{\sum FPKM} $$

where p stands for the probablity of selecting a fragment from one transcirpt among all read counts. Different from RPKM/FPKM, TPM maesures estimated abundance rather than relative abundance. TPM is generally preferred than RPKM/FPKM

- DESeq's sizeFactors

$$ \hat{s_j} = \underset{i}{median}\frac{k_{ij}}{\left(\prod_{v=1}^m k_{iv}\right)^{\frac 1m}} $$

- Cuffdiff

Similar to DESeq, firstly intro-condition scaling factor, secondly, across condition scaling factor. 

- Trimmed mean of M values (TMM)

TMM is implemented in edgeR which computes a scaling factor between two experiments by using the weighted average of the subset of genes after excluding genes that exhibit high average read counts and genes that have large differences in expression. 

baySeq implements similar method as TMM; It uses genes with upper quantile in terms of expression levels to calculate scaling factor

PoissonSeq use a subset of genes (Least differentially expressed between conditions) to calculate a scaling factor for normalization. 

- Quantile 

Very widely used normalization method for microarray, implemented in limmma; Basically ensure that the empirical distribution across all samples is the same.

## Statistical modeling

- Poisson 

PoissonSeq

- Negative binomial

To account for over-dispersion problem. DESeq and edgeR, DESeq and edgeR differs in terms of estimation of dispersion parameter (squared coefficient of variation). 

DESeq firstly models mean-variance relationship to get a better estimation of dispersion (squared coefficient of variation). For smaller number of replicates, additional apply a bias correction procedure for dispersion estimates.

edgeR firstly estimate a common dispersion of all the genes, then applied a weighted maximum liklihood estimation of the weighted paramter of common dispersion and gene-wise dispersion, so that if the dispersion are very variable (different) across all the genes, the weight parameter should be very small, otherwise, it should be very big (to a common dispersion estimates)

$$ WL(\phi_g) = l_g(\phi_g) + \alpha \cdot l_C(\phi_g) $$

Cufdiff models single isoform and multi-isoform differently; For single isoform, cufdiff applies a similar method as DESeq. Multi-isoform variance is modeled by a mixture model of negative binomials using the beta distribution parameters as mixture weights. 

baySeq implements a full Bayesian model of negative binomial distributions in which the prior probability parameters are estimated by numerical sampling from the data

## Test for differential expression

- Fisher exact test for NB distribution

Both DESeq and edgeR adopted a NB flavor fisher exact test

$$ p = \frac{\sum_{a+b=K_S \; p(a,b) \le p(K_A, K_B)} p(a,b)}{\sum_{a+b=K_S} p(a,b)} $$

- t test or moderated t test 

Cuffdiff computes test statistics:

$$ T = \frac{E[log(y)]}{Var[log(y)]} $$

This ratio approximately follows a normal distribution, a t test is applied to identify DEGs

limma uses a moderated t-statistic to compute P values in which both the standard error and the degrees of freedom are modified

<script>
  (function(i,s,o,g,r,a,m){i['GoogleAnalyticsObject']=r;i[r]=i[r]||function(){
  (i[r].q=i[r].q||[]).push(arguments)},i[r].l=1*new Date();a=s.createElement(o),
  m=s.getElementsByTagName(o)[0];a.async=1;a.src=g;m.parentNode.insertBefore(a,m)
  })(window,document,'script','//www.google-analytics.com/analytics.js','ga');

  ga('create', 'UA-66197207-1', 'auto');
  ga('send', 'pageview');

</script>


## references

Lior Pachter's talk <http://www.homolog.us/blogs/blog/2013/11/22/cshl-keynote-talk-lior-pachter/>

Discussion of RPKM, FPKM and TPM on google groups <https://groups.google.com/forum/#!topic/rsem-users/GRyJfEOK1BQ>


