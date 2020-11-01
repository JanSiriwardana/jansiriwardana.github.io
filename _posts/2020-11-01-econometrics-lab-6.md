---
layout: post
title:  "Econometrics Lab 6"
date:   2020-11-01 11:00:00
categories: jekyll update
---
Janithe Siriwardana

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

### Load data

    load("datasets/hprice1.RData")
    dt.hprice1 <- data.table(data)
    rm(data)
    colnames(dt.hprice1)

    ##  [1] "price"    "assess"   "bdrms"    "lotsize"  "sqrft"    "colonial"
    ##  [7] "lprice"   "lassess"  "llotsize" "lsqrft"

Exercise 1
----------

Estimate the model
![equation](https://latex.codecogs.com/gif.latex?price%20%3D%5Cbeta_0%20+%20%5Cbeta_1sqrft+%20%5Cbeta_2bdrms+u)
where price is house price measured in thousands of dollars

    reg1 <- lm(price ~ sqrft + bdrms, data = dt.hprice1)
    stargazer(reg1, type = "text")

    ## 
    ## ===============================================
    ##                         Dependent variable:    
    ##                     ---------------------------
    ##                                price           
    ## -----------------------------------------------
    ## sqrft                        0.128***          
    ##                               (0.014)          
    ##                                                
    ## bdrms                         15.198           
    ##                               (9.484)          
    ##                                                
    ## Constant                      -19.315          
    ##                              (31.047)          
    ##                                                
    ## -----------------------------------------------
    ## Observations                    88             
    ## R2                             0.632           
    ## Adjusted R2                    0.623           
    ## Residual Std. Error      63.045 (df = 85)      
    ## F Statistic           72.964*** (df = 2; 85)   
    ## ===============================================
    ## Note:               *p<0.1; **p<0.05; ***p<0.01

**i) Write out the results in equation form**

![equation](https://latex.codecogs.com/gif.latex?price%20%3D%20-19.315%20+%200.128sqrft%20+%2015.198bdrms)

**ii) What is the estimated increase in price for a house with one more
bedroom, holding square footage constant?**

$15,198

**iii) What is the estimated increase in price for a house with an
additional bedroom that is 140 square feet in size? Compare this to your
answer in part (ii).**

$33,118. It is more than twice the value in (ii)

**iv) What percentage of the variation in price is explained by square
footage and number of bedrooms?**

![equation](https://latex.codecogs.com/gif.latex?R%5E2)=0.632, so
percentage of variable explained by model is 63.2%

**v) The first house in the sample has sqrft = 2,438 and bdrms = 4. Find
the predicted selling price for this house from the OLS regression
line.**

    first.house <- data.table( sqrft = dt.hprice1[1, sqrft]
                               , bdrms = dt.hprice1[1, bdrms])

    my.pred <- predict(reg1, newdata = first.house)
    my.pred

    ##        1 
    ## 354.6052

The predicted sale price of first house is $354,605

**vi) The actual selling price of the first house in the sample was
$300,000 (so price=300). Find the residual for this house. Does it
suggest that the buyer underpaid or overpaid for the house?**

    first.house$u_hat <- my.pred - dt.hprice1[1, price]
    first.house$u_hat2 <- (first.house$u_hat)^2
    head(first.house)

    ##    sqrft bdrms    u_hat   u_hat2
    ## 1:  2438     4 54.60525 2981.733

The residual is $54,605. This suggests the buyer underpaid for the house
significantly.

