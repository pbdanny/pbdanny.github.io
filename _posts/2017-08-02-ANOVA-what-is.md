---
layout: post
title: "ANOVA, what really it is?"
---

## What really is the ANOVA?

ANOVA came from **AN**alysis **O**f **VA**rience which is statistical technique to inference about the sample data by focusing analysis on the varience; to be exact focusing on **Sum Square** \\(SS\\) analysis with equation

\\[ \sum_{i=1}^n (y\_{i} - \bar{y})^2 \\]

The methode of analysis could summarize in 3 steps

**1. Identify original state** or null hypothsis (\\(H\_{0}\\))
  For testing difference mean of 2 or more groups of data, we start with original state that all the data have **no** subgroup with their own _mean_ or _moment_, all the data varience could be best represented with \\(SS\\) that _minimized_ at grand _moment_, or **grand mean** \\(\bar{Y}\bullet\bullet\\). The result \\(SS\\) of original state will be \\(SS\\) on grand means with equation 
  \\[ SS_{Total} = \sum_{i=1}^n (y\_{i} - \bar{y})^2 \\]
    
  For the linear regression \(y \sim x\) the original state we start with the regression parameter \\(\beta\_{i}\\) of idependent varible \\(x\\) have **no** effect (or correlation) on \\(SS\\) of dependent varible \\(y\\), and the varience of \\(y\\) is best represented by the \\(SS\\) that _minimized_ at the _mean_ or _moment_ of \\(y\\) itself. (Quite confusing, yess.) The result \\(SS\\) oft original state will be \\(SS\\) on it's \\(y\\)'s mean it self with equaltion
  \\[ SS_{Total} = \sum_{i=1}^n (y\_{i} - \bar{y})^2 \\] 

**2. Calculate the effect of _Treatment_** or alternate hypothesis (\\(H\_{a}\\))
  For testing difference mean of 2 or more group of data, then we guess that there are better _moments of each group_ that yield _more minimized_ \\(SS\\) when each data point gravitated towards its own mean \\(\bar{Y}_{k}\bullet\\). The result \\(SS\\) of _treatment_ state will be \\(SS\\) of each data point on group mean 
  
  \\[ SS_{Treatment} = \sum_{j=1}^k \sum_{i=1}^n n_j(\bar{y}\_{j}\bullet - \bar{y}\bullet\bullet)^2 \\] to clarily we use \\(SS_{Group}\\}
  
  The left-over of \\(SS\\) or **residual** will be \\(SS_{resid}\\) = \\(SS_{total} - SS_{Treatment} \\)
    
  For the linear regression \\(y \sim x\\) , we guess that the regression parameter \\(\beta\_{i}\\) of idependent varible \\(x\\) have effect on the \\(SS\\) of \\(y\\), and the varience of \\(y\\) could be represented with the \\(SS\\) of _regression point_ \\(\hat{y}\\) or \\(\beta_{0} + \beta_{1}x\\).
  
  The \\(SS\\) of _regression line_ could be represented with quation
  \\[SS_{Treatment} = \sum_{i=1}^n(\hat{y}_{i} - y_{i})^2 \\] to clarify we use \\(SS_{Regression}\\) in the same meaning
  
  The left-over of \\(SS\\) or **residual** will be \\(SS_{resid}\\) = \\(SS_{total} - SS_{Regression} \\)

**3. Calcualate the Mean square** and find the _degree of freedom_
  the **Mean square** equation is
  \\[MS = \frac{SS}{degree of freedom} \\]
  
  The _Degree of freedom (df)_ is the penalties for using point estimator *(like sample mean \\(\bar{y} \\) in calculating varience (What?) \\( note: Var(y) = \sum_{i}^n (y\_{i} - \bar{y})^2 \\). In general the degree of freedom = the number of parameters - the nubmer of parameters used in calculate varience
  For example, the degree of freedom (df) of \\(SS_{Group}\\) (different group mean) = number of group (k) - 1, which the -1 come from the calculation of SS we use 1 parameter \\(\bar{y}_{j}\bullet\\) for calcualte \\(SS\\) then -1 deduct from k to be k-1).
  
  We use df. to caculate Mean Square by
  
  \\[\frac{SS}{df} \\]

