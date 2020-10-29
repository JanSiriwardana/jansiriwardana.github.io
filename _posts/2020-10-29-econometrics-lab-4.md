---
layout: post
title:  "Econometrics Lab 4"
date:   2020-10-29 
categories: jekyll update
---

Homework 4
================
Janithe Siriwardana

Setup
-----

Set working directory and activate necessary packages:

    setwd("~/Desktop/R WD")
    library(ggplot2)
    library(stargazer)

    ## 
    ## Please cite as:

    ##  Hlavac, Marek (2018). stargazer: Well-Formatted Regression and Summary Statistics Tables.

    ##  R package version 5.2.2. https://CRAN.R-project.org/package=stargazer

    library(data.table)

### Load data

    load("datasets/affairs.RData")
    dt.affairs <- data.table(data)
    rm(data)

### Summary statistics

Summary statistics table

    stargazer(dt.affairs, type = "text")

    ## 
    ## ============================================================
    ## Statistic  N    Mean    St. Dev. Min Pctl(25) Pctl(75)  Max 
    ## ------------------------------------------------------------
    ## id        601 1,059.722 914.905   4    528     1,453   9,029
    ## male      601   0.476    0.500    0     0        1       1  
    ## age       601  32.488    9.289   18     27       37     57  
    ## yrsmarr   601   8.178    5.571    0     4        15     15  
    ## kids      601   0.715    0.452    0     0        1       1  
    ## relig     601   3.116    1.168    1     2        4       5  
    ## educ      601  16.166    2.403    9     14       18     20  
    ## occup     601   4.195    1.819    1     3        6       7  
    ## ratemarr  601   3.932    1.103    1     3        5       5  
    ## naffairs  601   1.456    3.299    0     0        0      12  
    ## affair    601   0.250    0.433    0     0        0       1  
    ## vryhap    601   0.386    0.487    0     0        1       1  
    ## hapavg    601   0.323    0.468    0     0        1       1  
    ## avgmarr   601   0.155    0.362    0     0        0       1  
    ## unhap     601   0.110    0.313    0     0        0       1  
    ## vryrel    601   0.116    0.321    0     0        0       1  
    ## smerel    601   0.316    0.465    0     0        1       1  
    ## slghtrel  601   0.215    0.411    0     0        0       1  
    ## notrel    601   0.273    0.446    0     0        1       1  
    ## ------------------------------------------------------------

### Hypothesis

There are two variables here:

1.  An indicator variable *affair*: you should use this variable to
    test/compare the probabiliy/likelihood of having an extra-marital
    affair.

2.  Number of affairs *naffair*: you should use this variable to
    test/compare the average number of extramarital affairs.

Your hypotheses statements should be consistent with the variable you
choose to test - they should either mention the probability/likelihood
of having an affair or the average number of affairs, respectively.

**Two-sided hypothesis test**

Start by stating your hypotheses regarding the likelihood/number of
extra-marital affairs.

