---
layout: post
title:  "Econometrics Lab 12"
date:   2020-11-09 11:00:00
categories: jekyll update
comment_issue_id: 54
---

    setwd("~/Desktop/R WD")
    library(data.table)

### Matrix Manipulation

    df.OLSdat <- read.table("datasets/OLSqData.RData")
    DT <- data.table(df.OLSdat) 

    summary(df.OLSdat)

    ##        y1               const         x1                   x2           
    ##  Min.   :-11.7353   Min.   :1   Min.   :-1.0813748   Min.   :-3.626672  
    ##  1st Qu.: -0.2118   1st Qu.:1   1st Qu.:-0.2214267   1st Qu.:-0.686488  
    ##  Median :  2.2171   Median :1   Median : 0.0001322   Median : 0.000400  
    ##  Mean   :  2.1698   Mean   :1   Mean   :-0.0038033   Mean   :-0.006917  
    ##  3rd Qu.:  4.5833   3rd Qu.:1   3rd Qu.: 0.2136286   3rd Qu.: 0.670995  
    ##  Max.   : 17.5787   Max.   :1   Max.   : 1.1481061   Max.   : 4.024930  
    ##        x3                eps1               eps2         
    ##  Min.   :-3.75367   Min.   :-5.86698   Min.   :-53.6324  
    ##  1st Qu.:-0.69583   1st Qu.:-0.95432   1st Qu.: -9.4102  
    ##  Median :-0.03440   Median : 0.03125   Median :  0.1831  
    ##  Mean   :-0.02018   Mean   : 0.01829   Mean   :  0.1411  
    ##  3rd Qu.: 0.66101   3rd Qu.: 0.97468   3rd Qu.:  9.8168  
    ##  Max.   : 3.77398   Max.   : 5.35372   Max.   : 59.9260

    summary(DT)

    ##        y1               const         x1                   x2           
    ##  Min.   :-11.7353   Min.   :1   Min.   :-1.0813748   Min.   :-3.626672  
    ##  1st Qu.: -0.2118   1st Qu.:1   1st Qu.:-0.2214267   1st Qu.:-0.686488  
    ##  Median :  2.2171   Median :1   Median : 0.0001322   Median : 0.000400  
    ##  Mean   :  2.1698   Mean   :1   Mean   :-0.0038033   Mean   :-0.006917  
    ##  3rd Qu.:  4.5833   3rd Qu.:1   3rd Qu.: 0.2136286   3rd Qu.: 0.670995  
    ##  Max.   : 17.5787   Max.   :1   Max.   : 1.1481061   Max.   : 4.024930  
    ##        x3                eps1               eps2         
    ##  Min.   :-3.75367   Min.   :-5.86698   Min.   :-53.6324  
    ##  1st Qu.:-0.69583   1st Qu.:-0.95432   1st Qu.: -9.4102  
    ##  Median :-0.03440   Median : 0.03125   Median :  0.1831  
    ##  Mean   :-0.02018   Mean   : 0.01829   Mean   :  0.1411  
    ##  3rd Qu.: 0.66101   3rd Qu.: 0.97468   3rd Qu.:  9.8168  
    ##  Max.   : 3.77398   Max.   : 5.35372   Max.   : 59.9260

    colnames(DT)

    ## [1] "y1"    "const" "x1"    "x2"    "x3"    "eps1"  "eps2"

The variables we have are y1, constant, x1, x2, x3, eps1 and eps2

    library(matlib)

    X <- cbind(c(DT$const), c(DT$x1), c(DT$x2), c(DT$x3))

    Y <-cbind(c(DT$y1))

    XT <- t(X)

    XTX <- XT%*%X
    XTX

    ##            [,1]       [,2]       [,3]       [,4]
    ## [1,] 7584.00000  -28.84399  -52.45766 -153.04722
    ## [2,]  -28.84399  786.14496   10.09589 1794.58853
    ## [3,]  -52.45766   10.09589 7824.05912   79.67348
    ## [4,] -153.04722 1794.58853   79.67348 7616.46991

    XTX_1 <- inv(XTX)
    XTX_1

    ##             [,1]        [,2]        [,3]        [,4]
    ## [1,]  0.00013192 -0.00000260  0.00000085  0.00000325
    ## [2,] -0.00000260  0.00275263  0.00000304 -0.00064866
    ## [3,]  0.00000085  0.00000304  0.00012783 -0.00000204
    ## [4,]  0.00000325 -0.00064866 -0.00000204  0.00028422

    XTY <- XT%*%Y
    XTY

    ##           [,1]
    ## [1,] 16455.648
    ## [2,]  1174.928
    ## [3,] 10158.341
    ## [4,]  4778.788

    beta_hat <- XTX_1%*%XTY
    beta_hat

    ##           [,1]
    ## [1,] 2.1919399
    ## [2,] 0.1224290
    ## [3,] 1.3063511
    ## [4,] 0.6288565

