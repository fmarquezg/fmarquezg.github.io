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


### The Method

I'm going to use a Logistic Regression Model, which would output the probability of a recruit commiting to a School. 


\\( P(y =1|X) = h_{\theta(X)} = \sigma(\theta^{T}X \\)

Where the logit is:

<p><br></p>
\\(logit[\pi(X)] = \beta{1}X_{SC} + \beta_{2}X_{oVisit}+ \beta_{3}X_{uVisit}+ \beta{4}X_{cVisit}+ \beta_{5}X_{inState} \\)

<p><br></p>

I will begin with a general School agnostic model, and then move to a different model, where I compare Georgia, Ohio State, Texas, USC, Clemson, Penn State, Alabama, and Oklahoma. The top 8 recruiting classes of 2018.



### The Prediction


In this first part, I'm going to see if we can accurately predict where a recruit will enroll based on how a school as interacted with him. Based on the sheer number of offers a recruit gets, our accuracy to be low, but we'll refine our approach next.


{% highlight R %}
model <-glm(won~rank+schoolCamp+uvisit+cvisit+ovisit+in_state,
            family=binomial(link='logit'),data=training,
            control = list(maxit = 50))
summary(model)
                        
{% endhighlight %}


```{r}
Call:
glm(formula = won ~ rank + schoolCamp + uvisit + cvisit + ovisit + 
    in_state, family = binomial(link = "logit"), data = training, 
    control = list(maxit = 50))

Deviance Residuals: 
    Min       1Q   Median       3Q      Max  
-2.8487  -0.1834  -0.1303  -0.0996   3.2661  

Coefficients:
             Estimate Std. Error z value Pr(>|z|)    
(Intercept) -5.664591   0.194973 -29.053  < 2e-16 ***
rank         0.002035   0.000254   8.013 1.12e-15 ***
schoolCamp   0.276454   0.196053   1.410    0.159    
uvisit       0.339349   0.053661   6.324 2.55e-10 ***
cvisit       0.035264   0.105592   0.334    0.738    
ovisit       4.314356   0.149008  28.954  < 2e-16 ***
in_state     1.392162   0.178340   7.806 5.89e-15 ***
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1

(Dispersion parameter for binomial family taken to be 1)

    Null deviance: 3154.1  on 7079  degrees of freedom
Residual deviance: 1578.3  on 7073  degrees of freedom
AIC: 1592.3

Number of Fisher Scoring iterations: 7
                        
```

The output of thre regression, tells us Official visits have a huge impact on a recruit's decision. In State appears to be runner up, which makes sense as we know players want to stay close to their families. Coach Visits and School camps seem to have an impact in the recruiting process. This is schocking to me, I thought coaches visiting players would be a strong factor.


Next we analyse how factors reduce variance. In this case, the official visit factor comes out as a very strong indicator. 
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

Now the question.. how good is this model? Overall, accuracy is at 95.66%, but I must admit this data is heavily skewed towards not winning a recruit.

```{r}
predict<-predict(model,testing,type='response')
table(testing$won, predict > 0.5)
```

```{r}
    FALSE TRUE
  0  4374   77
  1   128  141   
```










<




Finally, I'm going to add School weights to see if schools take advantage of features. Particularly, I'm intereted in seeing if a School does a better job at keeping in-state talent, or if schools wow their prospects during their visists. 

To do this, I'm changing the original equation so that it looks like this...
