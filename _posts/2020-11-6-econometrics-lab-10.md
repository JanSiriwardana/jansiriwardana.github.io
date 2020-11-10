---
layout: post
title:  "Econometrics Lab 10"
date:   2020-11-06 11:00:00
categories: jekyll update
comment_issue_id: 41
---

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

    load("datasets/dt_wages.RData")

Part 2
------

### Descriptive Statistics

    dt.wages <- data.table(dt.wages)
    stargazer(dt.wages, type = "text")

    ## 
    ## ==============================================================
    ## Statistic  N   Mean   St. Dev.  Min   Pctl(25) Pctl(75)  Max  
    ## --------------------------------------------------------------
    ## wage      526  5.896   3.693   0.530   3.330    6.880   24.980
    ## educ      526 12.563   2.769     0       12       14      18  
    ## exper     526 17.017   13.572    1       5        26      51  
    ## tenure    526  5.105   7.224     0       0        7       44  
    ## nonwhite  526  0.103   0.304     0       0        0       1   
    ## female    526  0.479   0.500     0       0        1       1   
    ## married   526  0.608   0.489     0       0        1       1   
    ## numdep    526  1.044   1.262     0       0        2       6   
    ## smsa      526  0.722   0.448     0       0        1       1   
    ## northcen  526  0.251   0.434     0       0       0.8      1   
    ## south     526  0.356   0.479     0       0        1       1   
    ## west      526  0.169   0.375     0       0        0       1   
    ## construc  526  0.046   0.209     0       0        0       1   
    ## ndurman   526  0.114   0.318     0       0        0       1   
    ## trcommpu  526  0.044   0.205     0       0        0       1   
    ## trade     526  0.287   0.453     0       0        1       1   
    ## services  526  0.101   0.301     0       0        0       1   
    ## profserv  526  0.259   0.438     0       0        1       1   
    ## profocc   526  0.367   0.482     0       0        1       1   
    ## clerocc   526  0.167   0.374     0       0        0       1   
    ## servocc   526  0.141   0.348     0       0        0       1   
    ## lwage     526  1.623   0.532   -0.635  1.203    1.929   3.218 
    ## expersq   526 473.435 616.045    1       25      676    2,601 
    ## tenursq   526 78.150  199.435    0       0        49    1,936 
    ## --------------------------------------------------------------

    ncol(dt.wages)

    ## [1] 24

There are 526 observations of 24 variables

### Tables

    table(dt.wages[, list(female, nonwhite)])

    ##       nonwhite
    ## female   0   1
    ##      0 245  29
    ##      1 227  25

    table(dt.wages[, list(female, nonwhite, south)])

    ## , , south = 0
    ## 
    ##       nonwhite
    ## female   0   1
    ##      0 158  13
    ##      1 154  14
    ## 
    ## , , south = 1
    ## 
    ##       nonwhite
    ## female   0   1
    ##      0  87  16
    ##      1  73  11

    qplot(factor(educ), data=dt.wages, geom = "bar")

![](Lab-10_files/figure-markdown_strict/graphs-1.png)

    qplot(wage, data=dt.wages, geom = "histogram")

    ## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.

![](Lab-10_files/figure-markdown_strict/graphs-2.png)

    qplot(wage, data=dt.wages, geom = "histogram", binwidth=2)

![](Lab-10_files/figure-markdown_strict/graphs-3.png)

### Continuous vs Categorical Data

    qplot(x=factor("All"), y=wage, data=dt.wages, geom = "boxplot")

![](Lab-10_files/figure-markdown_strict/graphs2-1.png)

    qplot(x=factor(female), y=wage, data=dt.wages, geom = "boxplot")

![](Lab-10_files/figure-markdown_strict/graphs2-2.png)

    qplot (factor(female),wage, fill=factor(nonwhite), data=dt.wages, geom="boxplot")

![](Lab-10_files/figure-markdown_strict/graphs2-3.png)

    qplot (factor(nonwhite),wage, fill=factor(female), data=dt.wages, geom="boxplot")