**vii) Now add the variable colonial to your model. Interpret its
coefficient. Is it significant?**

    reg2 <- lm(price ~ sqrft + bdrms + colonial, data = dt.hprice1)
    stargazer(reg2, type = "text")

    ## 
    ## ===============================================
    ##                         Dependent variable:    
    ##                     ---------------------------
    ##                                price           
    ## -----------------------------------------------
    ## sqrft                        0.130***          
    ##                               (0.014)          
    ##                                                
    ## bdrms                         12.487           
    ##                              (10.024)          
    ##                                                
    ## colonial                      13.078           
    ##                              (15.436)          
    ##                                                
    ## Constant                      -21.552          
    ##                              (31.210)          
    ##                                                
    ## -----------------------------------------------
    ## Observations                    88             
    ## R2                             0.635           
    ## Adjusted R2                    0.622           
    ## Residual Std. Error      63.150 (df = 84)      
    ## F Statistic           48.720*** (df = 3; 84)   
    ## ===============================================
    ## Note:               *p<0.1; **p<0.05; ***p<0.01

    summary(reg2)

    ## 
    ## Call:
    ## lm(formula = price ~ sqrft + bdrms + colonial, data = dt.hprice1)
    ## 
    ## Residuals:
    ##      Min       1Q   Median       3Q      Max 
    ## -129.793  -40.167   -5.588   30.447  227.011 
    ## 
    ## Coefficients:
    ##              Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept) -21.55241   31.21022  -0.691    0.492    
    ## sqrft         0.12985    0.01395   9.310 1.41e-14 ***
    ## bdrms        12.48749   10.02366   1.246    0.216    
    ## colonial     13.07755   15.43591   0.847    0.399    
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 63.15 on 84 degrees of freedom
    ## Multiple R-squared:  0.635,  Adjusted R-squared:  0.622 
    ## F-statistic: 48.72 on 3 and 84 DF,  p-value: < 2.2e-16

The coefficient on colonial is 13.078 which means that a colonial house,
all else constant is worth $13,078 than a non-colonial house. The
p-value = 0.399 &gt; 0.05 so it is not significant.

Exercise 2
----------

    load("datasets/ceosal2.RData")
    dt.ceo.sal <- data.table(data)
    rm(data)

**i) Estimate a model relating annual salary to firm sales and market
value. Make the model of the constant elasticity variety for both
independent variables. Write the results out in equation form.**

    lm.ceo.sal <- lm(salary ~ sales + mktval, data = dt.ceo.sal)
    log.lm.ceo.sal <- lm(lsalary ~ lsales + lmktval, data = dt.ceo.sal)
    stargazer(lm.ceo.sal, log.lm.ceo.sal, type = "text")

    ## 
    ## ===========================================================
    ##                                    Dependent variable:     
    ##                                ----------------------------
    ##                                    salary        lsalary   
    ##                                     (1)            (2)     
    ## -----------------------------------------------------------
    ## sales                              0.016                   
    ##                                   (0.010)                  
    ##                                                            
    ## mktval                            0.025***                 
    ##                                   (0.010)                  
    ##                                                            
    ## lsales                                          0.162***   
    ##                                                  (0.040)   
    ##                                                            
    ## lmktval                                          0.107**   
    ##                                                  (0.050)   
    ##                                                            
    ## Constant                         716.576***     4.621***   
    ##                                   (47.188)       (0.254)   
    ##                                                            
    ## -----------------------------------------------------------
    ## Observations                        177            177     
    ## R2                                 0.178          0.299    
    ## Adjusted R2                        0.168          0.291    
    ## Residual Std. Error (df = 174)    535.894         0.510    
    ## F Statistic (df = 2; 174)        18.797***      37.129***  
    ## ===========================================================
    ## Note:                           *p<0.1; **p<0.05; ***p<0.01

