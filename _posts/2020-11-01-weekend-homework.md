---
layout: post
title:  "Weekend Homework"
date:   2020-11-01 09:30:00
categories: jekyll update
---
Part B: Formal Exercises
-----

E.1 For any constant 
![equation](https://latex.codecogs.com/gif.latex?E%28c%29%20%3D%20c)

E.2 For any constants *a* and *b* ![equation](https://latex.codecogs.com/gif.latex?E%28aX%20&plus;%20b%29%20%3D%20aE%28X%29%20&plus;%20b)

E.3 If ![equation](https://latex.codecogs.com/gif.latex?%5C%7Ba_1%2Ca_2%2C...%2Ca_n%5C%7D) are constants and ![equation](https://latex.codecogs.com/gif.latex?%5C%7BX_1%2CX_2%2C...%2CX_n%5C%7D) are random variables, then: ![equation](https://latex.codecogs.com/gif.latex?E%28a_1X_1%20&plus;%20a_2X_2%20&plus;%20...%20&plus;%20a_nX_n%29%20%3D%20a_1E%28X_1%29%20&plus;%20a_2E%28X_2%29%20&plus;%20...%20&plus;%20a_nE%28X_n%29)

V.1 For any constants *a* and *b* ![equation](https://latex.codecogs.com/gif.latex?Var%28aX%20&plus;%20b%29%20%3D%20a%5E2%20Var%28X%29.)

**Linear projection coefficient as best linear predictor**

![equation](https://latex.codecogs.com/gif.latex?%5Cbegin%7Balign*%7D%20%5Chat%5Cbeta%20%26%3D%20%5Carg%20%5Cmin_%7B%5Cbeta%5Cin%5Cmathbb%7BR%7D%5EK%7D%20%5Ctext%7BE%7D%28%5By-%5Ctextbf%7Bx%7D%27%5Cbeta%5D%5E2%29%20%5C%5C%20S%28%5Cbeta%29%20%26%3D%20%5Ctext%7BE%7D%28%5By-%5Ctextbf%7Bx%7D%27%5Cbeta%5D%5E2%29%5C%5C%20%26%3D%20%5Ctext%7BE%7D%28y%5E2%29-2%5Cbeta%27%20%5Ctext%7BE%7D%28%5Ctextbf%7Bx%7Dy%29%20&plus;%20%5Cbeta%27%5Ctext%7BE%7D%28%5Ctextbf%7Bxx%7D%27%29%5Cbeta%5C%5C%20%5Cfrac%7B%5Cpartial%20S%28%5Cbeta%29%7D%7B%5Cpartial%5Cbeta%7D%20%26%3D%20-2%5Ctext%7BE%7D%28%5Ctextbf%7Bxy%7D%29%20&plus;%202%5Ctext%7BE%7D%28%5Ctextbf%7Bxx%7D%27%29%5Cbeta%20%3D%200%5C%5C%20%5Cbeta%20%26%3D%20%28%5Ctext%7BE%7D%28%5Ctextbf%7Bxx%7D%27%29%29%5E%7B-1%7D%5Ctext%7BE%7D%28%5Ctextbf%7Bx%7Dy%29%20%5Cend%7Balign*%7D)

**Deriving the OLS-estimator in the sample analogue**

![equation](https://latex.codecogs.com/gif.latex?%5Cbegin%7Balign*%7D%20%5Chat%5Cbeta%20%26%3D%20%5Carg%20%5Cmin%20%5Cfrac%7B1%7D%7Bn%7D%5Csum%5En_%7Bi%3D1%7D%28y_i-x%27_i%5Cbeta%29%5E2%20%5C%5C%20%26%3D%20%5Carg%5Cmin%20%5Cfrac%7B1%7D%7Bn%7D%5Csum%5En_%7Bi%3D1%7D%5B%28y_i-x%27_i%5Cbeta%29%28y_i-x%27_i%5Cbeta%29%27%5D%20%5C%5C%20%26%3D%20%5Carg%5Cmin%20%28%5Cfrac%7B1%7D%7Bn%7D%5Csum%5En_%7Bi%3D1%7D%20y_iy_i%27%29%20-%202%5Cbeta%20%28%5Cfrac%7B1%7D%7Bn%7D%20%5Csum%5En_%7Bi%3D1%7Dx_i%27y_i%29%20&plus;%20%5Cbeta%5E2%20%28%5Cfrac%7B1%7D%7Bn%7D%20%5Csum%5En_%7Bi%3D1%7D%20x_ix_i%27%29%20%5C%5C%20%5Cfrac%7B%5Cpartial%20%28%5Ccdot%29%7D%7B%5Cpartial%5Cbeta%7D%20%26%3D%20-2%28%5Cfrac%7B1%7D%7Bn%7D%5Csum%5En_%7Bi%3D1%7Dx_i%27y_i%29%20&plus;%202%5Cbeta%28%5Cfrac%7B1%7D%7Bn%7D%5Csum%5En_%7Bi%3D1%7D%20x_ix_i%27%29%20%3D%200%5C%5C%20%5Cbeta%20%28%5Cfrac%7B1%7D%7Bn%7D%20%5Csum%5En_%7Bi%3D1%7D%20x_ix_i%27%29%20%26%3D%20%28%5Cfrac%7B1%7D%7Bn%7D%5Csum%5En_%7Bi%3D1%7Dx_i%27y_i%29%5C%5C%20%5Chat%5Cbeta%20%26%3D%20%28%5Cfrac%7B1%7D%7Bn%7D%5Csum%5En_%7Bi%3D1%7D%20x_ix_i%27%20%29%5E%7B-1%7D%20%28%5Cfrac%7B1%7D%7Bn%7D%5Csum%5En_%7Bi%3D1%7Dx_i%27y_i%29%20%5Cend%7Balign*%7D)

**Proof of unbiasedness**

![equation](https://latex.codecogs.com/gif.latex?%5Cbegin%7Balign*%7D%20%5Ctext%7BE%7D%28%5Chat%5Cbeta%7C%5Ctextbf%7BX%7D%29%20%26%3D%20%5Ctext%7BE%7D%28%28%5Ctextbf%7BX%7D%27%5Ctextbf%7BX%7D%29%5E%7B-1%7D%5Ctextbf%7BX%7D%27%5Ctextbf%7By%7D%7C%5Ctextbf%7BX%7D%29%20%5C%5C%20%26%3D%20%28%5Ctextbf%7BX%7D%27%5Ctextbf%7BX%7D%29%5E%7B-1%7D%5Ctextbf%7BX%7D%27%5Ctext%7BE%7D%28%5Ctextbf%7By%7D%7C%5Ctextbf%7BX%7D%29%20%5C%5C%20%26%3D%20%28%5Ctextbf%7BX%7D%27%5Ctextbf%7BX%7D%29%5E%7B-1%7D%5Ctextbf%7BX%7D%27%5Ctextbf%7BX%7D%29%5Cbeta%20&plus;%20%28%5Ctextbf%7BX%7D%27%5Ctextbf%7BX%7D%29%5E%7B-1%7D%5Ctextbf%7BX%7D%27%5Ctext%7BE%7D%28%5Ctextbf%7Bu%7D%7C%5Ctextbf%7BX%7D%29%20%5C%5C%20%26%3D%20%5Cbeta%20%5Cend%7Balign*%7D)

By law of iterated expectations

![equation](https://latex.codecogs.com/gif.latex?%5Cbegin%7Balign*%7D%20%5Ctext%7BE%7D%28%5Chat%5Cbeta%29%20%26%3D%20%5Ctext%7BE%7D%28%5Ctext%7BE%7D%28%5Chat%5Cbeta%7C%5Ctextbf%7BX%7D%29%29%20%5C%5C%20%26%3D%20%5Cbeta%20%5Cend%7Balign*%7D)

When ![equation](https://latex.codecogs.com/gif.latex?%5Cbegin%7Balign*%7D%20%5Ctext%7BE%7D%28%5Ctextbf%7Bu%7D%7C%5Ctextbf%7BX%7D%29%3D2%20%5Cend%7Balign*%7D)

![equation](https://latex.codecogs.com/gif.latex?%5Cbegin%7Balign*%7D%20%5Ctext%7BE%7D%28%5Chat%5Cbeta%7C%5Ctextbf%7BX%7D%29%20%26%3D%20%5Cbeta%20&plus;%202%20%28%5Ctextbf%7BX%7D%27%5Ctextbf%7BX%7D%29%5E%7B-1%7D%5Ctextbf%7BX%7D%27%20%5Cend%7Balign*%7D)

Instructions for Programming
-----

Setup
-----

Set working directory and activate necessary packages

    setwd("~/Desktop/R WD")
    library(GGally)

    ## Loading required package: ggplot2

    ## Registered S3 method overwritten by 'GGally':
    ##   method from   
    ##   +.gg   ggplot2

    library(ggplot2)
    library(stargazer)

    ## 
    ## Please cite as:

    ##  Hlavac, Marek (2018). stargazer: Well-Formatted Regression and Summary Statistics Tables.

    ##  R package version 5.2.2. https://CRAN.R-project.org/package=stargazer

    library(data.table)

Part 1: Distributions, Samples and the Central Limit Theorem
------------------------------------------------------------

**i) The slides show several distributions: binomial, Poisson, normal,
Chi Squared, F, and the tdistribution. Choose at least 3 distributions
Try to simulate data (e.g. 5000 draws) from a Random Variable that
follows this distribution.**

    set.seed(1)
    A <- rnorm(5000)
    set.seed(2)
    B <- rbinom(5000, 100, 0.25)
    set.seed(3)
    C <- rpois(5000, 10)

**ii) Draw two samples from each variable and report the sample means.
Are they equal?**

    set.seed(4)
    A1 <- sample(A, size = 100)
    set.seed(5)
    A2 <- sample(A, size = 100)
    mean.A1 <- mean(A1)
    mean.A2 <- mean(A2)
    mean.A1

    ## [1] -0.01210893

    mean.A2

    ## [1] -0.3066046

    set.seed(6)
    B1 <- sample(B, size = 100)
    set.seed(7)
    B2 <- sample(B, size = 100)
    mean.B1 <- mean(B1)
    mean.B2 <- mean(B2)
    mean.B1

    ## [1] 24.95

    mean.B2

    ## [1] 25.13

    set.seed(8)
    C1 <- sample(C, size = 100)
    set.seed(9)
    C2 <- sample(C, size = 100)
    mean.C1 <- mean(C1)
    mean.C2 <- mean(C2)
    mean.C1

    ## [1] 9.75

    mean.C2

    ## [1] 10.16

No the sample means are not equal

**iii)Follow the steps for illustrating the Central Limit Theorem using
a Poisson and a uniform distribution.**

    unif.10 <- data.table(replicate(n=10000, runif(10)))
    mean.unif.10 <- colMeans(unif.10)

    unif.100 <- data.table(replicate(n=10000, runif(100)))
    mean.unif.100 <- colMeans(unif.100)

    par(mfrow=c(1,2))
    hist(mean.unif.10
         , probability=FALSE
         , main="Sample of 10"
         , xlim=c(0.2,0.8)
         , ylim=c(0,800)
         , breaks = 50)
    hist(mean.unif.100
         , probability=FALSE
         , main="Sample of 100"
         , xlim=c(0.2,0.8)
         , ylim=c(0,800)
         , breaks = 50)

![uniform-1](https://user-images.githubusercontent.com/73550706/97801672-56806980-1c36-11eb-9ec7-59237add8456.png)

    pois.10 <- data.table(replicate(n=10000, rpois(10,5)))
    mean.pois.10 <- colMeans(pois.10)

    pois.100 <- data.table(replicate(n=10000, rpois(100,5)))
    mean.pois.100 <- colMeans(pois.100)

    par(mfrow=c(1,2))
    hist(mean.pois.10
         , probability=FALSE
         , main="Sample of 10"
         , xlim=c(3,7)
         , ylim=c(0,800)
         , breaks = 50)
    hist(mean.pois.100
         , probability=FALSE
         , main="Sample of 100"
         , xlim=c(3,7)
         , ylim=c(0,800)
         , breaks = 50)

![pois-1](https://user-images.githubusercontent.com/73550706/97801664-45cff380-1c36-11eb-99f5-686d59594b2a.png)

    norm.10 <- data.table(replicate(n=10000, rnorm(10)))
    mean.norm.10 <- colMeans(norm.10)

    norm.100 <- data.table(replicate(n=10000, rnorm(100)))
    mean.norm.100 <- colMeans(norm.100)

    par(mfrow=c(1,2))
    hist(mean.norm.10
         , probability=FALSE
         , main="Sample of 10"
         , xlim=c(-1.5,1.5)
         , ylim=c(0,800)
         , breaks = 50)
    hist(mean.norm.100
         , probability=FALSE
         , main="Sample of 100"
         , xlim=c(-1.5,1.5)
         , ylim=c(0,800)
         , breaks = 50)

![normal-1](https://user-images.githubusercontent.com/73550706/97801636-14572800-1c36-11eb-9e88-2fda8c836cec.png)

The variance for all the Samples of 100 is much smaller

Part 2: Testing
---------------

    load("datasets/dt_wages.RData")
    dt.wages1 <- data.table(dt.wages)
    rm(dt.wages)

**i)Can you answer the question of whether there is discrimination in
the labor market? How would you go about answering the question? What
can you say about the unemployed?**

Let’s estimate the sample mean

    dt.wages1[, list(avg_wage=mean(wage))]

    ##    avg_wage
    ## 1: 5.896103

Now calculate sample mean by group

    dt.wages1[, list(avg_wage=mean(wage)), by=nonwhite]

    ##    nonwhite avg_wage
    ## 1:        0 5.944174
    ## 2:        1 5.475926

    dt.wages1[, list(avg_wage=mean(wage)), by=female]

    ##    female avg_wage
    ## 1:      1 4.587659
    ## 2:      0 7.099489

There are no unemployed people in this sample so we cannot say anything
about them.

### Confidence Intervals

Let’s see what average wage and standard deviation are:

    dt.wages1[, list(avg_wage=mean(wage), sd_wage=sd(wage))]

    ##    avg_wage  sd_wage
    ## 1: 5.896103 3.693086

Now create a function to calculate the 95% confidence interval

    conf.int <- function(X){
      n <- length (X)
      error <- qt(0.975, df=n-1) * sd(X) / sqrt (n)
      mean.X <- mean(X)
      return (list( lower = mean.X- error, upper = mean.X + error ))
    }

Finally, apply the function to the wage variable

    dt.wages1[, conf.int(wage)]

    ##       lower    upper
    ## 1: 5.579768 6.212437

Then calculate condfidence intervals by group

    dt.wages1[, conf.int(wage), by=nonwhite]

    ##    nonwhite    lower    upper
    ## 1:        0 5.605032 6.283316
    ## 2:        1 4.614661 6.337191

    dt.wages1[, conf.int(wage), by=female]

    ##    female    lower    upper
    ## 1:      1 4.273855 4.901462
    ## 2:      0 6.604626 7.594352

To see if wages are different for men and women, we can use hypothesis
testing

    dt.wages1[, t.test (wage ~ female)]

    ## 
    ##  Welch Two Sample t-test
    ## 
    ## data:  wage by female
    ## t = 8.44, df = 456.33, p-value = 4.243e-16
    ## alternative hypothesis: true difference in means is not equal to 0
    ## 95 percent confidence interval:
    ##  1.926971 3.096690
    ## sample estimates:
    ## mean in group 0 mean in group 1 
    ##        7.099489        4.587659

Similarly, to see if wages are different for whites and non-whites

    dt.wages1[, t.test (wage ~ nonwhite)]

    ## 
    ##  Welch Two Sample t-test
    ## 
    ## data:  wage by nonwhite
    ## t = 1.0118, df = 71.298, p-value = 0.3151
    ## alternative hypothesis: true difference in means is not equal to 0
    ## 95 percent confidence interval:
    ##  -0.4544541  1.3909497
    ## sample estimates:
    ## mean in group 0 mean in group 1 
    ##        5.944174        5.475926
