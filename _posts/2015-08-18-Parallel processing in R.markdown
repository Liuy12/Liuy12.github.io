---
title: 'Parallel processing in R'
comments: true
layout: post
tags:
    - Parallel processing
---

The CRAN Task View for high performance and parallel computing <https://cran.r-project.org/web/views/HighPerformanceComputing.html>

Most of these packages cite or identify one or more of snow, Rmpi, multicore, foreach as relevant parallelization infratructure. 

Configure R session to always use a particular back-end configure `options(MulticoreParam=quote(MulticoreParam(workers=4)))`