![equation](https://latex.codecogs.com/gif.latex?salary%20%3D%20716.576%20+%200.016sales%20+%200.025mktval)
![equation](https://latex.codecogs.com/gif.latex?%5Clog%20salary%20%3D%204.621%20+%200.162%20%5Clog%20sales%20+%200.107%20%5Clog%20mktval)

**ii) Add profits to the model from part (i). Why can’t this variable
not be included in logarithmic form? Would you say that these firm
performance variables explain most of the variation in CEO salaries?**

    log.lm.ceo.sal2 <- lm(lsalary ~ lsales + lmktval + profits, data = dt.ceo.sal)
    stargazer(log.lm.ceo.sal2, type = "text")

    ## 
    ## ===============================================
    ##                         Dependent variable:    
    ##                     ---------------------------
    ##                               lsalary          
    ## -----------------------------------------------
    ## lsales                       0.161***          
    ##                               (0.040)          
    ##                                                
    ## lmktval                        0.098           
    ##                               (0.064)          
    ##                                                
    ## profits                       0.00004          
    ##                              (0.0002)          
    ##                                                
    ## Constant                     4.687***          
    ##                               (0.380)          
    ##                                                
    ## -----------------------------------------------
    ## Observations                    177            
    ## R2                             0.299           
    ## Adjusted R2                    0.287           
    ## Residual Std. Error      0.512 (df = 173)      
    ## F Statistic           24.636*** (df = 3; 173)  
    ## ===============================================
    ## Note:               *p<0.1; **p<0.05; ***p<0.01

Profit cannot be included in log form because some elements are
negative. Since R^2 is only 0.299, only 29.9% of the variation in CEO
salaries is explained by this model.

**iii) Interpret the coefficient of lmktval. Does it have a significant
effect? Explain**

    summary(log.lm.ceo.sal2)

    ## 
    ## Call:
    ## lm(formula = lsalary ~ lsales + lmktval + profits, data = dt.ceo.sal)
    ## 
    ## Residuals:
    ##      Min       1Q   Median       3Q      Max 
    ## -2.27002 -0.31026 -0.01027  0.31043  1.91489 
    ## 
    ## Coefficients:
    ##              Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept) 4.687e+00  3.797e-01  12.343  < 2e-16 ***
    ## lsales      1.614e-01  3.991e-02   4.043 7.92e-05 ***
    ## lmktval     9.753e-02  6.369e-02   1.531    0.128    
    ## profits     3.566e-05  1.520e-04   0.235    0.815    
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 0.5117 on 173 degrees of freedom
    ## Multiple R-squared:  0.2993, Adjusted R-squared:  0.2872 
    ## F-statistic: 24.64 on 3 and 173 DF,  p-value: 2.53e-13

The coefficient 0.098 in the last model for lmktval means that a 1
percent increase in market value of the firm is associated with a 0.098%
increase in CEO salary. The p-value = 0.128 &gt; 0.05 so it is not
significant.

**iv) Interpret the coefficient of profits. Does it have a significant
effect? Explain.**

    (exp(coef(log.lm.ceo.sal2)["profits"])-1)*100

    ##     profits 
    ## 0.003566069

The coefficient for profit is 0.00004 which means that a $1m increase in
profits increases CEO salary by 0.004%

**v) Add the variable ceoten and comten to the model in part (ii). What
is the estimated percentage return for another year of CEO tenure,
holding other factors fixed?**

    log.lm.ceo.sal3 <- lm(lsalary ~ lsales + lmktval + profits + ceoten + comten, data = dt.ceo.sal)
    stargazer(log.lm.ceo.sal3, type = "text")

    ## 
    ## ===============================================
    ##                         Dependent variable:    
    ##                     ---------------------------
    ##                               lsalary          
    ## -----------------------------------------------
    ## lsales                       0.191***          
    ##                               (0.040)          
    ##                                                
    ## lmktval                        0.077           
    ##                               (0.062)          
    ##                                                
    ## profits                       0.0001           
    ##                              (0.0001)          
    ##                                                
    ## ceoten                       0.017***          
    ##                               (0.006)          
    ##                                                
    ## comten                       -0.010***         
    ##                               (0.003)          
    ##                                                
    ## Constant                     4.697***          
    ##                               (0.376)          
    ##                                                
    ## -----------------------------------------------
    ## Observations                    177            
    ## R2                             0.349           
    ## Adjusted R2                    0.330           
    ## Residual Std. Error      0.496 (df = 171)      
    ## F Statistic           18.342*** (df = 5; 171)  
    ## ===============================================
    ## Note:               *p<0.1; **p<0.05; ***p<0.01

    (exp(coef(log.lm.ceo.sal3)["ceoten"])-1)*100

    ##  ceoten 
    ## 1.70879

For every additonal year of CEO tenure the estimated percentage return
is 1.71%

**vi) Interpret the coefficients on ceoten and comten. Are these
explanatory variables statistically significant?**

    (exp(coef(log.lm.ceo.sal3)["comten"])-1)*100

    ##     comten 
    ## -0.9493218