**4. Calculate F-Statistic**
  The _F-statistic_ is the proportion of \\( \frac{MS_{Treatment}}{MS_{residual}} \\). The motivation behind the F-statistic is if the \\(MS_{Treatment}\\) could be better represent the varience; than the Residual varience \\(MS_{residual}\\) will be very little compare to the \\(MS_{Treatment}\\) result in the more right side p-value, then reject \\(H_{0}\\)


  we can simulate the anova test of different mean in R
  
```r
r <- rnorm(n = 50, mean = 20, sd =1)
s <- rnorm(n = 50, mean = 0, sd = 1)
d <- as.data.frame(c(r,s))
colnames(d) <- "d"
plot(density(d$d))    
```
  Result in picture below, with grand mean = 9.964 and mean of group r = 20, while mean of group s = 0.
  
![anova pic](../images/Anova-1.png)

  Let's calculate Sum Square total and sum square of treatment and sum square of total.
  
```r
# sum square total
total.ss <- sum((d$d - mean(d$d))^2)

# sum square treatment
treat.ss.r <- ((mean(r) - mean(d$d))^2)*length(r)
treat.ss.s <- ((mean(s) - mean(d$d))^2)*length(s)
treat.ss <- treat.ss.r + treat.ss.s

# sum square error/residual
e.ss <- total.ss - treat.ss
```
  The sum square total = 10064.57, sum square treatment = 9961.857 and sum square error = 102.711. the mean square of treatment = SS. of treatment / df. of treatment (# of group - 1, 2 - 1 = 1). and the mean square of error = SS. of error/ df. of error (# total observation -  # group = 100 - 2 = 98)
  The result F-statistic = ms. of treatment / ms. of error = 9961.857/1.0588 = 9407.954. Calculate right side F-statistic from R by,
  
```r
pf(9407.857, df1 = 1, df2 = 98, lower.tail = F)
```

![F statistic pic](../images/f-table.jpg)

  P-value = 0 that means the chance of equal mean of each group = 0, or each group have different mean.

  
  We could simulate f-statistic from very messy data with the same process
```r
r <- rnorm(n = 50, mean = 19, sd = 20)
s <- rnorm(n = 50, mean = 20, sd = 20)
d <- as.data.frame(c(r,s))
colnames(d) <- "d"
plot(density(d$d))
total.ss <- sum((d$d - mean(d$d))^2)

treat.ss.r <- ((mean(r) - mean(d$d))^2)*length(r)
treat.ss.s <- ((mean(s) - mean(d$d))^2)*length(s)
treat.ss <- treat.ss.r + treat.ss.s

e.ss <- total.ss - treat.ss

treat.ms <- treat.ss/1
e.ms <- e.ss/(100-2-1)

f.stat <- treat.ms/e.ms
f.stat

pf(f.stat, df1 = 1, df2 = 98, lower.tail = F)
```
  The code above result f-statistic = 0.0097 that could be interpret that the *treatment* of mean could not best represent the data than the *original* mean or grand mean. With p-value = 0.921.

  We could use r function *aov* to comparing different mean between group as command below.
```r
# create data frame with group variable
r.d <- data.frame(data = r, group = 'r')
s.d <- data.frame(data = s, group = 's')
d <- rbind(r.d, s.d)

summary(aov(data ~ group, data = d))
            Df Sum Sq Mean Sq F value Pr(>F)
group        1      4     4.1    0.01  0.921
Residuals   98  40584   414.1 
```

### What about anova in assesment linear regression model.

  After fit the model we not only do t-test on each independent variables, but also do the anova to test the difference  *varience explained by Beta1* of each model with command

```r
anova(fit1, fit2)
```
  Noted here that the anova test of linear regression model in 'R' vary from test on SPSS. Since 'R' use anova **Type-1** in testing while 'SPSS' use **TYPE-III** in testing.
  
  Further details could find by linked below.
  
[One Way Anova](https://egret.psychol.cam.ac.uk/statistics/R/anova.html#technicalities)

[Basic Anova for linear regression evaluation](http://reliawiki.org/index.php/Simple_Linear_Regression_Analysis)

[Anova 'R' vs 'SPSS'](https://mcfromnz.wordpress.com/2011/03/02/anova-type-iiiiii-ss-explained/)

[Anova TYPE-I,II,III](http://goanna.cs.rmit.edu.au/~fscholer/anova.php)