![](Lab-10_files/figure-markdown_strict/graphs2-4.png)

    ggplot(dt.wages) + geom_boxplot(aes(factor(educ), exper))

![](Lab-10_files/figure-markdown_strict/graphs2-5.png)

    qplot(educ, wage, data=dt.wages, geom = "point")

![](Lab-10_files/figure-markdown_strict/graphs2-6.png)

    qplot(educ, wage, color=factor(female), data=dt.wages, geom = "point")

![](Lab-10_files/figure-markdown_strict/graphs2-7.png)

    ggplot(dt.wages) + geom_point(aes(educ, wage)) + facet_grid( ~ female)

![](Lab-10_files/figure-markdown_strict/graphs2-8.png)

    ggplot(dt.wages) + geom_point(aes(educ, wage)) + facet_grid(nonwhite ~ female)

![](Lab-10_files/figure-markdown_strict/graphs2-9.png)

    ggpairs(dt.wages[, list(wage, educ, exper)])

![](Lab-10_files/figure-markdown_strict/graphs2-10.png)

    ggpairs(dt.wages[, list(female=factor(female), nonwhite=factor(nonwhite))]) 

![](Lab-10_files/figure-markdown_strict/graphs2-11.png)

    ggpairs(dt.wages[, list(wage, educ, exper, female=factor(female), nonwhite=factor(nonwhite))]) 

    ## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.
    ## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.
    ## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.
    ## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.
    ## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.
    ## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.

![](Lab-10_files/figure-markdown_strict/graphs2-12.png)

### Outliers

    dt.wages[wage>20,][order(-wage)]

    ##     wage educ exper tenure nonwhite female married numdep smsa northcen south
    ## 1: 24.98   18    29     25        0      0       1      0    1        0     0
    ## 2: 22.86   16    16      7        0      0       1      2    1        0     0
    ## 3: 22.20   12    31     15        0      0       1      1    1        0     0
    ## 4: 21.86   12    24     16        0      0       1      3    1        1     0
    ## 5: 21.63   18     8      8        0      1       0      0    1        0     0
    ##    west construc ndurman trcommpu trade services profserv profocc clerocc
    ## 1:    0        0       0        0     0        0        0       1       0
    ## 2:    0        0       0        0     0        0        0       1       0
    ## 3:    1        0       0        0     0        0        0       1       0
    ## 4:    0        0       0        0     1        0        0       1       0
    ## 5:    0        0       0        0     0        0        1       1       0
    ##    servocc    lwage expersq tenursq
    ## 1:       0 3.218076     841     625
    ## 2:       0 3.129389     256      49
    ## 3:       0 3.100092     961     225
    ## 4:       0 3.084659     576     256
    ## 5:       0 3.074081      64      64

    qplot(lwage, data=dt.wages, geom = "histogram")

    ## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.

![](Lab-10_files/figure-markdown_strict/outliers-1.png)

    dt.wages[lwage<0,]

    ##    wage educ exper tenure nonwhite female married numdep smsa northcen south
    ## 1: 0.53   12     3      1        0      1       0      0    1        0     0
    ##    west construc ndurman trcommpu trade services profserv profocc clerocc
    ## 1:    1        0       0        0     0        1        0       0       0
    ##    servocc      lwage expersq tenursq
    ## 1:       1 -0.6348783       9       1

Part 3
------

### Difference in means estimator when treatment is ‘south’

Mean wage of treated (south=1) - mean wage of untreated (south=0) with second term as approximation for counterfactual.

    dt.wages[south==1, mean(wage)] - dt.wages[south==0, mean(wage)]

    ## [1] -0.7900927

### Regression of treatment effects with race and gender as controls

If we simply regress wage on south, there is a problem of omitted variables. Clearly other factors effect wage other than whether an individual is from the south or not. Omitting variables that determine both treatment status and the outcome variable generates a correlation between the treatment variable and the error term. Therefore we should include observed covariates.