For every additional year of company tenure the estimated percentage
return is -0.949%

    summary(log.lm.ceo.sal3)

    ## 
    ## Call:
    ## lm(formula = lsalary ~ lsales + lmktval + profits + ceoten + 
    ##     comten, data = dt.ceo.sal)
    ## 
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -2.5307 -0.2737 -0.0251  0.2910  1.9999 
    ## 
    ## Coefficients:
    ##               Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)  4.697e+00  3.759e-01  12.497  < 2e-16 ***
    ## lsales       1.909e-01  3.998e-02   4.774 3.86e-06 ***
    ## lmktval      7.726e-02  6.237e-02   1.239  0.21713    
    ## profits      6.456e-05  1.479e-04   0.437  0.66298    
    ## ceoten       1.694e-02  5.552e-03   3.052  0.00264 ** 
    ## comten      -9.539e-03  3.354e-03  -2.844  0.00500 ** 
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 0.4961 on 171 degrees of freedom
    ## Multiple R-squared:  0.3491, Adjusted R-squared:  0.3301 
    ## F-statistic: 18.34 on 5 and 171 DF,  p-value: 1.473e-14

The p-values for both ceoten and comten are &lt;0.01 so both are
significant.

**vii) What do you make of the fact that longer tenure with the company,
holding the other factors fixed, is associated with a lower salary?**

While it might be initially surprising, this could reflect the
‘superstar’ effect whereby firms pay more for highly regarded external
candidates.

**viii) Predict the salaries for all the CEOs in the sample**

    dt.ceo.sal$y_hatlog <- predict(log.lm.ceo.sal3, newdata=dt.ceo.sal)
    dt.ceo.sal$y_hat <- exp(dt.ceo.sal$y_hatlog)
    head(dt.ceo.sal)

    ##    salary age college grad comten ceoten sales profits mktval  lsalary   lsales
    ## 1:   1161  49       1    1      9      2  6200     966  23200 7.057037 8.732305
    ## 2:    600  43       1    1     10     10   283      48   1100 6.396930 5.645447
    ## 3:    379  51       1    1      9      3   169      40   1100 5.937536 5.129899
    ## 4:    651  55       1    0     22     22  1100     -54   1000 6.478509 7.003066
    ## 5:    497  44       1    1      8      6   351      28    387 6.208590 5.860786
    ## 6:   1067  64       1    1      7      7 19000     614   3900 6.972606 9.852194
    ##      lmktval comtensq ceotensq  profmarg y_hatlog     y_hat
    ## 1: 10.051908       81        4 15.580646 7.151210 1275.6483
    ## 2:  7.003066      100      100 16.961130 6.393152  597.7380
    ## 3:  7.003066       81        9 23.668638 6.185158  485.4899
    ## 4:  6.907755      484      484 -4.909091 6.727215  834.8188
    ## 5:  5.958425       64       36  7.977208 6.303558  546.5127
    ## 6:  8.268732       49       49  3.231579 7.308279 1492.6057

**ix) Use the predicted values to compute R2**

    SSR <- sum((dt.ceo.sal$y_hat - dt.ceo.sal$salary)^2)
    TSS <- sum((dt.ceo.sal[, mean(salary)] - dt.ceo.sal$salary)^2)
    Rsquared <- 1 - (SSR/TSS)
    Rsquared

    ## [1] 0.223594

Rsquared = 0.224

**x) Calculate the residuals**

    dt.ceo.sal$u_hat <- dt.ceo.sal$y_hat - dt.ceo.sal$salary
    dt.ceo.sal$u_hat2 <- (dt.ceo.sal$u_hat)^2

**xi) Find the sample correlation coefficient between the variables
log(mktval) and profits. Are these variables highly correlated? What
does this say about the OLS estimators?**

    cor(dt.ceo.sal$lmktval, dt.ceo.sal$profits)

    ## [1] 0.7768976

The two variables are quite highly correlated. Typically this is not too
much of an issue unless this linear dependence is very close to 1 or
equal to 1 such that there is perfect multicollinearity. In this case
rank(X) &lt; k so X’X is invertible and
![equation](https://latex.codecogs.com/gif.latex?%5Chat%5Cbeta) cannot
be computed.

Exercise 3
----------

    load("datasets/attend.RData")
    dt.attend <- data.table(data)
    rm(data)
    colnames(dt.attend)

    ##  [1] "attend"  "termGPA" "priGPA"  "ACT"     "final"   "atndrte" "hwrte"  
    ##  [8] "frosh"   "soph"    "missed"  "stndfnl"

**i) Obtain the minimum, maximum, and average values for the variables
atndrte, priGPA, and ACT.**

    dt.attend[, max(atndrte)]

    ## [1] 100

    dt.attend[, min(atndrte)]

    ## [1] 6.25

    dt.attend[, mean(atndrte)]

    ## [1] 81.70956

    dt.attend[, max(priGPA)]

    ## [1] 3.93

    dt.attend[, min(priGPA)]

    ## [1] 0.857

    dt.attend[, mean(priGPA)]

    ## [1] 2.586775

    dt.attend[, max(ACT)]

    ## [1] 32

    dt.attend[, min(ACT)]

    ## [1] 13

    dt.attend[, mean(ACT)]

    ## [1] 22.51029