This value is our OLS estimator

    library(stargazer)

    ## 
    ## Please cite as:

    ##  Hlavac, Marek (2018). stargazer: Well-Formatted Regression and Summary Statistics Tables.

    ##  R package version 5.2.2. https://CRAN.R-project.org/package=stargazer

    out1 <- lm(y1 ~ x1 + x2 +x3, data=DT)
    stargazer(out1, type = "text")

    ## 
    ## ===============================================
    ##                         Dependent variable:    
    ##                     ---------------------------
    ##                                 y1             
    ## -----------------------------------------------
    ## x1                             0.122           
    ##                               (0.167)          
    ##                                                
    ## x2                           1.306***          
    ##                               (0.036)          
    ##                                                
    ## x3                           0.629***          
    ##                               (0.054)          
    ##                                                
    ## Constant                     2.192***          
    ##                               (0.037)          
    ##                                                
    ## -----------------------------------------------
    ## Observations                   7,584           
    ## R2                             0.179           
    ## Adjusted R2                    0.179           
    ## Residual Std. Error      3.183 (df = 7580)     
    ## F Statistic          552.210*** (df = 3; 7580) 
    ## ===============================================
    ## Note:               *p<0.1; **p<0.05; ***p<0.01

The parameter values from the matrix estimate are the same as from the
regression estimate.

    XTX/nrow(DT)

    ##              [,1]         [,2]         [,3]        [,4]
    ## [1,]  1.000000000 -0.003803269 -0.006916885 -0.02018028
    ## [2,] -0.003803269  0.103658354  0.001331209  0.23662823
    ## [3,] -0.006916885  0.001331209  1.031653365  0.01050547
    ## [4,] -0.020180277  0.236628234  0.010505469  1.00428137

    cov(DT)

    ##                y1 const            x1           x2          x3          eps1
    ## y1    12.34255205     0  0.1631956969  1.354630521  0.67399025 -0.0345977698
    ## const  0.00000000     0  0.0000000000  0.000000000  0.00000000  0.0000000000
    ## x1     0.16319570     0  0.1036575573  0.001305074  0.23658268 -0.0009250523
    ## x2     1.35463052     0  0.0013050743  1.031741563  0.01036725  0.0044524846
    ## x3     0.67399025     0  0.2365826780  0.010367251  1.00400651 -0.0149200056
    ## eps1  -0.03459777     0 -0.0009250523  0.004452485 -0.01492001  2.0521287710
    ## eps2   0.35828556     0  0.0845584145 -0.105670109  0.22875506 -0.1687865058
    ##               eps2
    ## y1      0.35828556
    ## const   0.00000000
    ## x1      0.08455841
    ## x2     -0.10567011
    ## x3      0.22875506
    ## eps1   -0.16878651
    ## eps2  199.70843926

### IV Estimation

    df.IV <- read.table("datasets/IV_Data.RData")
    dt.IV <- data.table(df.IV) 

    colnames(dt.IV)

    ## [1] "y4"    "y5"    "const" "x1"    "x2"    "z1"    "z2"    "z3"    "z4"

    pop.endog2 <- lm(y5 ~ x1 + x2, data=dt.IV)
    stargazer(pop.endog2, type = "text")

    ## 
    ## ==================================================
    ##                          Dependent variable:      
    ##                     ------------------------------
    ##                                   y5              
    ## --------------------------------------------------
    ## x1                             2.930***           
    ##                                (0.007)            
    ##                                                   
    ## x2                             1.680***           
    ##                                (0.007)            
    ##                                                   
    ## Constant                       0.490***           
    ##                                (0.007)            
    ##                                                   
    ## --------------------------------------------------
    ## Observations                    17,584            
    ## R2                              0.932             
    ## Adjusted R2                     0.932             
    ## Residual Std. Error       0.965 (df = 17581)      
    ## F Statistic         120,701.300*** (df = 2; 17581)
    ## ==================================================
    ## Note:                  *p<0.1; **p<0.05; ***p<0.01

