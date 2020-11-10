---
layout: post
title:  "Econometrics Lab 7"
date:   2020-11-02 11:00:00
categories: jekyll update
comment_issue_id: 48
---

##Setup

Set working directory and activate necessary packages

    setwd("~/Desktop/R WD")

    library(stargazer)

    ## 
    ## Please cite as:

    ##  Hlavac, Marek (2018). stargazer: Well-Formatted Regression and Summary Statistics Tables.

    ##  R package version 5.2.2. https://CRAN.R-project.org/package=stargazer

    library(data.table)
    library(lmtest)

    ## Loading required package: zoo

    ## 
    ## Attaching package: 'zoo'

    ## The following objects are masked from 'package:base':
    ## 
    ##     as.Date, as.Date.numeric

    library(sandwich)

### Violation of Simple Linear Regression Assumption 2

Create data

    set.seed(1984)
    x1 <- rnorm(n = 10000, mean = 0 , sd = 3) # create indep. var. 1
    x2 <- rnorm(n = 10000, mean = 0, sd = 4) # create indep. var. 2
    e <- rnorm(n = 10000, mean = 0, sd = 2) # create error
    y <- 2 + 3*x1 + 4*x2 + e # create y according to population model

Define dataframe

    dt.population <- data.table( y, x1, x2, e) # creates tables
    dt.population <- dt.population[order(y)]
    dt.population # shows first and last entries of table

    ##                y         x1         x2          e
    ##     1: -65.51931 -8.9425248 -10.925706  3.0110909
    ##     2: -64.08313  0.3982382 -16.079049 -2.9616476
    ##     3: -62.40373 -7.5047649 -10.113220 -1.4365531
    ##     4: -58.28314 -3.0228161 -12.841153  0.1499192
    ##     5: -57.83996 -6.7402440  -9.934788  0.1199243
    ##    ---                                           
    ##  9996:  63.23389  5.1128133  10.935504  2.1534348
    ##  9997:  63.99458  7.7187598  10.025892 -1.2652619
    ##  9998:  67.18546  6.1780560  11.427169  0.9426169
    ##  9999:  67.34564  5.1032742  11.854979  2.6158987
    ## 10000:  67.38101  3.9659545  13.253237  0.4701935

Extract a random sample

    r.sample.rows <- sample(1:nrow(dt.population), size = 100)
    r.sample.rows # shows the vector of 100 randomly selected row numbers

    ##   [1] 9312 6165 6458 6477 5188  406 2820  572  652 5549 9072 3968 2815 7752 4184
    ##  [16] 9956 3045 6106 1921 3925 6653 9426 9697 8545 7052 2314 8746 5711 4654 7588
    ##  [31] 3315 4286 2542 4340 4799  186 2661 4487 5296 2770 4991 8833 5568 4317 7128
    ##  [46] 3859 5196 1072 6460 3063 8471 2182 7634  727 8534 8819  597 4368 4776 4845
    ##  [61] 4483 9155 3458 4571 7679 4408 2644 4230 7691 1020 4918 4983 6298 5278 3204
    ##  [76]  976  758 8705 2609 4244 6084 5799 5596 9210 8573 6431 7388  388 6286 7904
    ##  [91] 2573 3262 6379 3460 8385 9613 8025 7927 4496 2157

    r.sample <- dt.population[r.sample.rows,] # select the rows according to random sample
    head(r.sample) # show selection (note that the row numbers are the ones that were randomly selected above

    ##             y         x1        x2          e
    ## 1:  29.432379 -2.4933338  8.455335  1.0910400
    ## 2:   7.430384 -1.7219022  3.029325 -1.5212113
    ## 3:   8.966529  0.9625825  1.160981 -0.5651421
    ## 4:   9.036588 -1.4417106  2.091823  2.9944287
    ## 5:   3.144875  1.9221459 -0.930215 -0.9007030
    ## 6: -30.386939 -0.6415410 -7.397989 -0.8703582

    tail(r.sample)

    ##              y         x1         x2         e
    ## 1:  19.9215164  3.4279127  1.1952087  2.856943
    ## 2:  34.9533953  0.6946097  6.9837498  2.934567
    ## 3:  17.5075346 -3.5215802  5.9829736  2.140381
    ## 4:  16.9222357  4.2822281  0.7916681 -1.091121
    ## 5:   0.1280741 -0.9962701  0.3329119 -0.214763
    ## 6: -11.9797443 -0.4942824 -2.7167980 -1.629705