**ii) How many students are in their freshman year?**

    dt.attend[, sum(frosh)]

    ## [1] 158

**iii) How many of the students in freshmen year have missed more than
10 classes?**

    dt.attend[missed>10, sum(frosh)]

    ## [1] 29

**iv) Is it true that, on average, students who miss less than 10
classes have a higher GPA? State the null and alternative hypothesis and
conduct the appropriate test.**

![equation](https://latex.codecogs.com/gif.latex?H_0%3A%5Cmu_%7B%28missed%3C10%29%7D-%5Cmu_%7B%28missed%5Cgeq10%29%7D%5Cleq0)

![equation](https://latex.codecogs.com/gif.latex?H_1%3A%5Cmu_%7B%28missed%3C10%29%7D-%5Cmu_%7B%28missed%5Cgeq10%29%7D%3E0)

    dt.attend[, t.test(termGPA ~ (missed>=10), alternative = c("greater"))]

    ## 
    ##  Welch Two Sample t-test
    ## 
    ## data:  termGPA by missed >= 10
    ## t = 12.237, df = 150.04, p-value < 2.2e-16
    ## alternative hypothesis: true difference in means is greater than 0
    ## 95 percent confidence interval:
    ##  0.8018051       Inf
    ## sample estimates:
    ## mean in group FALSE  mean in group TRUE 
    ##            2.761899            1.834686

The p-value&lt;0.05 so we can reject the null hypothesis. There is a
difference in average term GPA between those who missed less than 10
classes and those who missed 10 more classes. Additionally, zero is not
in the 95% confidence interval.

**v) Estimate the model
![equation](https://latex.codecogs.com/gif.latex?atndrte%20%3D%20%5Cbeta_0%20+%20%5Cbeta_%201priGPA%20+%20%5Cbeta_%202ACT%20+%20u),
and write the results in equation form.Interpret the intercept. Does it
have a useful meaning?**

    lm.attend <- lm(atndrte ~ priGPA + ACT, data=dt.attend)
    stargazer(lm.attend, type = "text")

    ## 
    ## ===============================================
    ##                         Dependent variable:    
    ##                     ---------------------------
    ##                               atndrte          
    ## -----------------------------------------------
    ## priGPA                       17.261***         
    ##                               (1.083)          
    ##                                                
    ## ACT                          -1.717***         
    ##                               (0.169)          
    ##                                                
    ## Constant                     75.700***         
    ##                               (3.884)          
    ##                                                
    ## -----------------------------------------------
    ## Observations                    680            
    ## R2                             0.291           
    ## Adjusted R2                    0.288           
    ## Residual Std. Error      14.379 (df = 677)     
    ## F Statistic          138.651*** (df = 2; 677)  
    ## ===============================================
    ## Note:               *p<0.1; **p<0.05; ***p<0.01

![equation](https://latex.codecogs.com/gif.latex?atndrte%20%3D%2075.700%20+%2017.261priGPA%20-1.717ACT)

The intercept term suggests that for a person with cumulative GPA prior
to term equal to zero and an ACT score of zero, their percentage
attendance would be 75.7%.

**vi) Discuss the estimated slope coefficients. Are there any
surprises?**

The slope coefficient for prior term GPA is not suprising but the fact
that the ACT coefficient is negative may be surprisingly initially but
could be explained by extremely capable students skipping class.

**vii) What is the predicted atndrte if priGPA=3.65 and ACT=20? What do
you make of this result? Are there any students in the sample with these
values of the explanatory variables?**

    new.student <- data.table( priGPA = 3.65
                              , ACT = 20)
    pred.new.student <- predict(lm.attend, newdata = new.student)
    pred.new.student

    ##        1 
    ## 104.3705

This is a surprising result because it suggests students with these
characteristics attend more classes than available

    (dt.attend[priGPA==3.65, sum(ACT=20)])

    ## [1] 20

There is one student who has these characteristics

**viii) If Student A has priGPA=3.1 and ACT=21 and Student B has
priGPA=2.1 and ACT=26, what is \#the predicted difference in their
attendance rates?**

    studentA <- data.table( priGPA = 3.61
                               , ACT = 21)
    pred.studentA <- predict(lm.attend, newdata = studentA)
    pred.studentA

    ##        1 
    ## 101.9635

    studentB <- data.table( priGPA = 2.1
                            , ACT = 26)
    pred.studentB <- predict(lm.attend, newdata = studentB)
    pred.studentB

    ##        1 
    ## 67.31727

    pred.studentA - pred.studentB

    ##        1 
    ## 34.64626

