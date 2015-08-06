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




## references

Lior Pachter's talk <http://www.homolog.us/blogs/blog/2013/11/22/cshl-keynote-talk-lior-pachter/>

Discussion of RPKM, FPKM and TPM on google groups <https://groups.google.com/forum/#!topic/rsem-users/GRyJfEOK1BQ>