Here, we simply add race and gender variables to a regression of outcome variable (wage) on treatment variable (south).

    library(matlib)

    treat.reg <- lm(wage ~ south + nonwhite + female, data=dt.wages)
    stargazer(treat.reg, type = "text")

    ## 
    ## ===============================================
    ##                         Dependent variable:    
    ##                     ---------------------------
    ##                                wage            
    ## -----------------------------------------------
    ## south                        -0.884***         
    ##                               (0.317)          
    ##                                                
    ## nonwhite                      -0.372           
    ##                               (0.499)          
    ##                                                
    ## female                       -2.552***         
    ##                               (0.302)          
    ##                                                
    ## Constant                     7.471***          
    ##                               (0.243)          
    ##                                                
    ## -----------------------------------------------
    ## Observations                    526            
    ## R2                             0.130           
    ## Adjusted R2                    0.125           
    ## Residual Std. Error      3.454 (df = 522)      
    ## F Statistic           26.104*** (df = 3; 522)  
    ## ===============================================
    ## Note:               *p<0.1; **p<0.05; ***p<0.01

### Heterogeneous treatment effects (Regression adjustment)

So far we have assumed that the treatment effects are homogenous. That is, for each individual that receives treatment, the effect of the treatment on the outcome variable is the same. In this case the difference between ATE and ATET is trivial.

It may be reasonable to assume that the treatment effects is in fact heterogeneous. In order to do so we can add interaction terms between the covariates and the treatment dummy.

    RA <- lm(wage ~ south + nonwhite + female + nonwhite*south + female*south, data=dt.wages)
    stargazer(RA, type = "text") 

    ## 
    ## ===============================================
    ##                         Dependent variable:    
    ##                     ---------------------------
    ##                                wage            
    ## -----------------------------------------------
    ## south                        -1.288***         
    ##                               (0.447)          
    ##                                                
    ## nonwhite                       0.155           
    ##                               (0.691)          
    ##                                                
    ## female                       -2.953***         
    ##                               (0.374)          
    ##                                                
    ## south:nonwhite                -1.047           
    ##                               (0.996)          
    ##                                                
    ## south:female                  1.117*           
    ##                               (0.630)          
    ##                                                
    ## Constant                     7.628***          
    ##                               (0.269)          
    ##                                                
    ## -----------------------------------------------
    ## Observations                    526            
    ## R2                             0.138           
    ## Adjusted R2                    0.129           
    ## Residual Std. Error      3.446 (df = 520)      
    ## F Statistic           16.591*** (df = 5; 520)  
    ## ===============================================
    ## Note:               *p<0.1; **p<0.05; ***p<0.01

    south.nonwhite <- dt.wages[south==1, mean(nonwhite)] #mean wage of nonwhites in south
    south.female <- dt.wages[south==1, mean(female)] #mean wage of females in south

**ATE estimate**

