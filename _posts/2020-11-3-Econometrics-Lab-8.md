---
layout: post
title:  "Econometrics Lab 8"
date:   2020-11-03 11:00:00
categories: jekyll update
comment_issue_id: 41
---

#Task 1

1)	The four main sources of endogeneity are

  +	 Omitted Variable Bias
  + Simultaneity
  + Measurement Error
  + Treatment effects with selection into treatment

2) Selection bias occurs because of unobserved heterogeneity between those who are treated and those who are untreated, other than treatment status, which leads to confoundedness in the effect of the treatment. To observe average treatment effect on the treated, we want to compare the expected value of the outcome variable of treated individuals, ![equation](https://latex.codecogs.com/gif.latex?E%28Y_%7B1i%7D%7C%20D_i%3D1%29) with the outcome variable of the same individuals had they not been treated ![equation](https://latex.codecogs.com/gif.latex?E%28Y_%7B0i%7D%7C%20D_i%3D1%29). However, this counterfactual is unobservable. One way to approximate the counterfactual is to use the expected value of the observed variable for those who were untreated, ![equation](https://latex.codecogs.com/gif.latex?E%28Y_%7B0i%7D%7C%20D_i%3D0%29). Thus, from our two observed values, the average treatment effect on the treated is 

![equation](https://latex.codecogs.com/gif.latex?%5Cbegin%7Balign*%7D%20%5Chat%5Cbeta_%7BATET%7D%26%3D%5Cunderbrace%7BE%5Cleft%28Y_%7B1i%7D%5Cmiddle%7C%20D_i%3D1%5Cright%29-E%5Cleft%28Y_%7B0i%7D%5Cmiddle%7C%20D_i%3D0%5Cright%29%7D_%5Ctext%7Bobserved%20data%7D%5C%5C%20%26%3D%5Cunderbrace%7BE%5Cleft%28Y_%7B1i%7D%5Cmiddle%7C%20D_i%3D1%5Cright%29-E%5Cleft%28Y_%7B0i%7D%5Cmiddle%7C%20D_i%3D1%5Cright%29%7D_%5Ctext%7Baverage%20treatment%20effect%20on%20treated%7D%20&plus;%20%5Cunderbrace%7BE%5Cleft%28Y_%7B0i%7D%5Cmiddle%7C%20D_i%3D1%5Cright%29-%20E%5Cleft%28Y_%7B0i%7D%5Cmiddle%7C%20D_i%3D0%5Cright%29%7D_%5Ctext%7Bselection%20bias%7D%20%5Cend%7Balign*%7D)

3) ![equation](https://latex.codecogs.com/gif.latex?%5Cbegin%7Balign*%7D%20%5Ctext%7BE%7D%28%5Chat%5Cbeta_1%29%20%26%3D%20%28%5Ctextbf%7BX%7D_1%27%5Ctextbf%7BX%7D_1%29%5E%7B-1%7D%5Ctextbf%7BX%7D_1%27%5Ctextbf%7BY%7D%20%5C%5C%20%26%3D%20%5Ctext%7BE%7D%5B%28%5Ctextbf%7BX%7D_1%27%5Ctextbf%7BX%7D_1%29%5E%7B-1%7D%5Ctextbf%7BX%7D_1%27%28%5Ctextbf%7BX%7D_1%5Cbeta_1%20&plus;%5Ctextbf%7BX%7D_2%20%5Cbeta_2%20&plus;%20u%29%5D%20%5C%5C%20%26%3D%20%5Ctext%7BE%7D%5B%28%5Ctextbf%7BX%7D_1%27%5Ctextbf%7BX%7D_1%29%5E%7B-1%7D%5Ctextbf%7BX%7D_1%27%5Ctextbf%7BX%7D_1%5Cbeta_1%20&plus;%20%28%5Ctextbf%7BX%7D_1%27%5Ctextbf%7BX%7D_1%29%5E%7B-1%7D%5Ctextbf%7BX%7D_1%27%5Ctextbf%7BX%7D_2%5Cbeta_2%20&plus;%20%28%5Ctextbf%7BX%7D_1%27%5Ctextbf%7BX%7D_1%29%5E%7B-1%7D%5Ctextbf%7BX%7D_1%27u%5D%20%5C%5C%20%26%3D%20%5Cbeta_1%20&plus;%20%28%5Ctextbf%7BX%7D_1%27%5Ctextbf%7BX%7D_1%29%5E%7B-1%7D%5Ctextbf%7BX%7D_1%27%5Ctextbf%7BX%7D_2%5Cbeta_2%20%5Cend%7Balign*%7D)

![equation](https://latex.codecogs.com/gif.latex?%5Ctext%7BBias%7D%28%5Chat%5Cbeta_1%29%20%3D%20%28%5Ctextbf%7BX%7D_1%27%5Ctextbf%7BX%7D_1%29%5E%7B-1%7D%5Ctextbf%7BX%7D_1%27%5Ctextbf%7BX%7D_2%5Cbeta_2)

Setup
-----

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

When ![equation](https://latex.codecogs.com/gif.latex?%5Cdelta%20%3D0),
![equation](https://latex.codecogs.com/gif.latex?%5Ctilde%5Cbeta_1%3D%5Chat%5Cbeta_1)

Simulate x variables

    set.seed(1)
    ssize <- 1000
    x1 <- rnorm( n = ssize , sd = 3 )
    x2 <- rnorm( n = ssize , sd = 5 )
    y <- 2 + 3*x1 + 5 * x2 + rnorm(n = ssize, sd = 5)
    out.y.full <- lm( y ~ x1 + x2)
    out.y.x1.om <- lm( y ~ x1)
    out.y.x2.om <- lm( y ~ x2 )
    cor.test(x = x1, y = x2)

    ## 
    ##  Pearson's product-moment correlation
    ## 
    ## data:  x1 and x2
    ## t = 0.20223, df = 998, p-value = 0.8398
    ## alternative hypothesis: true correlation is not equal to 0
    ## 95 percent confidence interval:
    ##  -0.05561394  0.06836716
    ## sample estimates:
    ##         cor 
    ## 0.006401211

Output

    stargazer(out.y.full, out.y.x1.om, out.y.x2.om,
              type = 'text', omit.stat = c('f','ser'), no.space=T)

    ## 
    ## ==========================================
    ##                   Dependent variable:     
    ##              -----------------------------
    ##                            y              
    ##                 (1)       (2)       (3)   
    ## ------------------------------------------
    ## x1           3.082***  3.136***           
    ##               (0.053)   (0.271)           
    ## x2           5.022***            5.034*** 
    ##               (0.031)             (0.066) 
    ## Constant     2.081***   1.675**  1.974*** 
    ##               (0.163)   (0.842)   (0.344) 
    ## ------------------------------------------
    ## Observations   1,000     1,000     1,000  
    ## R2             0.967     0.118     0.853  
    ## Adjusted R2    0.967     0.117     0.853  
    ## ==========================================
    ## Note:          *p<0.1; **p<0.05; ***p<0.01

When
![equation](https://latex.codecogs.com/gif.latex?%5Cdelta%20%5Cneq0),
![equation](https://latex.codecogs.com/gif.latex?%5Ctilde%5Cbeta_1%5Cneq%5Chat%5Cbeta_1)
because
![equation](https://latex.codecogs.com/gif.latex?Cor%28x_1%2Cx_2%29%5Cneq0)

Simulate x variables

    set.seed(2)
    ssize <- 1000
    x1 <- rnorm( n = ssize , sd = 3 )
    x2 <- rnorm( n = ssize , mean = x1, sd = 5 )
    y <- 2 + 3*x1 + 5 * x2 + rnorm(n = ssize, sd = 5)
    out.y.full <- lm( y ~ x1 + x2)
    out.y.x1.om <- lm( y ~ x1)
    out.y.x2.om <- lm( y ~ x2 )
    cor.test(x = x1, y = x2)

    ## 
    ##  Pearson's product-moment correlation
    ## 
    ## data:  x1 and x2
    ## t = 20.425, df = 998, p-value < 2.2e-16
    ## alternative hypothesis: true correlation is not equal to 0
    ## 95 percent confidence interval:
    ##  0.4976946 0.5852314
    ## sample estimates:
    ##       cor 
    ## 0.5429362

Output

    stargazer(out.y.full, out.y.x1.om, out.y.x2.om,
              type = 'text', omit.stat = c('f','ser'), no.space =T)

    ## 
    ## ==========================================
    ##                   Dependent variable:     
    ##              -----------------------------
    ##                            y              
    ##                 (1)       (2)       (3)   
    ## ------------------------------------------
    ## x1           3.001***  8.272***           
    ##               (0.063)   (0.263)           
    ## x2           4.997***            5.836*** 
    ##               (0.032)             (0.049) 
    ## Constant     2.332***  2.806***  2.646*** 
    ##               (0.162)   (0.803)   (0.292) 
    ## ------------------------------------------
    ## Observations   1,000     1,000     1,000  
    ## R2             0.980     0.497     0.934  
    ## Adjusted R2    0.980     0.496     0.933  
    ## ==========================================
    ## Note:          *p<0.1; **p<0.05; ***p<0.01

![equation](https://latex.codecogs.com/gif.latex?x_2) centered around
![equation](https://latex.codecogs.com/gif.latex?1.33x_1)

    set.seed(2)
    ssize <- 1000
    x1 <- rnorm( n = ssize , sd = 3 )
    x2 <- rnorm( n = ssize , mean = 1.33*x1, sd = 5 )
    y <- 2 + 3*x1 + 5 * x2 + rnorm(n = ssize, sd = 5)
    out.y.full <- lm( y ~ x1 + x2)
    out.y.x1.om <- lm( y ~ x1)
    out.y.x2.om <- lm( y ~ x2 )
    cor.test(x = x1, y = x2)

    ## 
    ##  Pearson's product-moment correlation
    ## 
    ## data:  x1 and x2
    ## t = 26.815, df = 998, p-value < 2.2e-16
    ## alternative hypothesis: true correlation is not equal to 0
    ## 95 percent confidence interval:
    ##  0.6095812 0.6817626
    ## sample estimates:
    ##       cor 
    ## 0.6471197

Output

    stargazer(out.y.full, out.y.x1.om, out.y.x2.om,
              type = 'text', omit.stat = c('f','ser'), no.space =T)

    ## 
    ## ==========================================
    ##                   Dependent variable:     
    ##              -----------------------------
    ##                            y              
    ##                 (1)       (2)       (3)   
    ## ------------------------------------------
    ## x1           3.003***  9.922***           
    ##               (0.069)   (0.263)           
    ## x2           4.997***            5.905*** 
    ##               (0.032)             (0.042) 
    ## Constant     2.332***  2.806***  2.570*** 
    ##               (0.162)   (0.803)   (0.274) 
    ## ------------------------------------------
    ## Observations   1,000     1,000     1,000  
    ## R2             0.983     0.587     0.952  
    ## Adjusted R2    0.983     0.587     0.952  
    ## ==========================================
    ## Note:          *p<0.1; **p<0.05; ***p<0.01

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

    ##       x1 
    ## 6.919252

Bias when we omit x1

    out.part.x1 <- lm ( x1 ~ x2)
    coeffs.part <- coefficients(out.part.x1)
    delta <- coeffs.part[2]

    bias <- delta*b1_hat
    bias

    ##        x2 
    ## 0.9080115

### Stork Data

    stork <- read.csv(file = "datasets/stork_population.dsv" ,sep =';')
    names(stork)

    ## [1] "country"                     "area_km2"                   
    ## [3] "stork_pairs"                 "human_population_millions"  
    ## [5] "yearly_birth_rate_thousands"

    stork[1:8,c('country','stork_pairs','yearly_birth_rate_thousands')]

    ##    country stork_pairs yearly_birth_rate_thousands
    ## 1  Albania         100                          83
    ## 2  Austria         300                          87
    ## 3  Belgium           1                         118
    ## 4 Bulgaria        5000                         117
    ## 5  Denmark           9                          59
    ## 6   France         140                         774
    ## 7  Germany        3300                         901
    ## 8   Greece        2500                         106

    library(ggplot2)
    qplot( data = stork,x = stork_pairs
           , y = yearly_birth_rate_thousands
           , geom = 'point') +
      stat_smooth(method='lm') + theme_bw()

    ## `geom_smooth()` using formula 'y ~ x'

![storkplot-1](https://user-images.githubusercontent.com/73550706/98030001-402a0780-1e08-11eb-8064-c8037160714a.png)


    out.lm <- lm( yearly_birth_rate_thousands ~ stork_pairs, data
                  = stork)
    stargazer(out.lm, type = 'text')

    ## 
    ## ===============================================
    ##                         Dependent variable:    
    ##                     ---------------------------
    ##                     yearly_birth_rate_thousands
    ## -----------------------------------------------
    ## stork_pairs                  0.029***          
    ##                               (0.009)          
    ##                                                
    ## Constant                     225.029**         
    ##                              (93.561)          
    ##                                                
    ## -----------------------------------------------
    ## Observations                    17             
    ## R2                             0.385           
    ## Adjusted R2                    0.344           
    ## Residual Std. Error      332.185 (df = 15)     
    ## F Statistic            9.380*** (df = 1; 15)   
    ## ===============================================
    ## Note:               *p<0.1; **p<0.05; ***p<0.01

    qplot( data = stork,x = stork_pairs
           , y = yearly_birth_rate_thousands
           , geom = 'point', color = area_km2) +
      theme_bw()

![storkplot-2](https://user-images.githubusercontent.com/73550706/98030053-559f3180-1e08-11eb-8893-345df9bbaf72.png)

    out.lm <- lm( yearly_birth_rate_thousands ~ stork_pairs +
                    area_km2, data = stork)
    stargazer(out.lm, type = 'text')

    ## 
    ## ===============================================
    ##                         Dependent variable:    
    ##                     ---------------------------
    ##                     yearly_birth_rate_thousands
    ## -----------------------------------------------
    ## stork_pairs                    0.006           
    ##                               (0.006)          
    ##                                                
    ## area_km2                     0.002***          
    ##                              (0.0002)          
    ##                                                
    ## Constant                      -7.412           
    ##                              (56.702)          
    ##                                                
    ## -----------------------------------------------
    ## Observations                    17             
    ## R2                             0.862           
    ## Adjusted R2                    0.842           
    ## Residual Std. Error      162.744 (df = 14)     
    ## F Statistic           43.787*** (df = 2; 14)   
    ## ===============================================
    ## Note:               *p<0.1; **p<0.05; ***p<0.01

    x <- stork[,"stork_pairs"]
    y <- stork[,"area_km2"]
    cor.test(x,y)

    ## 
    ##  Pearson's product-moment correlation
    ## 
    ## data:  x and y
    ## t = 2.7528, df = 15, p-value = 0.0148
    ## alternative hypothesis: true correlation is not equal to 0
    ## 95 percent confidence interval:
    ##  0.1367869 0.8291138
    ## sample estimates:
    ##       cor 
    ## 0.5793423

This is omitted variable bias because our first model excludes area
which should be in the model.

You want to
estimate:![equation](https://latex.codecogs.com/gif.latex?movie%5C_sales%3D%5Cbeta_0%3D%5Cbeta_1movie%5C_price+u)
What do you think will be the relation between sales and price ? A:
Inverse relationship between price and sales - ie a downward sloping
demand curve

    dataset <- read.csv(file="datasets/vod_sales.csv")
    movies <- data.table(dataset)
    rm(dataset)
    qplot(data=movies,y=price,x=leases,geom='point') + labs(x = 'Leases', y = 'Price') + theme_bw()

![moviesplot-1](https://user-images.githubusercontent.com/73550706/98029705-da3d8000-1e07-11eb-9dc2-fe40a9097a02.png)


    summary(out <- lm( leases ~ price,data=movies))

    ## 
    ## Call:
    ## lm(formula = leases ~ price, data = movies)
    ## 
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -52.293 -24.119  -5.695  10.893 204.707 
    ## 
    ## Coefficients:
    ##              Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept) -20.46678    8.96496  -2.283   0.0237 *  
    ## price         0.18549    0.03012   6.158 5.21e-09 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 34.93 on 169 degrees of freedom
    ## Multiple R-squared:  0.1832, Adjusted R-squared:  0.1784 
    ## F-statistic: 37.92 on 1 and 169 DF,  p-value: 5.214e-09

This appears to suggest that as price goes up, sales (leases) go up.

    movies$age <- 2013 - movies$year
    summary(out <- lm( leases ~ price + age,data=movies))

    ## 
    ## Call:
    ## lm(formula = leases ~ price + age, data = movies)
    ## 
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -53.425 -20.664  -6.367  13.102 201.575 
    ## 
    ## Coefficients:
    ##             Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)  3.72748   12.47086   0.299 0.765389    
    ## price        0.13712    0.03444   3.982 0.000102 ***
    ## age         -1.76483    0.64474  -2.737 0.006862 ** 
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 34.28 on 168 degrees of freedom
    ## Multiple R-squared:  0.2181, Adjusted R-squared:  0.2088 
    ## F-statistic: 23.43 on 2 and 168 DF,  p-value: 1.057e-09

    qplot(data=movies,y=price,x=age,geom='point') +
      labs(x = 'age', y = 'Price') + theme_bw()

![moviesplot2-1](https://user-images.githubusercontent.com/73550706/98029776-f7724e80-1e07-11eb-9d0a-90984ab8d946.png)


    qplot(data=movies,y=price,x=age,geom='point', position = position_jitter(w = 0.0, h = 20)
          , size = leases, alpha = 0.5) +
      labs(x = 'age', y = 'Price') + theme_bw()

    ## Warning: `position` is deprecated

![moviesplot2-2](https://user-images.githubusercontent.com/73550706/98029817-0953f180-1e08-11eb-804a-a8b24aca962f.png)


    qplot(data=movies,y=price,x=imdb,geom='point', position = position_jitter(w = 0.0, h = 20)
          , size = leases, alpha = 0.5) +
      labs(x = 'Imdb Rating', y = 'Price') + theme_bw()

    ## Warning: `position` is deprecated

![moviesplot2-3](https://user-images.githubusercontent.com/73550706/98029950-2f799180-1e08-11eb-8a95-1da827bd2c21.png)

    summary(out <- lm( leases ~ price + I(age == 2) + I( age >= 3 & age <= 9) + I(age >=10) ,data=movies))

    ## 
    ## Call:
    ## lm(formula = leases ~ price + I(age == 2) + I(age >= 3 & age <= 
    ##     9) + I(age >= 10), data = movies)
    ## 
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -73.593 -15.199  -5.904  12.194 181.407 
    ## 
    ## Coefficients:
    ##                             Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)                 68.90888   16.88737   4.080 6.97e-05 ***
    ## price                        0.01988    0.03797   0.524    0.601    
    ## I(age == 2)TRUE            -11.52135    9.81053  -1.174    0.242    
    ## I(age >= 3 & age <= 9)TRUE -52.29505    9.75924  -5.359 2.77e-07 ***
    ## I(age >= 10)TRUE           -63.44998   11.99849  -5.288 3.85e-07 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 31.66 on 166 degrees of freedom
    ## Multiple R-squared:  0.3409, Adjusted R-squared:  0.325 
    ## F-statistic: 21.46 on 4 and 166 DF,  p-value: 2.758e-14

OBV or selection bias are probably the two types of endogeniety I am
likely to encounter as it is likely there will be variables in the
underlying model that are either unobservable or unmeasurable.
Additionally when analysing treatment effects the approximation of the
counterfactual will often cause endogeneity.
