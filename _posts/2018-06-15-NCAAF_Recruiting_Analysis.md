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

Recruiting is probably the one of the most important factors, if not the most important factor that determines how good a season will be. With this in mind, I dug up some data from <a href="https://247sports.com/Season/2018-Football/" target="_blank">247Sports</a>.  to see if we could use public recruiting event data to see if we could predict where a player was committing to, or at the very least see if a school of (my) interest was doing a good job at recruiting.    



In this analysis I'm pulled the player timeline records from <a href="https://247sports.com/Season/2018-Football/" target="_blank">247Sports</a>. 

On average, recruits receive 13 offers. As we would expect, the higher the recruit is ranked, the more offers he might receive. In the chart below I grouped recruits by 100s according to their rank. 

<figure>
     <img src="/images/recruiting2018/Tier_offers.jpeg">
    <figcaption></figcaption>
</figure>

The offer letter isn't the only event. There are coaches visiting the student, students attending summer camps, and students visiting schools both officialy and unofficialy. 

<figure>
     <img src="/images/recruiting2018/Tier_events.jpeg">
    <figcaption></figcaption>
</figure>



Looking back at the 2018 class, Clemson, Georgia, Ohio State, Texas and USC finished in the top 5. 

<figure>
     <img src="/images/recruiting2018/School_events.jpeg">
    <figcaption></figcaption>
</figure>

Georgio putting 200+ offers is suspicious... let's look into that 






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
model <-glm(won~schoolCamp+uvisit+cvisit+ovisit+in_state,
            family=binomial(link='logit'),data=training,
            control = list(maxit = 50))

summary(model)
                        
{% endhighlight %}


```{r}
Call:
glm(formula = won ~ schoolCamp + uvisit + cvisit + ovisit + in_state, 
    family = binomial(link = "logit"), data = training, control = list(maxit = 50))

Deviance Residuals: 
    Min       1Q   Median       3Q      Max  
-2.8262  -0.1343  -0.1343  -0.1343   3.0703  

Coefficients:
            Estimate Std. Error z value Pr(>|z|)    
(Intercept) -4.70429    0.12495 -37.648  < 2e-16 ***
schoolCamp   0.52197    0.18353   2.844  0.00445 ** 
uvisit       0.23327    0.05058   4.612 3.99e-06 ***
cvisit      -0.13102    0.08994  -1.457  0.14518    
ovisit       4.22311    0.14542  29.040  < 2e-16 ***
in_state     1.29659    0.17052   7.604 2.88e-14 ***
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1

(Dispersion parameter for binomial family taken to be 1)

    Null deviance: 3143  on 7079  degrees of freedom
Residual deviance: 1620  on 7074  degrees of freedom
AIC: 1632

Number of Fisher Scoring iterations: 7                     
```

The output of thre regression...

Next we analyse how factors reduce variance. In this case, the official visit factor comes out as a very strong indicator. 
```{r}
Analysis of Deviance Table

Model: binomial, link: logit

Response: won

Terms added sequentially (first to last)


           Df Deviance Resid. Df Resid. Dev  Pr(>Chi)    
NULL                        7079     3143.0              
schoolCamp  1   148.13      7078     2994.9 < 2.2e-16 ***
uvisit      1   218.73      7077     2776.2 < 2.2e-16 ***
cvisit      1    21.46      7076     2754.7 3.621e-06 ***
ovisit      1  1078.37      7075     1676.3 < 2.2e-16 ***
in_state    1    56.36      7074     1620.0 6.037e-14 ***
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1                
```

Now the question.. how good is this model? Overall, accuracy is at 95.56%, but I must admit this data is heavily skewed towards not winning a recruit.

```{r}
predict<-predict(model,testing,type='response')
table(testing$won, predict > 0.5)
```

```{r}
    FALSE TRUE
  0  5486   44
  1   218  152
```


Finally, I'm going to add School weights to see if schools take advantage of features. Particularly, I'm intereted in seeing if a School does a better job at keeping in-state talent, or if schools wow their prospects during their visists. 

To do this, I'm changing the original equation so that it looks like this...