Run regression on random sample

    summary(lm( y ~ x1 + x2, data = r.sample))

    ## 
    ## Call:
    ## lm(formula = y ~ x1 + x2, data = r.sample)
    ## 
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -5.1047 -1.4439 -0.0498  1.1808  5.9282 
    ## 
    ## Coefficients:
    ##             Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)  1.90817    0.20551   9.285 4.69e-15 ***
    ## x1           3.00848    0.07517  40.025  < 2e-16 ***
    ## x2           4.09883    0.05554  73.796  < 2e-16 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 2.049 on 97 degrees of freedom
    ## Multiple R-squared:  0.9841, Adjusted R-squared:  0.9838 
    ## F-statistic:  3008 on 2 and 97 DF,  p-value: < 2.2e-16

Extract a non-random sample

    nr.sample <- dt.population[1:100,] # select a subset of the first 100 rows
    head(nr.sample)

    ##            y         x1         x2          e
    ## 1: -65.51931 -8.9425248 -10.925706  3.0110909
    ## 2: -64.08313  0.3982382 -16.079049 -2.9616476
    ## 3: -62.40373 -7.5047649 -10.113220 -1.4365531
    ## 4: -58.28314 -3.0228161 -12.841153  0.1499192
    ## 5: -57.83996 -6.7402440  -9.934788  0.1199243
    ## 6: -57.68156 -5.4035347 -10.977426  0.4387483

    
Run regression on non-random sample

    summary(lm( y ~ x1 + x2, data = nr.sample))

    ## 
    ## Call:
    ## lm(formula = y ~ x1 + x2, data = nr.sample)
    ## 
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -4.4988 -1.2392 -0.0186  1.2096  3.9580 
    ## 
    ## Coefficients:
    ##             Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)  -5.7973     1.5566  -3.724 0.000329 ***
    ## x1            2.5723     0.1078  23.868  < 2e-16 ***
    ## x2            3.4089     0.1294  26.352  < 2e-16 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 1.744 on 97 degrees of freedom
    ## Multiple R-squared:  0.8814, Adjusted R-squared:  0.8789 
    ## F-statistic: 360.3 on 2 and 97 DF,  p-value: < 2.2e-16

###Violation of Simple Linear Regression Assumption 3 
**SLR.3: The sample outcomes x ,xi, i = 1,. . . ,n are not all the same value.**

**Colinearity: The variable x2 is a constant.**

    set.seed(1984)
    x1 <- rnorm(n = 1000, mean = 0 , sd = 2) # create indep. var. 1
    x2 <- rep(3, times=1000) # create indep. var. 2 - a constant
    e <- rnorm(n = 1000, mean = 0, sd = 1) # create error
    y <- 2 + 3*x1 + 4*x2 + e # create y according to population model

Run regression including constant

    summary(lm( y ~ x1 + x2 ))

    ## 
    ## Call:
    ## lm(formula = y ~ x1 + x2)
    ## 
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -3.3114 -0.6618  0.0197  0.6507  3.6707 
    ## 
    ## Coefficients: (1 not defined because of singularities)
    ##             Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept) 13.98124    0.03209   435.7   <2e-16 ***
    ## x1           2.99665    0.01620   185.0   <2e-16 ***
    ## x2                NA         NA      NA       NA    
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 1.014 on 998 degrees of freedom
    ## Multiple R-squared:  0.9717, Adjusted R-squared:  0.9716 
    ## F-statistic: 3.423e+04 on 1 and 998 DF,  p-value: < 2.2e-16