I expect the bias to be positive for both x1 and x2. The correltaion
between x1 and x2 is positive and in the full model the coefficient on
each is also positive so I expect the bias to be positive for both x1
and x2.

x1 is endogenous because it is correlated with the error term. Although
E(X,u) for x2 is also not zero it is only 0.02 so x1 should be tackled
first.

z1 is endogenous because it is correlated with the error term

z2 is irrelevant because it is not correlated with x1

z3 is the best instrument

z4 is a weak instrument because its covariance with x1 is only 0.01

Therefore z1 and z2 cannot be used as instruments at all and z4 is not a
good instrument. Violation of IV assumption 1 - endogeneity means the
moment equations do not hold. Violation of IV assumption 2 - relevance
means the rank condition does not hold. Together, assumption 1 and 2
constitute the exclusion restriction. A good instrument must satisfy
both assumptions.

With regards to z4, we can test the relevance assumption using Pearson’s
product moment correlation test.

    cor.test(dt.IV$z4, dt.IV$x1)

    ## 
    ##  Pearson's product-moment correlation
    ## 
    ## data:  dt.IV$z4 and dt.IV$x1
    ## t = 2.2537, df = 17582, p-value = 0.02423
    ## alternative hypothesis: true correlation is not equal to 0
    ## 95 percent confidence interval:
    ##  0.002214089 0.031766931
    ## sample estimates:
    ##        cor 
    ## 0.01699422

This indicates z4 is a weak instrument. Weak instruments result in
biased estimators. In general a model is only as good as its weakest
instrument.

**2SLS**

    out1st <- lm(x1 ~ x2 + z3, data=dt.IV)
    summary(out1st)

    ## 
    ## Call:
    ## lm(formula = x1 ~ x2 + z3, data = dt.IV)
    ## 
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -3.8181 -0.6625  0.0070  0.6684  3.6077 
    ## 
    ## Coefficients:
    ##              Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept) -0.002372   0.007369  -0.322    0.747    
    ## x2           0.128994   0.007294  17.685   <2e-16 ***
    ## z3           0.186035   0.007397  25.150   <2e-16 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 0.9771 on 17581 degrees of freedom
    ## Multiple R-squared:  0.05069,    Adjusted R-squared:  0.05059 
    ## F-statistic: 469.4 on 2 and 17581 DF,  p-value: < 2.2e-16

    dt.IV <- dt.IV[, x1hat:=predict(out1st, newdata=dt.IV)]
    dt.IV

    out3rd <- lm(y5 ~ x1hat + x2, data=dt.IV)
    stargazer(out3rd, pop.endog2, type="text")

    ## 
    ## =============================================================
    ##                                      Dependent variable:     
    ##                                  ----------------------------
    ##                                               y5             
    ##                                       (1)           (2)      
    ## -------------------------------------------------------------
    ## x1hat                              2.115***                  
    ##                                     (0.124)                  
    ##                                                              
    ## x1                                                2.930***   
    ##                                                   (0.007)    
    ##                                                              
    ## x2                                 1.784***       1.680***   
    ##                                     (0.028)       (0.007)    
    ##                                                              
    ## Constant                           0.488***       0.490***   
    ##                                     (0.023)       (0.007)    
    ##                                                              
    ## -------------------------------------------------------------
    ## Observations                        17,584         17,584    
    ## R2                                   0.325         0.932     
    ## Adjusted R2                          0.325         0.932     
    ## Residual Std. Error (df = 17581)     3.044         0.965     
    ## F Statistic (df = 2; 17581)      4,231.712***  120,701.300***
    ## =============================================================
    ## Note:                             *p<0.1; **p<0.05; ***p<0.01

**MM Estimator**

Taking z_i as the vector of exogenous variables we can write the moment conditions as 

