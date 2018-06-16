---
layout: post
title: "Predicting commitments in the NCAAF 2018 Recruitment class"
modified:
excerpt: ""
tags: [NCAAF, R, RevolUTion]
modified: 2018-06-5
comments: true
---

Recruiting is the essence of College Football. 



In this analysis I'm pulled the player timeline records from <a href="https://247sports.com/Season/2018-Football/" target="_blank">247Sports</a>. 

Georgio, Ohio State, Texas, South California, and Clemson finished in the top 5. Basing myself exclusively in the data available in this site, I 


Recruiting events tracked are:

1. Offer
2. School Camp
3. Unofficial Visit
4. Official Visit
5. Coach Visit
6. Decommit


On average, recruits received 12.7 offers. As the plot below shows, the offer count seems to be all over the place.

2.32 School Vistis, 6.19 Unoficial Visits, 1.97 Official Visits, 3.04 Coach Visits, 


Using this information, how well can schools predict if a recruit is commiting?

After extensive data manipulations, I create at table where each row represents a unique recruit and school that interacted with him. The metric columns (other than rank, in_state, and won) are the sum of events.

<p><br></p>
```{r}
  name        actionSchool                 rank offer schoolCamp uvisit cvisit ovisit commits decommits in_state   won
  <chr>       <fct>                       <dbl> <int>      <int>  <int>  <int>  <int>   <int>     <int>    <dbl> <int>
1 Aaron Brule Cincinnati Bearcats           496     1          0      0      0      0       0         0        0     0
2 Aaron Brule Georgia Bulldogs              496     1          0      1      0      0       1         1        0     0
3 Aaron Brule Georgia Tech Yellow Jackets   496     1          0      0      0      0       0         0        0     0
4 Aaron Brule Mississippi State Bulldogs    496     1          1      0      0      1       1         0        0     1
5 Aaron Brule Oklahoma State Cowboys        496     1          0      0      0      1       0         0        0     0
6 Aaron Brule TCU Horned Frogs              496     1          0      0      0      1       0         0        0     0
```

<p><br></p>


### The Prediction


In this first part, I'm going to see if we can accurately predict where a recruit will enroll based on how a school as interacted with him. Based on the sheer number of offers a recruit gets, our accuracy to be low, but we'll refine our approach next.


```{r}
model <-glm(won~rank+schoolCamp+uvisit+cvisit+ovisit+in_state,
            family=binomial(link='logit'),data=recruitX,
            control = list(maxit = 50))
summary(model)
                        
```


```{r}
Call:
glm(formula = won ~ rank + schoolCamp + uvisit + cvisit + ovisit + 
    in_state, family = binomial(link = "logit"), data = recruitX, 
    control = list(maxit = 50))

Deviance Residuals: 
    Min       1Q   Median       3Q      Max  
-3.0863  -0.1801  -0.1314  -0.1018   3.2537  

Coefficients:
              Estimate Std. Error z value Pr(>|z|)    
(Intercept) -5.6007843  0.1491958 -37.540  < 2e-16 ***
rank         0.0018943  0.0001954   9.695  < 2e-16 ***
schoolCamp   0.3327211  0.1557867   2.136   0.0327 *  
uvisit       0.3453559  0.0426382   8.100 5.51e-16 ***
cvisit      -0.0388794  0.0822575  -0.473   0.6365    
ovisit       4.2866280  0.1148228  37.333  < 2e-16 ***
in_state     1.2392349  0.1376934   9.000  < 2e-16 ***
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1

(Dispersion parameter for binomial family taken to be 1)

    Null deviance: 5217.9  on 11799  degrees of freedom
Residual deviance: 2634.6  on 11793  degrees of freedom
AIC: 2648.6

Number of Fisher Scoring iterations: 7
                        
```

```{r}
anova(model,test="Chisq")                     
```

```{r}
Analysis of Deviance Table

Model: binomial, link: logit

Response: won

Terms added sequentially (first to last)


           Df Deviance Resid. Df Resid. Dev  Pr(>Chi)    
NULL                       11799     5217.9              
rank        1    38.57     11798     5179.3 5.296e-10 ***
schoolCamp  1   214.83     11797     4964.5 < 2.2e-16 ***
uvisit      1   394.53     11796     4570.0 < 2.2e-16 ***
cvisit      1    39.80     11795     4530.2 2.814e-10 ***
ovisit      1  1817.28     11794     2712.9 < 2.2e-16 ***
in_state    1    78.30     11793     2634.6 < 2.2e-16 ***
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1                   
```





\\( P( y =1 | X) = h_{\theta(X)} = \sigma(\theta^{T}X \\)


Where the logit is:

<p><br></p>
\\(logit[\pi(X)] = \beta{1}X_{SC} + \beta_{2}X_{oVisit}+ \beta_{3}X_{uVisit}+ \beta{4}X_{cVisit}+ \beta_{5}X_{inState} \\)

<p><br></p>



<




Finally, I'm going to add School weights to see if schools take advantage of features. Particularly, I'm intereted in seeing if a School does a better job at keeping in-state talent, or if schools wow their prospects during their visists. 

To do this, I'm changing the original equation so that it looks like this...