**Very high colinearity: x1 and x2 are very highly correlated.**

    set.seed(1984)
    x1 <- rnorm(n = 1000, mean = 0 , sd = 2) # create indep. var. 1
    x2 <- 0.4*x1 + rnorm(n=1000, mean = 0, sd =0.01) # create indep. var. 2 - mean = x1
    e <- rnorm(n = 1000, mean = 0, sd = 1) # create error
    y <- 2 + 3*x1 + 4*x2 + e # create y according to population model
    summary(lm( y ~ x1 + x2 ))

    ## 
    ## Call:
    ## lm(formula = y ~ x1 + x2)
    ## 
    ## Residuals:
    ##      Min       1Q   Median       3Q      Max 
    ## -2.84702 -0.68167 -0.01766  0.69449  2.81105 
    ## 
    ## Coefficients:
    ##             Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)  2.02596    0.03151  64.300   <2e-16 ***
    ## x1           2.18183    1.24299   1.755   0.0795 .  
    ## x2           6.02423    3.10749   1.939   0.0528 .  
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 0.9954 on 997 degrees of freedom
    ## Multiple R-squared:  0.9882, Adjusted R-squared:  0.9882 
    ## F-statistic: 4.169e+04 on 2 and 997 DF,  p-value: < 2.2e-16

###Violation of Simple Linear Regression Assumption 4

**SLR.4: E(u|x)=0 (the error u has an expected value of zero given any
value of the explanatory variable)**

    set.seed(1984)
    x1 <- rnorm(n = 1000, mean = 0 , sd = 3) # create indep. var. 1
    x2 <- rnorm(n = 1000, mean = x1 , sd = 5) # create indep. var. 2
    e <- rnorm(n = 1000, mean = 0, sd = 1)

    plot(x1,x2) # plot x1 against x2

    cor.test(x = x1, y = x2)

    y <- 2 + 3*x1 + 4*x2 + e # create y according to population model
    out.y.full <- lm ( y ~ x1 + x2) # full model
    out.y.x1.om <- lm ( y ~ x1) # model with x2 ommitted
    stargazer(out.y.full, out.y.x1.om, type="text")

    ## 
    ## ===========================================================================
    ##                                       Dependent variable:                  
    ##                     -------------------------------------------------------
    ##                                                y                           
    ##                                 (1)                         (2)            
    ## ---------------------------------------------------------------------------
    ## x1                            2.990***                    6.950***         
    ##                               (0.012)                     (0.216)          
    ##                                                                            
    ## x2                            4.004***                                     
    ##                               (0.006)                                      
    ##                                                                            
    ## Constant                      2.026***                    1.650**          
    ##                               (0.032)                     (0.643)          
    ##                                                                            
    ## ---------------------------------------------------------------------------
    ## Observations                   1,000                       1,000           
    ## R2                             0.999                       0.508           
    ## Adjusted R2                    0.999                       0.508           
    ## Residual Std. Error       0.995 (df = 997)           20.324 (df = 998)     
    ## F Statistic         422,429.000*** (df = 2; 997) 1,030.926*** (df = 1; 998)
    ## ===========================================================================
    ## Note:                                           *p<0.1; **p<0.05; ***p<0.01

**If x1 and x2 are negatively correlated and 2 is negative, then if we
omit x2 from the model beta1 will be positively biased.**

    set.seed(1984)
    x1 <- rnorm(n = 1000, mean = 0 , sd = 3) # create indep. var. 1
    x2 <- rnorm(n = 1000, mean = -x1 , sd = 5) # create indep. var. 2
    plot(x1,x2) # plot x1 against x2
    