![equation](https://latex.codecogs.com/gif.latex?%5Cbegin%7Balign*%7D%20E%28%5Ctextbf%7Bz%7D_i%28y_i-%5Ctextbf%7Bx%7D%27_i%5Cbeta%29%29%3D0%20%26%26%20%5C%3E%20%5Ctextbf%7Bz%7D_i%20%3D%20%5Cbegin%7Bbmatrix%7D%20%5Ctextbf%7Bx%7D_%7Bi1%7D%20%5C%5C%20%5Ctextbf%7Bz%7D_%7Bi1%7D%20%5Cend%7Bbmatrix%7D%20%5Cend%7Balign*%7D)

If E(z'x) has full rank then above equation has unique solution

![equation](https://latex.codecogs.com/gif.latex?%5Cbeta%20%3D%20%28E%28%5Ctextbf%7Bz%7D_i%5Ctextbf%7Bx%7D%27_i%29%29%5E%7B-1%7DE%28%5Ctextbf%7Bz%7D_iy_i%29)

Method of moments estimator replaces these population moments with sample moments

![equation](https://latex.codecogs.com/gif.latex?%5Cfrac%7B1%7D%7Bn%7D%5Csum%5En_%7Bi%3D1%7D%5Ctextbf%7Bz%7D_i%28y_i-%5Ctextbf%7Bx%7D%27_i%5Cbeta%29%3D0)

from which 

![equation](https://latex.codecogs.com/gif.latex?%5Cbegin%7Balign*%7D%20%7B%7D%5Chat%7B%5Cbeta%7D_%7BMM%7D%26%3D%28%5Cfrac%7B1%7D%7Bn%7D%5Csum%5En_%7Bi%3D1%7D%5Ctextbf%7Bz%7D_i%5Ctextbf%7Bx%7D%27_i%29%5E%7B-1%7D%28%5Cfrac%7B1%7D%7Bn%7D%5Csum%5En_%7Bi%3D1%7D%5Ctextbf%7Bz%7D_iy_i%29%5C%5C%20%26%3D%28%5Ctextbf%7BZ%7D%27%5Ctextbf%7BX%7D%29%5E%7B-1%7D%5Ctextbf%7BZ%7D%27%5Ctextbf%7By%7D%20%5Cend%7Balign*%7D)



Include x2 and z3 in the z matrix

    Z <- cbind(c(dt.IV$x2), c(dt.IV$z3))
    X <- cbind(c(dt.IV$x1), c(dt.IV$x2))
    Y5 <- cbind(c(dt.IV$y5))

    inv(t(Z)%*%X)%*%(t(Z)%*%Y5)

    ##          [,1]
    ## [1,] 2.115374
    ## [2,] 1.780623

**Using ivreg library in R**

    library(ivreg)

    ivy5z1 <- ivreg(y5~x1+x2|x2 + z1, data=dt.IV)

    summary(ivy5z1)

    ## 
    ## Call:
    ## ivreg(formula = y5 ~ x1 + x2 | x2 + z1, data = dt.IV)
    ## 
    ## Residuals:
    ##       Min        1Q    Median        3Q       Max 
    ## -5.139435 -0.868555 -0.008043  0.849666  4.614232 
    ## 
    ## Coefficients:
    ##             Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept) 0.491905   0.009585   51.32   <2e-16 ***
    ## x1          3.760850   0.019225  195.62   <2e-16 ***
    ## x2          1.574199   0.009800  160.64   <2e-16 ***
    ## 
    ## Diagnostic tests:
    ##                    df1   df2 statistic p-value    
    ## Weak instruments     1 17581      5902  <2e-16 ***
    ## Wu-Hausman           1 17580      5740  <2e-16 ***
    ## Sargan               0    NA        NA      NA    
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 1.271 on 17581 degrees of freedom
    ## Multiple R-Squared: 0.8823,  Adjusted R-squared: 0.8823 
    ## Wald test: 4.258e+04 on 2 and 17581 DF,  p-value: < 2.2e-16

    stargazer(out3rd, ivy5z1, type="text")

    ## 
    ## ==========================================================================
    ##                                             Dependent variable:           
    ##                                  -----------------------------------------
    ##                                                     y5                    
    ##                                              OLS              instrumental
    ##                                                                 variable  
    ##                                              (1)                  (2)     
    ## --------------------------------------------------------------------------
    ## x1hat                                      2.115***                       
    ##                                            (0.124)                        
    ##                                                                           
    ## x1                                                              3.761***  
    ##                                                                 (0.019)   
    ##                                                                           
    ## x2                                         1.784***             1.574***  
    ##                                            (0.028)              (0.010)   
    ##                                                                           
    ## Constant                                   0.488***             0.492***  
    ##                                            (0.023)              (0.010)   
    ##                                                                           
    ## --------------------------------------------------------------------------
    ## Observations                                17,584               17,584   
    ## R2                                          0.325                0.882    
    ## Adjusted R2                                 0.325                0.882    
    ## Residual Std. Error (df = 17581)            3.044                1.271    
    ## F Statistic                      4,231.712*** (df = 2; 17581)             
    ## ==========================================================================
    ## Note:                                          *p<0.1; **p<0.05; ***p<0.01

    ivy5z2 <- ivreg(y5~x1+x2|x2 + z2, data=dt.IV)

    ivy5z3 <- ivreg(y5~x1+x2|x2 + z3, data=dt.IV)

    ivy5z4 <- ivreg(y5~x1+x2|x2 + z4, data=dt.IV)

    stargazer (out3rd, ivy5z2, type="text")

    ## 
    ## ==========================================================================
    ##                                             Dependent variable:           
    ##                                  -----------------------------------------
    ##                                                     y5                    
    ##                                              OLS              instrumental
    ##                                                                 variable  
    ##                                              (1)                  (2)     
    ## --------------------------------------------------------------------------
    ## x1hat                                      2.115***                       
    ##                                            (0.124)                        
    ##                                                                           
    ## x1                                                               4.983    
    ##                                                                 (3.997)   
    ##                                                                           
    ## x2                                         1.784***             1.418***  
    ##                                            (0.028)              (0.510)   
    ##                                                                           
    ## Constant                                   0.488***             0.495***  
    ##                                            (0.023)              (0.019)   
    ##                                                                           
    ## --------------------------------------------------------------------------
    ## Observations                                17,584               17,584   
    ## R2                                          0.325                0.628    
    ## Adjusted R2                                 0.325                0.628    
    ## Residual Std. Error (df = 17581)            3.044                2.259    
    ## F Statistic                      4,231.712*** (df = 2; 17581)             
    ## ==========================================================================
    ## Note:                                          *p<0.1; **p<0.05; ***p<0.01

    stargazer (out3rd, ivy5z3, type="text")

    ## 
    ## ==========================================================================
    ##                                             Dependent variable:           
    ##                                  -----------------------------------------
    ##                                                     y5                    
    ##                                              OLS              instrumental
    ##                                                                 variable  
    ##                                              (1)                  (2)     
    ## --------------------------------------------------------------------------
    ## x1hat                                      2.115***                       
    ##                                            (0.124)                        
    ##                                                                           
    ## x1                                                              2.115***  
    ##                                                                 (0.051)   
    ##                                                                           
    ## x2                                         1.784***             1.784***  
    ##                                            (0.028)              (0.011)   
    ##                                                                           
    ## Constant                                   0.488***             0.488***  
    ##                                            (0.023)              (0.010)   
    ##                                                                           
    ## --------------------------------------------------------------------------
    ## Observations                                17,584               17,584   
    ## R2                                          0.325                0.884    
    ## Adjusted R2                                 0.325                0.884    
    ## Residual Std. Error (df = 17581)            3.044                1.260    
    ## F Statistic                      4,231.712*** (df = 2; 17581)             
    ## ==========================================================================
    ## Note:                                          *p<0.1; **p<0.05; ***p<0.01

    stargazer (out3rd, ivy5z4, type="text")

    ## 
    ## ==========================================================================
    ##                                             Dependent variable:           
    ##                                  -----------------------------------------
    ##                                                     y5                    
    ##                                              OLS              instrumental
    ##                                                                 variable  
    ##                                              (1)                  (2)     
    ## --------------------------------------------------------------------------
    ## x1hat                                      2.115***                       
    ##                                            (0.124)                        
    ##                                                                           
    ## x1                                                              2.263***  
    ##                                                                 (0.459)   
    ##                                                                           
    ## x2                                         1.784***             1.765***  
    ##                                            (0.028)              (0.059)   
    ##                                                                           
    ## Constant                                   0.488***             0.488***  
    ##                                            (0.023)              (0.009)   
    ##                                                                           
    ## --------------------------------------------------------------------------
    ## Observations                                17,584               17,584   
    ## R2                                          0.325                0.900    
    ## Adjusted R2                                 0.325                0.900    
    ## Residual Std. Error (df = 17581)            3.044                1.171    
    ## F Statistic                      4,231.712*** (df = 2; 17581)             
    ## ==========================================================================
    ## Note:                                          *p<0.1; **p<0.05; ***p<0.01

    Den <- cov(dt.IV$z3, dt.IV$x1)
    Num <- cov(dt.IV$z3, dt.IV$y4)

    iv_foot = Num/Den 
    iv_foot

    ## [1] 2.149448

My best guess for the coefficients used ar 2.1 on x1 and 1.7 on x2.