![equation](https://latex.codecogs.com/gif.latex?H_0%3A%20%5Cmu_%7B%5Ctext%28non-religious%29%7D%20-%20%5Cmu_%7B%5Ctext%28religious%29%7D%20%3D%200)

![equation](https://latex.codecogs.com/gif.latex?H_1%3A%20%5Cmu_%7B%5Ctext%28non-religious%29%7D%20-%20%5Cmu_%7B%5Ctext%28religious%29%7D%20%5Cneq%200)

Then set your criteria for what being “religious” is and create an
indicator variable:

    dt.affairs[, religious:= relig>3]

Check how many people are in each group:

    dt.affairs[, .N, by = religious]

    ##    religious   N
    ## 1:     FALSE 341
    ## 2:      TRUE 260

Run t-test

    dt.affairs[, t.test(affair ~ religious)]

    ## 
    ##  Welch Two Sample t-test
    ## 
    ## data:  affair by religious
    ## t = 3.7191, df = 594.76, p-value = 0.0002189
    ## alternative hypothesis: true difference in means is not equal to 0
    ## 95 percent confidence interval:
    ##  0.06043572 0.19568880
    ## sample estimates:
    ## mean in group FALSE  mean in group TRUE 
    ##           0.3049853           0.1769231

The p-value is less than 0.05 so we reject the null hypothesis that
there is no difference in mean probability of having an affair between
the two groups.

    dt.affairs[, t.test(naffairs ~ religious)]

    ## 
    ##  Welch Two Sample t-test
    ## 
    ## data:  naffairs by religious
    ## t = 4.0676, df = 593.3, p-value = 5.393e-05
    ## alternative hypothesis: true difference in means is not equal to 0
    ## 95 percent confidence interval:
    ##  0.5382493 1.5432981
    ## sample estimates:
    ## mean in group FALSE  mean in group TRUE 
    ##           1.9061584           0.8653846

The p-value is less than 0.05 so again we reject the null hypothesis
that there is no difference in the average number of affairs had by each
group.

Our test results will only change if we set different thresholds for
what religious is/is not.

**One-sided hypothesis test**

Start by stating your hypotheses regarding the likelihood/number of
extra-marital affairs.

![equation](https://latex.codecogs.com/gif.latex?H_0%3A%20%5Cmu_%7B%5Ctext%28non-religious%29%7D%20-%20%5Cmu_%7B%5Ctext%28religious%29%7D%20%5Cleq%200)

![equation](https://latex.codecogs.com/gif.latex?H_1%3A%20%5Cmu_%7B%5Ctext%28non-religious%29%7D%20-%20%5Cmu_%7B%5Ctext%28religious%29%7D%20%3E%200)

    dt.affairs[, t.test(affair ~ religious, alternative = c("greater"))]

    ## 
    ##  Welch Two Sample t-test
    ## 
    ## data:  affair by religious
    ## t = 3.7191, df = 594.76, p-value = 0.0001094
    ## alternative hypothesis: true difference in means is greater than 0
    ## 95 percent confidence interval:
    ##  0.07133542        Inf
    ## sample estimates:
    ## mean in group FALSE  mean in group TRUE 
    ##           0.3049853           0.1769231

    dt.affairs[, t.test(naffairs ~ religious, alternative = c("greater"))]

    ## 
    ##  Welch Two Sample t-test
    ## 
    ## data:  naffairs by religious
    ## t = 4.0676, df = 593.3, p-value = 2.696e-05
    ## alternative hypothesis: true difference in means is greater than 0
    ## 95 percent confidence interval:
    ##  0.6192441       Inf
    ## sample estimates:
    ## mean in group FALSE  mean in group TRUE 
    ##           1.9061584           0.8653846

Note that means are the same but the p-value is half of that of the
two-tailed test. Because we are just considering one area under the
curve (one tail of the curve.)

### Possible biases in self-reported data

You should consider the following points

-   Can answers be biased? Can people lie? Why?

-   Do you think both groups are equally likely to lie?

-   Do you think both groups are equally likely to lie in the same
    direction?

-   How will this affect our results?

Multiple Regression
-------------------

### Case: Direct Marketing

The DirectMarketing dataset shows data from a direct marketer. The
direct marketer sells her products (e.g., clothing, books, or sports
gear) only via direct mail; she sends catalogs with product descriptions
to her customers, and the customers order directly from the catalogs.

She is interested in mining her customers in order to better customize
the marketing process. In particular, she is interested in understanding
what factors drive some customers to spend more money than others. To
that end, she has assembled a database of customer records, including:

-   *Age*: young, middle, and old

-   *Gender*: female/male

-   *OwnHome*: own home or rented home

-   *Married*: single or married

-   *Location*: whether the customer is close or far from the nearest
    brick-and-mortar store selling similar products.

-   *Salary*: in US dollars

-   *Children*: how many children the customer has (between 0 and 3)

-   *History*: past purchasing history (low, medium, or high, or NA if
    the customer has not purchased anything in the past)

-   *Catalogs*: the number of catalogs she has sent to that customer

-   *amountspent*: the amount of money the customer has spent (in US
    dollars).

### Predict Amount Spent

**Goal:** To explain amount spent in terms of the provided customer
characteristics.

Load the data

    dt.mktg <- data.table(read.csv("datasets/DirectMarketing.csv"))
    dt.mktg <- setnames(dt.mktg, tolower(names(dt.mktg)))

**Get to know the data**

    nrow(dt.mktg)

    ## [1] 1000

    colnames(dt.mktg)

    ##  [1] "age"         "gender"      "ownhome"     "married"     "location"   
    ##  [6] "salary"      "children"    "history"     "catalogs"    "amountspent"

    head(dt.mktg)

    ##       age gender ownhome married location salary children history catalogs
    ## 1:    Old Female     Own  Single      Far  47500        0    High        6
    ## 2: Middle   Male    Rent  Single    Close  63600        0    High        6
    ## 3:  Young Female    Rent  Single    Close  13500        0     Low       18
    ## 4: Middle   Male     Own Married    Close  85600        1    High       18
    ## 5: Middle Female     Own  Single    Close  68400        0    High       12
    ## 6:  Young   Male     Own Married    Close  30400        0     Low        6
    ##    amountspent
    ## 1:         755
    ## 2:        1318
    ## 3:         296
    ## 4:        2436
    ## 5:        1304
    ## 6:         495

Next, get summary statistics for the data set. Note that “summary” and
“stargazer” do not give us the same summary statistics. In particular,
while “summary” gives us the number of observations in each category for
categorical variables, stargazer simply ignores these variables.

    summary(dt.mktg)

    ##      age               gender            ownhome            married         
    ##  Length:1000        Length:1000        Length:1000        Length:1000       
    ##  Class :character   Class :character   Class :character   Class :character  
    ##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
    ##                                                                             
    ##                                                                             
    ##                                                                             
    ##    location             salary          children       history         
    ##  Length:1000        Min.   : 10100   Min.   :0.000   Length:1000       
    ##  Class :character   1st Qu.: 29975   1st Qu.:0.000   Class :character  
    ##  Mode  :character   Median : 53700   Median :1.000   Mode  :character  
    ##                     Mean   : 56104   Mean   :0.934                     
    ##                     3rd Qu.: 77025   3rd Qu.:2.000                     
    ##                     Max.   :168800   Max.   :3.000                     
    ##     catalogs      amountspent    
    ##  Min.   : 6.00   Min.   :  38.0  
    ##  1st Qu.: 6.00   1st Qu.: 488.2  
    ##  Median :12.00   Median : 962.0  
    ##  Mean   :14.68   Mean   :1216.8  
    ##  3rd Qu.:18.00   3rd Qu.:1688.5  
    ##  Max.   :24.00   Max.   :6217.0

    stargazer(dt.mktg, type = "text")

    ## 
    ## ========================================================================
    ## Statistic     N      Mean     St. Dev.   Min   Pctl(25) Pctl(75)   Max  
    ## ------------------------------------------------------------------------
    ## salary      1,000 56,103.900 30,616.310 10,100  29,975   77,025  168,800
    ## children    1,000   0.934      1.051      0       0        2        3   
    ## catalogs    1,000   14.682     6.623      6       6        18      24   
    ## amountspent 1,000 1,216.770   961.069     38    488.2   1,688.5   6,217 
    ## ------------------------------------------------------------------------

Explore the data graphically:

    qplot( data = dt.mktg
           , x = age
           , geom = "bar") + theme_bw() + xlim("Young","Middle","Old")

![age-1](https://user-images.githubusercontent.com/73550706/97571206-a1ed0a80-19df-11eb-8c2e-7ea00c21dcd5.png)

    qplot( data = dt.mktg
           , x = gender
           , geom = "bar") + theme_bw()

![gender-1](https://user-images.githubusercontent.com/73550706/97572162-f5f7ef00-19df-11eb-8a3c-fe8a71c32a49.png)

    qplot( data = dt.mktg
           , x = ownhome
           , geom = "bar") + theme_bw()

![ownhome-1](https://user-images.githubusercontent.com/73550706/97572374-0740fb80-19e0-11eb-9820-edeadae926ca.png)

    qplot( data = dt.mktg
           , x = married
           , geom = "bar") + theme_bw()

![married-1](https://user-images.githubusercontent.com/73550706/97572430-1fb11600-19e0-11eb-8d3d-de352cdec2ce.png)

    qplot( data = dt.mktg
           , x = location
           , geom = "bar") + theme_bw()

![location-1](https://user-images.githubusercontent.com/73550706/97572470-3192b900-19e0-11eb-943b-cc2ad81b4dc0.png)

    qplot( data = dt.mktg
           , x = factor(children)
           , geom = "bar") + theme_bw()

![children-1](https://user-images.githubusercontent.com/73550706/97572502-41120200-19e0-11eb-9251-196a7085c6d5.png)


    qplot( data = dt.mktg
           , x = history
           , geom = "bar") + theme_bw()

![history-1](https://user-images.githubusercontent.com/73550706/97572541-54bd6880-19e0-11eb-8e1a-56c15aac4803.png)

    qplot( data = dt.mktg
           , x = factor(catalogs)
           , geom = "bar") + theme_bw()

![catalogs-1](https://user-images.githubusercontent.com/73550706/97572569-630b8480-19e0-11eb-8fb0-bada5f211bef.png)

    qplot( data = dt.mktg
           , x = salary
           , geom = "histogram") + theme_bw()

    ## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.

![salary1-1](https://user-images.githubusercontent.com/73550706/97572619-77e81800-19e0-11eb-8b2b-a532c2c07960.png)

    qplot( data = dt.mktg
           , x = salary
           , geom = "density") + theme_bw()

![salary2-1](https://user-images.githubusercontent.com/73550706/97572681-8df5d880-19e0-11eb-9a44-a1c8997abd42.png)

    qplot( data = dt.mktg
           , x = amountspent
           , geom = "histogram") + theme_bw()

    ## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.

![amountspent1-1](https://user-images.githubusercontent.com/73550706/97572716-9a7a3100-19e0-11eb-8ea5-05bcc5a6f09e.png)

    qplot( data = dt.mktg
           , x = amountspent
           , geom = "density") + theme_bw()

![amountspent2-1](https://user-images.githubusercontent.com/73550706/97572785-b1208800-19e0-11eb-8ee5-39c7d76156ae.png)

Now lets explore the amount spent by the different customer segments

    qplot( data = dt.mktg
           , x = factor(age)
           , y = amountspent
           , geom ="boxplot") + theme_bw() + xlim("Young","Middle","Old")

![box age-1](https://user-images.githubusercontent.com/73550706/97573552-d9f54d00-19e1-11eb-88a9-5c136830b6c9.png)

    qplot( data = dt.mktg
           , x = factor(gender)
           , y = amountspent
           , geom ="boxplot") + theme_bw()

![box gender-1](https://user-images.githubusercontent.com/73550706/97573436-ae726280-19e1-11eb-8d69-bd8be53e4051.png)

    qplot( data = dt.mktg
           , x = factor(ownhome)
           , y = amountspent
           , geom ="boxplot") + theme_bw()

![box ownhome-1](https://user-images.githubusercontent.com/73550706/97573607-f09ba400-19e1-11eb-982a-628403d18db2.png)

    qplot( data = dt.mktg
           , x = factor(married)
           , y = amountspent
           , geom ="boxplot") + theme_bw()

![box married-1](https://user-images.githubusercontent.com/73550706/97573645-03ae7400-19e2-11eb-8644-a2b69e994e45.png)

    qplot( data = dt.mktg
           , x = factor(location)
           , y = amountspent
           , geom ="boxplot") + theme_bw()

![box location-1](https://user-images.githubusercontent.com/73550706/97573694-14f78080-19e2-11eb-8ea7-66a7333ec2ff.png)


    qplot( data = dt.mktg
           , x = factor(children)
           , y = amountspent
           , geom ="boxplot") + theme_bw()

![box children-1](https://user-images.githubusercontent.com/73550706/97573760-26d92380-19e2-11eb-9ba9-6f07e261b78f.png)


    qplot( data = dt.mktg
           , x = factor(history)
           , y = amountspent
           , geom ="boxplot") + theme_bw() + xlim("Low", "Medium", "High", NA)

![box history-1](https://user-images.githubusercontent.com/73550706/97573811-36f10300-19e2-11eb-9353-1a4e36813bbb.png)


    qplot( data = dt.mktg
           , x = factor(catalogs)
           , y = amountspent
           , geom ="boxplot") + theme_bw()

![box catalogs-1](https://user-images.githubusercontent.com/73550706/97573874-4d975a00-19e2-11eb-9b99-41ceb3855c5d.png)

**Simple regression - Interpretation**

    lm1 <- lm(amountspent ~ salary, data = dt.mktg)
    stargazer(lm1, type = "text")

    ## 
    ## ===============================================
    ##                         Dependent variable:    
    ##                     ---------------------------
    ##                             amountspent        
    ## -----------------------------------------------
    ## salary                       0.022***          
    ##                               (0.001)          
    ##                                                
    ## Constant                      -15.318          
    ##                              (45.374)          
    ##                                                
    ## -----------------------------------------------
    ## Observations                   1,000           
    ## R2                             0.489           
    ## Adjusted R2                    0.489           
    ## Residual Std. Error     687.065 (df = 998)     
    ## F Statistic          956.694*** (df = 1; 998)  
    ## ===============================================
    ## Note:               *p<0.1; **p<0.05; ***p<0.01

![equation](https://latex.codecogs.com/gif.latex?%5Cbeta_0) = −15.318
and the corresponding standard error is 45.374.
![equation](https://latex.codecogs.com/gif.latex?%5Cbeta_0) is not
significantly different from zero, thus the absence of stars by this
coefficient.

![equation](https://latex.codecogs.com/gif.latex?%5Cbeta_1) = 0.022, and
the corresponding standard error is 0.001.
![equation](https://latex.codecogs.com/gif.latex?%5Cbeta_1) is
significant at the 1% level, indicated by the three stars by this
coefficient. According to this simple regression model, for each unit
(dollar) increase in the customer’s salary, we can expect an increase of
0.022 units (dollars) in the amount spent by the customer.

The variable *salary* explains 49% of the variation in the variable
*amountspent* (![equation](https://latex.codecogs.com/gif.latex?R%5E2) =
0.489).

    lm2 <- lm(amountspent ~ location, data = dt.mktg)
    stargazer(lm2, type = "text")

    ## 
    ## ===============================================
    ##                         Dependent variable:    
    ##                     ---------------------------
    ##                             amountspent        
    ## -----------------------------------------------
    ## locationFar                 534.773***         
    ##                              (64.837)          
    ##                                                
    ## Constant                   1,061.686***        
    ##                              (34.916)          
    ##                                                
    ## -----------------------------------------------
    ## Observations                   1,000           
    ## R2                             0.064           
    ## Adjusted R2                    0.063           
    ## Residual Std. Error     930.364 (df = 998)     
    ## F Statistic           68.028*** (df = 1; 998)  
    ## ===============================================
    ## Note:               *p<0.1; **p<0.05; ***p<0.01

![equation](https://latex.codecogs.com/gif.latex?%5Cbeta_0) = 1, 061.686
which is the average amount spent by customers who are “close” (where
close is the omitted category of the variable location). You can confirm
this by computing:

    dt.mktg[location=="Close", mean(amountspent)]

    ## [1] 1061.686

![equation](https://latex.codecogs.com/gif.latex?%5Cbeta_1) = 534.7736.
By adding ![equation](https://latex.codecogs.com/gif.latex?%5Cbeta_0) +
![equation](https://latex.codecogs.com/gif.latex?%5Cbeta_1) we get the
average amount spent by customers who are “far”. You can confirm this by
calculating:

    dt.mktg[location=="Far", mean(amountspent)]

    ## [1] 1596.459

    lm3 <- lm(amountspent ~ history, data = dt.mktg)
    stargazer(lm3, type = "text")

    ## 
    ## ===============================================
    ##                         Dependent variable:    
    ##                     ---------------------------
    ##                             amountspent        
    ## -----------------------------------------------
    ## historyLow                 -1,829.050***       
    ##                              (56.917)          
    ##                                                
    ## historyMedium              -1,235.736***       
    ##                              (58.174)          
    ##                                                
    ## Constant                   2,186.137***        
    ##                              (39.196)          
    ##                                                
    ## -----------------------------------------------
    ## Observations                    697            
    ## R2                             0.610           
    ## Adjusted R2                    0.608           
    ## Residual Std. Error     625.902 (df = 694)     
    ## F Statistic          541.884*** (df = 2; 694)  
    ## ===============================================
    ## Note:               *p<0.1; **p<0.05; ***p<0.01

![equation](https://latex.codecogs.com/gif.latex?%5Cbeta_0) = 2, 186.137
which is the average amount spent by customers who have a “high”
purchase history (where “high” is the omitted category of the variable
history). You can confirm this by computing:

    dt.mktg[history=="High", mean(amountspent)]

    ## [1] 2186.137

![equation](https://latex.codecogs.com/gif.latex?%5Cbeta_0) +
![equation](https://latex.codecogs.com/gif.latex?%5Cbeta_1) give us the
average amount spent by customers who have a “low” purchase history. You
can confirm this by computing:

    dt.mktg[history=="Low", mean(amountspent)]

    ## [1] 357.087

![equation](https://latex.codecogs.com/gif.latex?%5Cbeta_0) +
![equation](https://latex.codecogs.com/gif.latex?%5Cbeta_2) give us the
average amount spent by customers who have a “medium” purchase history.
You can confirm this by computing:

    dt.mktg[history=="Medium", mean(amountspent)]

    ## [1] 950.4009

**Multiple regression**

    lm.spend1 <- lm( amountspent ~ gender + location + salary + children + catalogs
    , data = dt.mktg)
    stargazer(lm.spend1 , type = "text")

    ## 
    ## ===============================================
    ##                         Dependent variable:    
    ##                     ---------------------------
    ##                             amountspent        
    ## -----------------------------------------------
    ## genderMale                    -42.309          
    ##                              (33.959)          
    ##                                                
    ## locationFar                 508.129***         
    ##                              (36.207)          
    ##                                                
    ## salary                       0.021***          
    ##                               (0.001)          
    ##                                                
    ## children                    -205.806***        
    ##                              (15.731)          
    ##                                                
    ## catalogs                     42.802***         
    ##                               (2.544)          
    ##                                                
    ## Constant                    -528.143***        
    ##                              (50.454)          
    ##                                                
    ## -----------------------------------------------
    ## Observations                   1,000           
    ## R2                             0.715           
    ## Adjusted R2                    0.714           
    ## Residual Std. Error     514.103 (df = 994)     
    ## F Statistic          499.438*** (df = 5; 994)  
    ## ===============================================
    ## Note:               *p<0.1; **p<0.05; ***p<0.01

One shortcut for including all the variables in your dataset (except the
dependent variable) as independent variables in your model is to use a
“.”.

    lm.spend2 <- lm(amountspent ~ ., data = dt.mktg)
    stargazer(lm.spend1, lm.spend2 , type = "text")

    ## 
    ## ======================================================================
    ##                                    Dependent variable:                
    ##                     --------------------------------------------------
    ##                                        amountspent                    
    ##                               (1)                       (2)           
    ## ----------------------------------------------------------------------
    ## ageOld                                                41.385          
    ##                                                      (52.764)         
    ##                                                                       
    ## ageYoung                                              89.654          
    ##                                                      (58.741)         
    ##                                                                       
    ## genderMale                  -42.309                   -53.701         
    ##                             (33.959)                 (38.016)         
    ##                                                                       
    ## ownhomeRent                                           -18.288         
    ##                                                      (41.512)         
    ##                                                                       
    ## marriedSingle                                         19.503          
    ##                                                      (49.812)         
    ##                                                                       
    ## locationFar                508.129***               608.992***        
    ##                             (36.207)                 (43.985)         
    ##                                                                       
    ## salary                      0.021***                 0.019***         
    ##                             (0.001)                   (0.001)         
    ##                                                                       
    ## children                  -205.806***               -268.283***       
    ##                             (15.731)                 (25.019)         
    ##                                                                       
    ## historyLow                                          -267.514***       
    ##                                                      (88.617)         
    ##                                                                       
    ## historyMedium                                       -344.553***       
    ##                                                      (59.964)         
    ##                                                                       
    ## catalogs                   42.802***                 40.521***        
    ##                             (2.544)                   (2.868)         
    ##                                                                       
    ## Constant                  -528.143***                -249.579*        
    ##                             (50.454)                 (134.031)        
    ##                                                                       
    ## ----------------------------------------------------------------------
    ## Observations                 1,000                      697           
    ## R2                           0.715                     0.789          
    ## Adjusted R2                  0.714                     0.785          
    ## Residual Std. Error    514.103 (df = 994)       463.457 (df = 685)    
    ## F Statistic         499.438*** (df = 5; 994) 232.493*** (df = 11; 685)
    ## ======================================================================
    ## Note:                                      *p<0.1; **p<0.05; ***p<0.01

Note that by including the variable history (which has missing data) in
the regression model, we lose observations.

The interpretation of the coefficients of continuous variables, such as
salary, is straightforward: the coefficient gives you the unit change in
your dependent variable that results from a unit change in your
independent variable.

The coefficients of dummy variables tell you how people in that category
behave differently from people in the corresponding omitted category.
For instance, the coefficient 508.129 of the variable *locationFar*
tells you that, on average, people who are “far” spend more than people
who are “close”.

### Predict amount spent by new customer

Now lets predict the amount spent for a new customer:

    new.client <- data.table( gender = "Male"
    , location = "Close"
    , salary = 53700
    , children = 1
    , catalogs = 12)
    new.client

    ##    gender location salary children catalogs
    ## 1:   Male    Close  53700        1       12

    my.pred <- predict(lm.spend1, newdata = new.client)
    my.pred

    ##        1 
    ## 868.9695

We can also add in a 95% confidence interval to our prediction

    my.pred <- predict(lm.spend1, newdata = new.client, interval="prediction", level = .95)
    my.pred

    ##        fit       lwr      upr
    ## 1 868.9695 -141.2554 1879.194