![seed6-1](https://user-images.githubusercontent.com/73550706/98686947-14a3a180-2361-11eb-8d0c-951db1eee1f4.png)


    cor.test(x = x1, y = x2)

    ## 
    ##  Pearson's product-moment correlation
    ## 
    ## data:  x1 and x2
    ## t = -18.728, df = 998, p-value < 2.2e-16
    ## alternative hypothesis: true correlation is not equal to 0
    ## 95 percent confidence interval:
    ##  -0.5544216 -0.4625868
    ## sample estimates:
    ##        cor 
    ## -0.5099558

    e <- rnorm(n = 1000, mean = 0, sd = 1) # create error
    y <- 2 + 3*x1 - 4*x2 + e # create y according to population model
    out.y.full <- lm ( y ~ x1 + x2) # full model
    out.y.x1.om <- lm ( y ~ x1) # model with x2 ommitted
    stargazer(out.y.full, out.y.x1.om, type="text")

    ## 
    ## ===========================================================================
    ##                                       Dependent variable:                  
    ##                     -------------------------------------------------------
    ##                                                y                           
    ##                                 (1)                         (2)            
    ## ---------------------------------------------------------------------------
    ## x1                            2.998***                    7.039***         
    ##                               (0.012)                     (0.216)          
    ##                                                                            
    ## x2                           -3.996***                                     
    ##                               (0.006)                                      
    ##                                                                            
    ## Constant                      2.026***                    2.401***         
    ##                               (0.032)                     (0.642)          
    ##                                                                            
    ## ---------------------------------------------------------------------------
    ## Observations                   1,000                       1,000           
    ## R2                             0.999                       0.516           
    ## Adjusted R2                    0.999                       0.515           
    ## Residual Std. Error       0.995 (df = 997)           20.283 (df = 998)     
    ## F Statistic         427,153.100*** (df = 2; 997) 1,061.892*** (df = 1; 998)
    ## ===========================================================================
    ## Note:                                           *p<0.1; **p<0.05; ***p<0.01

**If x1 and x2 are positively correlated and 2 is negative, then if we
omit x2 from the model betaˆ1 will be negatively biased.**

    set.seed(1984)
    x1 <- rnorm(n = 1000, mean = 0 , sd = 3) # create indep. var. 1
    x2 <- rnorm(n = 1000, mean = x1/3 , sd = 5)
    cor.test(x = x1, y = x2)

    ## 
    ##  Pearson's product-moment correlation
    ## 
    ## data:  x1 and x2
    ## t = 5.9669, df = 998, p-value = 3.355e-09
    ## alternative hypothesis: true correlation is not equal to 0
    ## 95 percent confidence interval:
    ##  0.1250425 0.2447737
    ## sample estimates:
    ##       cor 
    ## 0.1855969

    e <- rnorm(n = 1000, mean = 0, sd = 1) # create error
    y <- 2 + 3*x1 - 4*x2 + e # create y according to population model
    out.y.full <- lm ( y ~ x1 + x2) # full model
    out.y.x1.om <- lm ( y ~ x1) # model with x2 ommitted
    stargazer(out.y.full, out.y.x1.om, type="text")

    ## 
    ## ========================================================================
    ##                                     Dependent variable:                 
    ##                     ----------------------------------------------------
    ##                                              y                          
    ##                                 (1)                        (2)          
    ## ------------------------------------------------------------------------
    ## x1                            2.993***                  1.706***        
    ##                               (0.011)                    (0.216)        
    ##                                                                         
    ## x2                           -3.996***                                  
    ##                               (0.006)                                   
    ##                                                                         
    ## Constant                      2.026***                  2.401***        
    ##                               (0.032)                    (0.642)        
    ##                                                                         
    ## ------------------------------------------------------------------------
    ## Observations                   1,000                      1,000         
    ## R2                             0.998                      0.059         
    ## Adjusted R2                    0.998                      0.058         
    ## Residual Std. Error       0.995 (df = 997)          20.283 (df = 998)   
    ## F Statistic         219,639.600*** (df = 2; 997) 62.351*** (df = 1; 998)
    ## ========================================================================
    ## Note:                                        *p<0.1; **p<0.05; ***p<0.01

Compute the bias

    out.y.full <- lm ( y ~ x1 + x2)
    coeffs.full <- coefficients(out.y.full)
    b2_hat <- coeffs.full[3]
    b1_hat <- coeffs.full[2]

    out.part.x2 <- lm ( x2 ~ x1)
    coeffs.part <- coefficients(out.part.x2)
    delta <- coeffs.part[2]

    bias <- delta*b2_hat
    bias

    ##        x1 
    ## -1.287344