![equation1](https://latex.codecogs.com/gif.latex?%5Cbeta_%7BATE%7D%3D%5Cbeta_1%20&plus;%20E%5Bb%28X_%7Bit%7D%29%5D) where ![equation1](https://latex.codecogs.com/gif.latex?%5Cbeta_1) is the regression coefficient on the treatment dummy and the second part is the expected value of the covariate multiplied by the coefficient on the corresponding interaction variable.

    summary(RA)$coefficients[2,1] + (dt.wages[, mean(nonwhite)]*summary(RA)$coefficients[5,1]) + (dt.wages[, mean(female)]*summary(RA)$coefficients[6,1])

    ## [1] -0.8600141

**ATET estimate**

![equation1] (https://latex.codecogs.com/gif.latex?%5Cbeta_%7BATET%7D%3D%5Cbeta_1%20&plus;%20E%5Bb%28X_%7Bit%7D%29%7CD_%7Bit%7D%3D1%5D) where the second part is the expected value of the covariate, given the individual was treated, multiplied by the coefficient on the corresponding interaction variable.

    summary(RA)$coefficients[2,1] + (south.nonwhite*summary(RA)$coefficients[5,1]) + (south.female*summary(RA)$coefficients[6,1])

    ## [1] -0.9370693

### Two-step fitted regression

First carry out seperate regressions on the treatment group and the non-treatment group.

(The first two lines of code create subsets from the data table of treated (south.1) and untreated (south.0) groups)

NB. we don't need to add the treatment dummy to the regressions

    south.1 <- subset(dt.wages, south==1)
    south.0 <- subset(dt.wages, south==0)
    
    twostep.1 <- lm(wage ~ nonwhite + female, data=south.1)
    twostep.0 <- lm(wage ~ nonwhite + female, data=south.0)

    stargazer(twostep.1, twostep.0, type = "text")

    ## 
    ## ==================================================================
    ##                                  Dependent variable:              
    ##                     ----------------------------------------------
    ##                                          wage                     
    ##                              (1)                     (2)          
    ## ------------------------------------------------------------------
    ## nonwhite                    -0.892                  0.155         
    ##                            (0.617)                 (0.739)        
    ##                                                                   
    ## female                    -1.836***               -2.953***       
    ##                            (0.436)                 (0.400)        
    ##                                                                   
    ## Constant                   6.341***               7.628***        
    ##                            (0.307)                 (0.287)        
    ##                                                                   
    ## ------------------------------------------------------------------
    ## Observations                 187                     339          
    ## R2                          0.096                   0.139         
    ## Adjusted R2                 0.086                   0.134         
    ## Residual Std. Error    2.963 (df = 184)       3.684 (df = 336)    
    ## F Statistic         9.724*** (df = 2; 184) 27.234*** (df = 2; 336)
    ## ==================================================================
    ## Note:                                  *p<0.1; **p<0.05; ***p<0.01

Next, using the 2 models, predict wages for our full sample 

    library(Metrics)

    dt.wages$y_hat1 <- predict(twostep.1, newdata=dt.wages)
    dt.wages$y_hat0 <- predict(twostep.0, newdata=dt.wages)

**ATE estimate**

![equation](https://latex.codecogs.com/gif.latex?%5Ctext%7BATE%7D%20%3D%20%5Cfrac%7B1%7D%7Bn%7D%5Csum%5En_%7Bi%3D1%7D%5Chat%20Y_%7B1i%7D-%20%5Chat%20Y_%7B0i%7D)

    mean(dt.wages$y_hat1 - dt.wages$y_hat0)

    ## [1] -0.8600141

**ATET estimate**

![equation](https://latex.codecogs.com/gif.latex?%5Ctext%7BATET%7D%20%3D%20%5Cleft%28%5Cfrac%7B1%7D%7Bn%7D%5Csum%5En_%7Bi%3D1%7DD_i%28%5Chat%20Y_%7B1i%7D-%20%5Chat%20Y_%7B0i%7D%29%5Cright%29/%5Cfrac%7B1%7D%7Bn%7D%5Csum%5En_%7Bi%3D1%7DD_i)

    mean(dt.wages$south*(dt.wages$y_hat1 - dt.wages$y_hat0))/mean(dt.wages$south)

    ## [1] -0.9370693

### Regression with experience added

    exp.reg <- lm(wage ~ south + nonwhite + female + exper, data=dt.wages)

**Heterogenous treatment effects** using two step fitted regression

    exptwostep.1 <- lm(wage ~ nonwhite + female + exper, data=south.1)
    exptwostep.0 <- lm(wage ~ nonwhite + female + exper, data=south.0)

    dt.wages$y_hat2 <- predict(exptwostep.1, newdata=dt.wages)
    dt.wages$y_hat3 <- predict(exptwostep.0, newdata=dt.wages)

**ATE estimate**

    mean(dt.wages$y_hat2 - dt.wages$y_hat3)

    ## [1] -0.8723571

**ATET estimate**

    mean(dt.wages$south*(dt.wages$y_hat2 - dt.wages$y_hat3))/mean(dt.wages$south)

    ## [1] -1.006724
