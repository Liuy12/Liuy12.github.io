---
title: 'Parallel processing in R'
comments: true
layout: post
tags:
    - Parallel processing
---

# Overview

The CRAN Task View for [high performance and parallel computing](https://cran.r-project.org/web/views/HighPerformanceComputing.html)

Most of these packages cite or identify one or more of [snow](https://cran.r-project.org/web/packages/snow/index.html), [Rmpi](https://cran.r-project.org/web/packages/Rmpi/index.html), multicore, [foreach](https://cran.r-project.org/web/packages/foreach/index.html) as relevant parallelization infratructure. Here we will focus on snow, multicore and parallel (Basically a combination of snow and multicore) packages

- snow: 
Simple Network of Workstations, parallel processing is made possible Via `system("Rscript")` or similar to launch a new process on the current machine or a similar machine with an identical R installation. Communication between master and worker processes is usually done via sockets. snow also takes advantage of fork and Rmpi mensioned below.

- multicore:
Parallel processing is made possible by forking, which creates a new R process by taking a complete copy of the master process.

- Rmpi:
Using OS-level facilities to set up a means to send tasks to other members of a group of machines.

- `detectCores` is function used to detect number of phisical cores/CPUs in a system. The result is system dependent. 

- Parallel version of apply function, including `parLapply, parSapply, parApply, mclapply` ect. 

# Benchmark of `lapply, for loop, foreach dopar, foreach do, parLapply, parSapply` in various scenarios

- Compare the performance of six methods for simple function `sqrt`

~~~~~~~~~~~~~~~~~
library(doParallel)
library(doMC)
library(rbenchmark)
library(ggplot2)

cl <- makeCluster(4)
registerDoParallel(cl)
getDoParWorkers()
set.seed(1990)

FUN <- function(x) { round(sqrt(x), 4) }
a <- lapply(1:10, function(i) i)

test1 <- benchmark("lapply" = lapply(1:10, FUN = FUN), 
                   "For loop" = for(i in 1:10){ FUN(i)},
                   "Foreach dopar" = foreach(i = 1:10) %dopar% FUN(i),
                   "Foreach do" = foreach(i = 1:10) %do% FUN(i),
                   "parLapply" = parLapply(cl = cl, X = a, fun = FUN),
                   "parSapply" = parSapply(cl = cl, X = a, FUN = FUN),
                   columns=c('test', 'elapsed', 'replications'),
                   replications = c(100, 200, 500, 1000))

ggplot() +
  geom_line(aes(x = replications, y = elapsed, colour = test), data = test1)
~~~~~~~~~~~~~~~~~

![p1](/media/2015-08-18-ParallelprocessinginR/benchmark1.png)

In this case, `for loop` and `lappy` seems to perform the best among other methods. 

- Compare the performance of six methods for matrix multiplication

Element-wise multiplication

~~~~~~~~~~~~
FUN <- function(x){
  a <- matrix(rnorm(10000), 100, 100)
  b <- matrix(rnorm(10000), 100, 100)
  a * b
}

test2 <- benchmark("lapply" = lapply(1:10, FUN = FUN), 
                   "For loop" = for(i in 1:10){ FUN(i)},
                   "Foreach dopar" = foreach(i = 1:10) %dopar% FUN(i),
                   "Foreach do" = foreach(i = 1:10) %do% FUN(i),
                   "parLapply" = parLapply(cl = cl, X = a, fun = FUN),
                   "parSapply" = parSapply(cl = cl, X = a, FUN = FUN),
                   columns=c('test', 'elapsed', 'replications'),
                   replications = c(100, 200, 500, 1000))

ggplot() +
  geom_line(aes(x = replications, y = elapsed, colour = test), data = test2)
~~~~~~~~~~~~

![p2](/media/2015-08-18-ParallelprocessinginR/benchmark2.png)

In this case, `parLapply` and `parSapply` seems to perform the best, but not a significant increase in terms of efficiency since we used 4 cores. 

Inner product

~~~~~~~~~~~~~
FUN <- function(x) {
  a <- matrix(rnorm(10000), 100, 100)
  b <- matrix(rnorm(10000), 100, 100)
  a %*% b
}

test3 <- benchmark("lapply" = lapply(1:10, FUN = FUN), 
                   "For loop" = for(i in 1:10){ FUN(i)},
                   "Foreach dopar" = foreach(i = 1:10) %dopar% FUN(i),
                   "Foreach do" = foreach(i = 1:10) %do% FUN(i),
                   "parLapply" = parLapply(cl = cl, X = a, fun = FUN),
                   "parSapply" = parSapply(cl = cl, X = a, FUN = FUN),
                   columns=c('test', 'elapsed', 'replications'),
                   replications = c(100, 200, 500, 1000))

ggplot() +
  geom_line(aes(x = replications, y = elapsed, colour = test), data = test3)
~~~~~~~~~~~~~

![p3](/media/2015-08-18-ParallelprocessinginR/benchmark3.png)

This time, `parLapply` performed the best followed by `parSapply`, but still not a significant increase in terms of efficiency since we used 4 cores. 

Inner productor with unbalanced dimensions

~~~~~~~~~~
FUN <- function(x) {
  a <- matrix(rnorm(10000), 1000, 10)
  b <- matrix(rnorm(10000), 10, 1000)
  a %*% b
}

test4 <- benchmark("lapply" = lapply(1:10, FUN = FUN), 
                   "For loop" = for(i in 1:10){ FUN(i)},
                   "Foreach dopar" = foreach(i = 1:10) %dopar% FUN(i),
                   "Foreach do" = foreach(i = 1:10) %do% FUN(i),
                   "parLapply" = parLapply(cl = cl, X = a, fun = FUN),
                   "parSapply" = parSapply(cl = cl, X = a, FUN = FUN),
                   columns=c('test', 'elapsed', 'replications'),
                   replications = c(10, 20, 50, 100))

ggplot() +
  geom_line(aes(x = replications, y = elapsed, colour = test), data = test4)
~~~~~~~~~~

![p4](/media/2015-08-18-ParallelprocessinginR/benchmark4.png)

This time, `For loop, Foreach do, lapply` performed much better than the other three methods. `parSapply` performed the worst.

- Compare the performance of six methods for fitting generalized linear model

~~~~~~~~~~
FUN <- function(i) {
  ind <- sample(100, 100, replace=TRUE)
  result1 <- glm(Species~Sepal.Length, family=binomial(logit), data = iris[ind,])
  coefficients(result1)
}

test5 <- benchmark("lapply" = lapply(1:10, FUN = FUN), 
                   "For loop" = for(i in 1:10){ FUN(i)},
                   "Foreach dopar" = foreach(i = 1:10) %dopar% FUN(i),
                   "Foreach do" = foreach(i = 1:10) %do% FUN(i),
                   "parLapply" = parLapply(cl = cl, X = a, fun = FUN),
                   "parSapply" = parSapply(cl = cl, X = a, FUN = FUN),
                   columns=c('test', 'elapsed', 'replications'),
                   replications = c(100, 200, 500, 100))

ggplot() +
  geom_line(aes(x = replications, y = elapsed, colour = test), data = test5)
~~~~~~~~~~

![p5](/media/2015-08-18-ParallelprocessinginR/benchmark5.png)

This time, `parLapply` and `parSapply` performed the best. `Foreach do` performed the worst

`glm` but in a list fashion

~~~~~~~~~~~~~~
a <- lapply(1:10, function(i) iris)

FUN <- function(x) {
  ind <- sample(100, 100, replace=TRUE)
  result1 <- glm(Species~Sepal.Length, family=binomial(logit), data = x[ind,])
  coefficients(result1)
}

test6 <- benchmark("lapply" = lapply(a, FUN = FUN), 
                   "For loop" = for(i in 1:10){ FUN(a[[i]])},
                   "Foreach dopar" = foreach(i = 1:10) %dopar% FUN(a[[i]]),
                   "Foreach do" = foreach(i = 1:10) %do% FUN(a[[i]]),
                   "parLapply" = parLapply(cl = cl, X = a, fun = FUN),
                   "parSapply" = parSapply(cl = cl, X = a, FUN = FUN),
                   columns=c('test', 'elapsed', 'replications'),
                   replications = c(100, 200, 500, 1000))

ggplot() +
  geom_line(aes(x = replications, y = elapsed, colour = test), data = test6)
~~~~~~~~~~~~~~

![p6](/media/2015-08-18-ParallelprocessinginR/benchmark6.png)

Not much difference compared to the previous one. `parLapply` and `parSapply` performed the best. 

- Using `doMC` as the backend

~~~~~~~~~~~
registerDoMC(cores = 4)

test7 <- benchmark("lapply" = lapply(a, FUN = FUN), 
                   "For loop" = for(i in 1:10){ FUN(a[[i]])},
                   "Foreach dopar" = foreach(i = 1:10) %dopar% FUN(a[[i]]),
                   "Foreach do" = foreach(i = 1:10) %do% FUN(a[[i]]),
                   "parLapply" = parLapply(cl = cl, X = a, fun = FUN),
                   "parSapply" = parSapply(cl = cl, X = a, FUN = FUN),
                   columns=c('test', 'elapsed', 'replications'),
                   replications = c(100, 200, 500, 1000))

ggplot() +
  geom_line(aes(x = replications, y = elapsed, colour = test), data = test7)
~~~~~~~~~~~

![p7](/media/2015-08-18-ParallelprocessinginR/benchmark7.png)

`parSapply` and `parLapply` still performed the best. Very interestingly, `foreach dopar` performed the worst.

# To summerize
Generally, `parLapply` and `parSapply` perform better than `foreach`. However, for all parallel implementation methods, the increase in terms of efficiency is not propotional to the number of cores being used (Theoretical efficiency). 

# Session information

~~~~~~~~~
R version 3.2.3 (2015-12-10)
Platform: x86_64-apple-darwin13.4.0 (64-bit)
Running under: OS X 10.11.1 (El Capitan)

locale:
[1] en_US.UTF-8/en_US.UTF-8/en_US.UTF-8/C/en_US.UTF-8/en_US.UTF-8

attached base packages:
[1] parallel  stats     graphics  grDevices utils     datasets  methods   base     

other attached packages:
[1] ggplot2_2.1.0    rbenchmark_1.0.0 doMC_1.3.4       doParallel_1.0.8 iterators_1.0.7 
[6] foreach_1.4.2   

loaded via a namespace (and not attached):
 [1] Rcpp_0.12.3      codetools_0.2-14 digest_0.6.9     grid_3.2.3       plyr_1.8.3      
 [6] gtable_0.2.0     scales_0.4.0     labeling_0.3     tools_3.2.3      munsell_0.4.3   
[11] compiler_3.2.3   colorspace_1.2-6
~~~~~~~~~