**ix) Should we include attend along with alcohol as explanatory
variables in a multiple regression model? (Think about how you would
interpret alcohol.)**

The coefficient on alcohol would be interpreted as change in college GPA
per unit of alcohol consumption. I would include attendance as
intuitively it should have some explanatory power over GPA but there may
be some correlation between alcohol consumption and attendance.

**x) Should SAT and hsGPA be included as explanatory variables?
Explain.**

Again there is likely to be correlation between these two variables so
it might be that only one is needed in the model.

Exercise 4
----------

**i) Load the dataset MEAP93.RData and obtain the summary statistics.**

    load("datasets/meap93.RData")
    dt.math<-data.table(data)
    rm(data)

**ii) We want to explore the relationship between the math pass rate
(math10) and spending per student (expend). Do you think each additional
dollar spent has the same effect on the pass rate, or does a diminishing
effect seem more appropriate? Explain.**

There is likely to be a diminishing effect, as eventually the marginal
gains from extra study get less and less (and may even reverse)

**iii) In the population model
![equation](https://latex.codecogs.com/gif.latex?math10%20%3D%20%5Cbeta_0%20+%20%5Cbeta_%201log%28expend%29%20+%20%5Cmu)
argue that
![equation](https://latex.codecogs.com/gif.latex?%5Cbeta_1)/10 is the
percentage point change in math 10 given a 10% increase in expend.**

The interpretation of a lin-log model is if we increase x by one
percent, we expect y to increase by (β1/100) units of y. However, since math10 goes from 1-100, β1 is the percent change in math10 given a 100% increase in expend. Therefore β1/10 is the percent change in math10 given a 10% increase in spend.

**iv) Use the data in MEAP93.RAW to estimate the model from part (ii).
Report the estimated equation in the usual way, including the sample
size and R-squared.**

    lm.math <- lm(math10 ~ log(expend), data=dt.math)
    stargazer(lm.math, type = "text")

    ## 
    ## ===============================================
    ##                         Dependent variable:    
    ##                     ---------------------------
    ##                               math10           
    ## -----------------------------------------------
    ## log(expend)                  11.164***         
    ##                               (3.169)          
    ##                                                
    ## Constant                    -69.341***         
    ##                              (26.530)          
    ##                                                
    ## -----------------------------------------------
    ## Observations                    408            
    ## R2                             0.030           
    ## Adjusted R2                    0.027           
    ## Residual Std. Error      10.350 (df = 406)     
    ## F Statistic           12.411*** (df = 1; 406)  
    ## ===============================================
    ## Note:               *p<0.1; **p<0.05; ***p<0.01

![equation](https://latex.codecogs.com/gif.latex?math10%3D%20-69.341%20+%2011.164%5Clog%28expend%29)

Sample size = 408 Rsquared = 0.030

**v) How big is the estimated spending effect? Namely, if spending
increases by 10%, what is the estimated percentage point increase in
math10?**

The percentage increase in math 10 given a 10% increase in spending is
1.12%

**vi) One might worry that regression analysis can produce fitted values
for math10 that are greater than 100. Why is this not much of a worry in
this data set?**

    dt.math[,max(math10)]

    ## [1] 66.7

Since the maximum starting point for math scores in our data set is 66.7
we would require very large increases in spending for out fitted values
to potentially go above 100.
